---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应让它能够获取这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义说明和按路径指定的说明文件，将项目上下文、编码指南和文档规范提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较更新说明前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示差异。后续审查云智能体创建的拉取请求时，你将看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须有单元测试。
- 用户界面应采用深色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加注释块，说明该文件的用途。

通过说明文件，可以确保 Copilot 按照这些实践完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库和注释习惯，并为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目简介、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体都会使用存储库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

创建说明文件的完整讨论超出了本练习的范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码示例说明推荐和不推荐的写法。

说明文件没有唯一的正确写法，与使用 AI 一样，需要通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应维护完善的说明文件。浏览当前项目时，你会发现其中有用于[界面更新][ui-instructions]和 [Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式各有不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本学习路径会在相关课程中介绍对应操作。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间查看本存储库随附的说明文件：核心文件 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 下的 **GitHub Actions Workflows** 指南适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中的 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它会设置相对于存储库根目录的 glob，确定说明适用的文件。这里会匹配 TypeScript 测试文件，例如 `**/*.test.ts`。
5. 查看该文件中针对单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成可复用的小文件，并提供生成代码时可参考的示例。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码规范，但尚未要求添加代码文档。接下来将添加 TSDoc 注释和文件注释头的规则。

## 添加新的存储库规范

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码规范，提升代码建议质量。

在编辑前，先创建分支。云智能体会读取议题目标分支中的说明，你也可以在独立分支上提交更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于提供项目级信息。添加一条规范，要求代码包含 TSDoc 注释和文件注释头。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，通常靠近第 27 行。查看现有编码规范；其中尚无代码文档要求，因此之前生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所示，可以在存储库级文件 `.github/copilot-instructions.md` 中添加全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供说明。项目级规范（例如文档注释规则）适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中将从 `main` 分支为 Copilot 分配议题，因此必须先将此更改合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中分配议题时，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取项目中的说明文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。接下来，你会在云智能体创建的拉取请求中看到该规范的作用。

下一步，将[首个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
