---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有创建了未关闭拉取请求（PR）的智能体任务，了解已执行的操作，也可以引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 在会话进行时引导智能体添加功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队发现也应该添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此同时加入浅色模式很合理。现在引导智能体，确保它同时实现高对比度和浅色模式。

## 查看 Copilot 云智能体任务

先查看所有已分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
   - 为代码库添加文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 注意右侧显示的 PR 面板。你可以在这里查看 PR 和更改的文件，以便继续监控。

## 引导云智能体

查看当前活动任务后，要求 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择负责添加高对比度模式的会话。会话标题可能因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始实际工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

整个过程需要几分钟。等待时，可以回顾并思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有活动和已完成的任务、审查工作，也可以在会话进行时调整范围或提供更多指导。

本课探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
