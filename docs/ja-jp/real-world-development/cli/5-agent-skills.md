---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズしてフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業は、コードを書くことだけではありません。手動で動作を確認し、指示ファイルを使って標準に従わせました。テスト、lint、継続的インテグレーション（CI）など、ほかの作業はどうでしょうか。

こうしたタスクには、**エージェント スキル**が適しています。スキルを使うと、Copilot がこれらの操作を正しい方法で実行できるようになります。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認する
- 結果の書式をカスタマイズする
- スキルを再読み込みして実行する

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースがまとめられています。基本的には、スキル名のフォルダーに `SKILL.md` という Markdown ファイルを置きます。Markdown には、スキルの内容を定義する `name` と `description` を含むフロントマター、機能の概要、呼び出す条件を記載します。必要に応じて、スクリプトなどのリソースをサブフォルダーに置くこともできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルはリポジトリの `.github/skills` に置いてチームで共有したり、ユーザーの `~/.copilot/skills` に置いたりできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックのために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭にある `name` と `description` を読みます。説明により、Copilot はスキルを呼び出すタイミングを判断できます。
3. 指示を読み、テストや lint の手順をどのように案内しているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot にスキルの 3 つのチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 完了時のレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかが分かるレポートにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。変更の効果を確認しましょう。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認した
- 結果レポートの形式をカスタマイズした
- スキルを再読み込みして実行した

この変更は、機能の PR に含めます。次は [Playwright MCP サーバー][next-lesson]を使い、Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー：`make-repo-contribution`][contribution-example]
- [要件ドキュメント：`prd`][prd-example]
- [図とエクスポート スクリプト：`drawio`][drawio-example]
- [ブラウザー テスト：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
