---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者存在 Copilot 应该了解的背景信息，就要确保这些内容可供 Copilot 使用。[说明文件][instruction-files]可以提供此类指导，让 Copilot 不仅理解你希望它完成*什么*，也理解应当*如何*完成。

本练习将介绍：

- 项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 执行环境不同，这里不会运行“修改前/修改后”提示。Copilot cloud agent 会异步处理 GitHub issue，因此不容易实时对比效果。之后审查 cloud agent 创建的 pull request 时，就能看到说明文件的影响。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件顶部都应添加注释块，说明文件用途。

通过使用说明文件，可以确保 Copilot 获取正确信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可用于向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对存储库中**每次**请求发送给 Copilot 的单一说明文件。该文件应包含大多数聊天或 CLI 请求都会用到的项目级信息，例如技术栈、构建内容概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：用于为特定语言（例如 TypeScript 或 Astro）、特定任务（例如创建 UI 组件）或文件类型提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明文件和带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot [还支持采用其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

完整讨论说明文件的创建方法超出了本工作坊范围。不过，示例项目中的文件展示了具有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 的内容聚焦于项目级指导，例如构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件，为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

说明文件没有唯一正确的创建方式，使用 AI 也没有唯一正确的方法。通过不断尝试，找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。浏览本项目文件时，你可能会看到许多任务类型对应的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供此功能的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关课程会在适用位置介绍。
>
> 想寻找模板或起点？可以探索 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件，其中包括一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。在 **Code standards** 下，还要留意嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览。可以看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它会设置一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都符合条件。
5. 阅读该文件中专门针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目文件的链接。将大型说明集拆分成更小、可复用的文件，也可以为 Copilot 提供生成代码时参考的示例。（这些路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的全库标准

接下来是本课唯一需要编辑的内容：添加一条项目级规则，要求通过 TSDoc 文档注释和文件级注释头来记录代码。之后分配给 cloud agent 的 issue 会使用这条规则。

修改前，请先创建用于工作的分支（cloud agent 会读取 issue 目标分支中的说明，但仍应提交自己的修改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。注意此处记录了项目编码标准，但尚未要求添加代码文档，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在存储库级别（`.github/copilot-instructions.md`），用于提供全局指导；也可以创建 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

Cloud agent 会从 issue 目标分支读取说明。在下一课中将 issue 分配给 Copilot 时，它会从 `main` 分支创建分支，因此说明更新必须先合并到 `main`，cloud agent 才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上从 `custom-instructions-cloud` 向 `main` 打开 pull request 并合并。这样 Copilot cloud agent 才能在下一课处理 issue 时读取这些说明。

> [!TIP]
> 如果更希望直接在 `main` 分支上完成本工作坊，可以跳过分支步骤并直接提交。这里创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的全库标准。后续课程中，Copilot cloud agent 创建的 pull request 会体现这项标准。

接下来将[把第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
