---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者存在 Copilot 应该了解的背景信息，就需要确保这些内容可用。可以通过多种方式向 Copilot 提供具体上下文，其中最重要的是[说明文件][instruction-files]，它们会为代码生成提供指导。

本练习将完成以下任务：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在当前说明生效的情况下，生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能数据切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有非确定性。本练习的目标是观察更新说明后行为发生的*趋势变化*，而不是逐字符匹配输出。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明文件用途。

通过使用说明文件，可以确保 Copilot 获取正确信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可用于向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对存储库中**每次**请求发送给 Copilot 的单一说明文件。该文件应包含大多数聊天或 CLI 请求都会用到的项目级信息，例如所用技术栈、构建内容概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：用于为特定任务或文件类型提供指导，例如 TypeScript 或 Astro 等语言，也可以用于创建 UI 组件或一组单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明文件以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持采用其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

关于如何创建说明文件的完整讨论超出了本工作坊范围。不过，示例项目中的文件展示了一种具有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如构建内容说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法。通过不断尝试，可以找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。浏览本项目中的文件时，你会看到许多任务对应的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供此功能的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前执行环境的课程会在适用位置说明。
>
> 想寻找模板或起点？可以探索 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读存储库附带的说明文件，其中包括一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。在 **Code standards** 下，注意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览。可以看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都符合条件。
5. 阅读该文件中针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这样可以将大型说明集拆分成更小、可复用的文件，并为 Copilot 提供可参考的示例。（其中的路径相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 创建变更分支

遵循最佳实践，为变更创建一个分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的新对话。各练习使用的模式和模型可能不同；相关步骤会在适用位置说明。

为了观察自定义说明的影响，先在当前说明文件生效时发送提示。稍后更新说明文件后，再次发送相同提示进行比较。

1. 关闭之前练习中打开的编辑器选项卡，这样 Copilot 只会获取你希望提供的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，使 Copilot 能够应用文件变更。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意生成的 helper 是带类型的函数：第一个参数是 `db` 客户端，返回值是带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；更新说明后行为的*一致性*是否有所提升，才是本练习的重点。

## 添加新的全库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。该部分记录了项目的编码标准，但还没有关于代码内文档的规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以创建 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空当前缓冲区。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在生成的文件开头会有类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到后续练习将继续使用的真实代码中。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，都通过版本控制管理。现在提交变更并将分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 变更都已就绪：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为 helper 基础所做的每项变更：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他辅助更新（例如测试），也将这些文件添加进去。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并 pull request

分支已推送，接下来创建 pull request，并关联到待办列表中的文档标准 issue。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具完成。请 Copilot 查找 issue，创建用于关闭该 issue 的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub；按照提示授权即可。稍后会在另一课中了解这种连接方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认已选中 **Agent**。
3. 请 Copilot 找到与更新说明文件有关的 issue，并根据当前分支创建 PR；PR 应同时说明说明更新和即将用于筛选功能的 publishers helper：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 系统提示允许 Copilot 代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR，并将当前分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示允许 Copilot 代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在已在 GitHub Copilot 的帮助下创建并合并 pull request。

## 总结和后续步骤

你已了解 Copilot 如何从本项目中的说明文件获取上下文，并使用 VS Code 中的 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 如何根据*现有*说明生成代码。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和 helper 基础合并到 `main`，供下一练习继续使用。

接下来将使用[智能体模式][next-lesson]，让 Copilot 在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
