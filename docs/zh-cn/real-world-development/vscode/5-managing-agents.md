---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 在你的计算机上实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中查看每个差异、接受或撤销单项更改，并通过后续提示引导对话。

本练习将学习如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时引导智能体，在高对比度模式旁添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作过程中，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式。与其从头开始新对话，不如引导现有会话继续完成工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录并查看工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。它会显示一条消息，总结所做的更改。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中继续添加浅色模式开关。由于仍在同一个会话中，智能体会保留之前的上下文，包括已修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查已有更改，并提出浅色模式的实现。
3. 像处理高对比度模式一样审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不合适，例如尝试以不喜欢的方式重构开关界面，可使用聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 在正在进行的会话中引导智能体，比从头开始更快。可以用后续提示提出调整（例如“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（例如“不要添加第三个开关，将浅色模式集成到现有设置面板”）或扩展范围（例如“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件列表。应该可以看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认其中体现了无障碍自定义智能体的指导，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话，审查每项文件编辑、接受或撤销更改，并在保留上下文的情况下发送后续提示扩展工作。

本课探索了以下概念：

- 实时监控智能体工作。
- 接受或撤销单项文件编辑。
- 使用后续提示引导会话。

在[下一课][next-lesson]中，你将发布无障碍改进的拉取请求，并审查本实验中本地自定义智能体产生的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
