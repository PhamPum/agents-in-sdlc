---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你要编写什么代码，也说明代码应如何组织。现在你已实现筛选功能，接下来将探索 Copilot 使用的说明、添加文档规范，并将其应用于代码。

本课将学习如何：

- 探索存储库说明和按路径指定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码规范。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改后，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过说明文件，你可以让 Copilot 按照这些实践执行任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释习惯，并为整个存储库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

先查看入门项目中已有的说明文件：核心文件 `copilot-instructions.md`，以及针对不同任务的一组 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等章节。**Code standards** 中的 **GitHub Actions Workflows** 子章节适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中的 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它会设置相对于存储库根目录的 glob，确定说明适用的文件。例如，它会匹配 `**/*.test.ts` TypeScript 测试文件。
5. 查看该文件中针对单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分为可复用的小文件，并提供生成代码时可参考的示例。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting guidance** 章节记录了项目编码规范，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 更新说明文件，使其符合团队规范

自定义说明已介绍过 Copilot 如何获取上下文。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 遵循文档规范。

1. 打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting guidance** 章节，位置约在第 35 行。
3. 在最后一个条目后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的说明

Copilot CLI 会在新对话启动时加载存储库说明。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新说明。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的分支为 `game-filters-cli`。
3. 让 Copilot 应用更新后的说明：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何读取说明文件，并将新规范应用于功能。本课中，你：

- 探索了存储库说明和按路径指定的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码规范。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
