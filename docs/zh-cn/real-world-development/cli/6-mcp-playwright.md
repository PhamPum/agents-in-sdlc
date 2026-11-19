---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还需要处理数据、外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol（MCP）以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加该服务器。
- 让智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要实际与界面交互。你可以让 Copilot 像用户一样操作网站，进一步自动验证更改是否符合预期。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以实时访问信息并代表你执行操作。

MCP 工具和资源通过 MCP 服务器访问。MCP 服务器充当智能体与外部工具及服务之间的桥梁，每个服务器代表一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，方便发现和参与 MCP 生态系统。

> [!CAUTION]
> 请像对待项目的其他依赖项一样对待 MCP 服务器。使用前应审查源代码、核实发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 可访问的 Playwright MCP 服务器执行同样的操作。

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自动找到 MCP 服务器，不一定需要明确要求使用它。不过，指出要使用的工具有助于保持结果一致，也能减少一些令牌消耗。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器，并提供报告。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器，从 Copilot CLI 中通过真实浏览器体验了功能。回顾一下，你：

- 了解了 Model Context Protocol（MCP）以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了该服务器。
- 让智能体驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具结合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
