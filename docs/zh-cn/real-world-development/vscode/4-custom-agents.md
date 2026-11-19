---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域创建的专用 AI 助手。通过在存储库 `.github/agents` 文件夹中定义 Markdown 文件，可以为 Copilot 提供具体指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段；后面是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体和智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者通常都通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以简单理解为：**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体拥有自己的上下文窗口，并可在工作过程中协调技能，甚至协调其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站，并可调用拉取请求工作流或测试相关技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。与 AI 的其他用法一样，需要在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期的用户反馈指出，有些用户觉得当前深色主题难以阅读，因为文本和背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

本练习将学习如何：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已经为无障碍工作创建了自定义智能体。先查看文件，了解它将如何引导 Copilot。

返回 Codespace，然后为无障碍功能创建新分支：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 查看后续章节，其中说明：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该会话会使用智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace 并在 Copilot Chat 面板中选择 **New Chat**，创建新的对话。不同练习所需的模式和模型可能不同；相关步骤会在适用时说明。

1. 如果尚未选择，请在 Chat 视图的智能体下拉列表中选择 **Agent**。
2. 选择 Chat 视图底部的智能体下拉列表（默认显示活动智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，包含文件路径和可审查的差异。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一课将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域设计的专用 AI 助手。借助自定义智能体，可以将团队专业知识和规范整理成可复用的角色，引导 Copilot 更有效地完成工作。

本课探索了以下概念：

- 自定义智能体的定义方式。
- 在 Copilot Chat 代理模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
