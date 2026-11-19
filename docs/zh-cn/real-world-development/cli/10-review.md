---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了已创建的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了首个拉取请求（PR）。
- 从筛选议题开始，使用计划模式确定方案、自动驾驶模式构建功能，并在交互模式下审查。
- 使用自定义说明引导智能体，自定义现有的 `quality-checks` 技能并运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包括两个 PR 阶段，每个阶段都基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为了简化研讨会，我们将这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 汇总了这些结果，并在创建 PR 前审查需求和测试覆盖范围。

新增测试应解决实际缺口；如果 QA 检查后认为不需要新增测试，也可能是合理结果。授权合并前，请审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本研讨会中，你更新了说明、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明定义规范，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用**计划模式**先考虑实现方案，使用**交互模式**处理需要持续协作的更改，并使用**自动驾驶模式**处理范围明确的任务。常规编辑可选择较快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响结果。

## 继续探索

你已完成核心工作流。Copilot CLI 还有一些值得了解的功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得不同视角。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离其他任务。
- `/delegate`：将任务交给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的好方法。用它处理生产代码、个人项目，或一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端继续扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
