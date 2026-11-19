---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルター機能をテストします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したとおり、コードを書く作業はコードを書くことだけではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加することもあります。MCP サーバーを使うと、CLI に組み込まれた機能だけでなく、Copilot が利用できるツールやサービスを増やせます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学ぶ
- まだ利用できない場合は Playwright MCP サーバーを追加する
- エージェントにブラウザーを操作させ、フィルター機能を確認する

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot が作業中の Web サイトを使えるようにし、変更を自動で確認して、期待どおりに動作する確度を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。AI エージェントが MCP を使って外部ツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

エージェントは、外部ツールやサービスとの橋渡しをする MCP サーバーを通じて、これらのツールやリソースにアクセスします。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**：GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**：Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の発見や貢献を促進するために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同じように扱ってください。利用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、この手順をスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を選択し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは手動で機能を確認しました。次は Copilot が使える Playwright MCP サーバーで同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に見つけるので、明示的に使うよう伝える必要はありません。ただし、使うべきサーバーが分かっている場合は、指示するとよいでしょう。結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、レポートを表示します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。内容を振り返ります。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学んだ
- まだ利用できない場合は Playwright MCP サーバーを追加した
- エージェントにブラウザーを操作させ、フィルター機能を確認した

次はスキルとブラウザー ツールをまとめた [QA カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
