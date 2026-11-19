---
title: "レッスン 2 - 星評価を追加する：小さく効果的な変更"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示してもらい、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始める
- エージェントに小さな変更を依頼する
- `/diff` で変更をレビューする
- 転送されたブラウザーでアプリを実行し、変更を確認する
- 最初のプルリクエストを開いてマージする

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。まずエージェントに既存の評価をカードにも表示してもらいます。Copilot で最初のセッションを試すのに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree ではなく、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話には後で戻れますが、ファイルやブランチは通常の Git 状態としてディスク上にあります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集する際のツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を始めて変更を依頼する

新しい会話を始め、機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押します。
3. Copilot CLI をまだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲーム詳細を表示するファイルを探して、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューしてください。Copilot CLI で変更を確認します。

1. `/diff` を入力して、変更されたファイルをすべて確認します。
2. ゲーム カードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが完了したら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成コードは異なる場合があります。特定の実装と完全に一致することではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Codespaces で転送されたブラウザーから Web サイトを確認します。

1. 次のプロンプトを Copilot に送り、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲーム カードに 5 点満点の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを開いてマージする

機能を作成できたので、プルリクエスト（PR）を作成してプロジェクトにマージします。

1. 次のプロンプトで、デフォルト エージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更を公開しました。次のことを行いました。

- 機能ブランチで Copilot の会話を始めた
- ゲーム カードに小さな変更を依頼した
- `/diff` で変更をレビューした
- 転送されたブラウザーでアプリを実行し、星評価を確認した
- 最初のプルリクエストを開いてマージした

次は[フィルター issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を実装します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
