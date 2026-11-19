---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン：Agent mode][previous-lesson] · [次のレッスン：カスタム エージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol（MCP）とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学ぶ
- Playwright MCP サーバーをワークスペースに追加する
- Copilot にサーバーを使って、ブラウザーでフィルター機能を手動テストしてもらう
- ブランチを公開し、フィルター機能のプルリクエストを作成する

## Model Context Protocol (MCP) とは

エディターの外部にアクセスできると、agent mode はさらに便利になります。Copilot が外部のツールとやり取りするための標準的な方法が Model Context Protocol (MCP) です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP により、AI エージェントはリアルタイムに外部ツールやサービスとやり取りでき、最新情報（リソース）にアクセスしたり、ユーザーに代わって操作（ツール）を実行したりできます。

これらのツールやリソースへのアクセスは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて行います。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールと AI エージェントとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーがツールやリソースへのアクセスを提供しています。GitHub は、エコシステム内の発見や貢献を促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。利用前にソース コードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習では Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使う際に GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。今回の機能テストに使用します。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザーを画面に表示せずに実行します。画面を表示するデスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストにすでに Playwright を使っているため、必要なブラウザーはインストール済みの場合がほとんどです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使う際に VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` の設定の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するよう求められた場合は、設定を確認して信頼する操作を選択し、サーバーを起動します。

手動で起動する必要はありません。スキップすると、次の手順で Copilot が初めてツールを使う際に VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、観察内容をまとめます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使って操作します。ボタン、リンク、リスト項目など、ラベル付きの構造化要素を使ってエージェントが判断します。支援技術と同様にページを認識するため、簡単な機能チェックがアクセシビリティの確認にもなります。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルター機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、手順を実行して確認内容を報告します。結果を issue の受け入れ条件と照らし合わせます。動作に問題がある場合は、追加の質問をしたり、プルリクエストを作成する前にコードの修正を依頼したりします。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリを実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームのレビューに向けてプルリクエスト（PR）を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス（CLI）など、プルリクエストの作成方法はいくつかあります。GitHub Copilot を使って、関連する issue を探し、関連付けた PR を作成してもらいましょう。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを使って PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の案内に従います。
5. Copilot が issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択してプルリクエストを確認します。**まだマージしないでください。**

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが新しいツールで Copilot を拡張する方法を学ぶ
- Playwright MCP サーバーをワークスペースに追加する
- サーバーを使って、公開前にブラウザーでフィルター機能をテストする
- ブランチを公開し、フィルター機能のプルリクエストを開く

機能のテストに MCP サーバーを使いました。Copilot を拡張する方法は MCP だけではありません。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどに特化したタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
