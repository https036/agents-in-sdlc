---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果某项任务需要按特定方式完成，就应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅说明你希望生成*什么*代码，还会说明代码应当*如何*组织。筛选功能已经完成，现在来了解 Copilot 使用了哪些说明、添加文档标准，并将新标准应用于代码。

本课将完成以下任务：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的设计决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数包含可注入的 `db` 参数，也应说明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持更新。
- 保留现有的格式和 lint 指导。

通过使用说明文件，可以确保 Copilot 获取正确信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可用于向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对存储库中**每次**请求发送给 Copilot 的单一说明文件。该文件应包含大多数请求都会用到的项目级信息。
- `.github/instructions/*.instructions.md`：用于为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明文件格式的支持情况各不相同。依赖某种格式前，请先查阅[自定义说明支持参考][custom-instructions-support]。

## 探索本项目中的自定义说明文件

起始项目已经包含了一组说明文件。先查看现有文件，再进行修改，以便观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而不是终端中）打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件中的说明适用于 Copilot 在本存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。可以看到面向 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和项目文件。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供可参考的示例。

## 根据团队指导更新说明文件

已有文件是一个不错的起点，但还需要补充一项要求。修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 将以下内容作为最后一条项目符号添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在会话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能会话，这样 Copilot 就会加载新指导，同时保留功能上下文。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。具体来说，你：

- 了解了存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试得到一致执行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
