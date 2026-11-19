---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应让它能够获取这些上下文。[说明文件][instruction-files]可以为代码生成提供指导。

本练习将学习如何：

- 通过存储库自定义说明和按路径指定的说明文件，将项目上下文、编码指南和文档规范提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 重新运行同一提示，观察生成的代码如何遵循新规范。
- 将说明更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有规范，因为 Copilot 具有非确定性。本练习的重点是观察更新说明后行为的整体变化，而不是要求输出与示例完全相同。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须有单元测试。
- 用户界面应采用深色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加注释块，说明该文件的用途。

说明文件可以为 Copilot 提供这些要求，使其按项目实践执行任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库和注释习惯，并为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体都会使用存储库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码示例说明推荐和不推荐的写法。

说明文件没有唯一的正确写法，与使用 AI 一样，需要通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应维护完善的说明文件。你会发现本项目中有用于[界面更新][ui-instructions]和 [Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式各有不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本学习路径会在相关课程中介绍对应操作。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看本存储库随附的说明文件：核心文件 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 下的 **GitHub Actions Workflows** 指南适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中的 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置相对于存储库根目录的 glob；这里会匹配 `**/*.test.ts` 等 TypeScript 测试文件。
5. 查看该文件中针对单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成可复用的小文件，并提供生成代码时可参考的示例。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码规范，但尚未要求添加代码文档。接下来将添加 TSDoc 注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，创建干净的新对话。不同练习所需的模式和模型可能不同；相关步骤会在适用时说明。

先在当前说明下发送提示，然后更新说明并重新发送相同提示，观察生成结果的变化。

1. 关闭之前练习中打开的编辑器标签页，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数和测试。
6. 注意生成的辅助函数是带类型的函数：它接受 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此它也可能在没有明确说明时添加文档注释。如果发生这种情况也没有问题；本练习关注的是更新说明后行为的一致性改进。

## 添加新的存储库规范

`.github/copilot-instructions.md` 用于提供项目级信息。现在添加一项编码规范，提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，通常靠近第 27 行。现有规范尚未包含代码文档要求，这就是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下内容，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级文件 `.github/copilot-instructions.md` 适合放置项目级规范；`*.instructions.md` 则适合提供特定语言、文件类型或任务的说明。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件开头会包含类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在也包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目规范，并将其应用到下一练习将继续使用的真实代码中。

## 提交说明并推送分支

说明文件和其他资源一样，也需要通过版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也要暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联待办事项中的文档规范议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建并合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经配置了 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub，请按提示完成登录。稍后的练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认已选择 **Agent**。
3. 要求 Copilot 找到与说明文件更新有关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作时，先审查命令，再酌情批准。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令时，先审查命令，再酌情批准。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何读取说明文件，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 重新发送相同提示，观察生成结果如何遵循新规范。
- 将说明更新和辅助函数基础提交到 `main`，供下一课使用。

接下来，在[代理模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
