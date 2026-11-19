---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン：カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を委任して開発プロセスを加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲の issue が重要な理由
- Copilot への issue の割り当て

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した委託先が、ドキュメントのない状態で作業を終えました。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別タイトルなど、関連ゲームを表示します。完成度の高い実装はまだ必要ありませんが、UX の受け入れテストに使える状態にしたいと考えています。この作業は現在ブロッカーですが、優先度がより高い issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業を進める間に非同期で処理できます。その後、作業を確認して期待どおりの結果になっているかをレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを処理します。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取り組みます。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を明確にする][cloud-agent-best-practices]ことが重要です。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整してください。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその役割を確認しました。カスタム指示の演習をすでに完了した場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、これを機会にファイルを確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で指示の影響を確認できるようにします。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、`**/*.test.ts` ファイルに指示が適用されることを確認します。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てると、後で PR をレビューする際に、これらの指示に由来する TSDoc コメント、TypeScript 規約、コメント ヘッダーを確認できます。後の演習で PR をレビューする際に、改めて説明します。

## Copilot cloud agent の開発環境を設定する

コードを作成する際は、通常、環境を準備して必要なスクリプトを実行します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係をインストールできます。このラボの流れを円滑にするため、必要な設定は事前に用意されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、SQLite データベースのマイグレーションとシードを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローに似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されているため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずにワークフローの成功を確認できるので、テストに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトもあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR がない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかる場合があります。前述のワークフローを実行して環境を準備し、プロジェクトを分析して、問題への対処方法を検討しているためです。

13. todo リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて判断し、コードを作成するところまで確認できます。

これには数分かかります。Copilot cloud agent の主な目的の一つは、タスクを非同期に実行し、開発者が別の作業を進められるようにすることです。2 つ目のタスクを cloud agent に割り当て、その間にアプリケーションの機能を追加するコードを作成しましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チームからの依頼にすぐ対応できる場合でも、その時間を別の作業に使えます。関連ゲームのセクションの追加を cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue に取り組んでいることを目の絵文字で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、積極的に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期に作業を進めてもらえます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする
- 操作を安全に行う
- issue の範囲を明確にすることの重要性
- Copilot に issue を割り当てる

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使いましょう。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン：カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
