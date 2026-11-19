---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、省略可能な cloud agent への委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI には対話に使うスラッシュ コマンドが多数あります。高度な機能、内部の情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って Copilot が会話のコンテキストを管理する方法を確認する
- `/model` を使って利用可能なモデルを確認する
- `/share` でセッションをエクスポート、共有する方法を学ぶ
- 並列作業、worktree、cloud agent への委任など、省略可能なコマンドを確認する

## シナリオ

CLI の主なワークフローを完了しました。次にコンテキストの管理、モデルの切り替え、セッションの共有など、追加機能を確認します。また、省略可能な機能として [Copilot cloud agent][about-cloud-agent] への委任も試せます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルが処理できるコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って、状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関係のない別タスクに切り替えて、新しい会話を始めたい場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルに Markdown をエクスポートします。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 省略可能：並列実行や委任を使う

ワークショップの主要な作業は完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送り、非同期で処理してプルリクエストを作成できるようにします。

これらのコマンドは worktree やリモート作業を作成することがあるため、省略可能にしています。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部で何が起きているかを確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って会話コンテキストの管理方法を確認した
- `/model` を使って利用可能なモデルを確認した
- `/share` でセッションをエクスポート、共有する方法を学んだ
- 並列作業、worktree、cloud agent への委任など、省略可能なコマンドを確認した

ほかにもスラッシュ コマンドがあります。次は、学んだことを[振り返り][next-lesson]、さらに学ぶための手順を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
