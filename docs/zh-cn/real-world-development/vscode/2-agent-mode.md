---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行操作，例如运行测试。开发人员需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件，再进行修改……这样的流程会不断重复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中自主工作。它会像开发人员一样，先探索现有项目结构，再进行必要的更新、运行测试等任务，并自动修复发现的问题。现在来看看如何使用智能体模式为网站添加功能。

> [!NOTE]
> 虽然名称相似，但智能体模式和 cloud agent 是两种不同的体验。智能体模式在 IDE 中执行任务，可以快速获得反馈并进行交互。Cloud agent 则像团队中的同事一样异步工作，通过 issue 和 pull request 与你协作。

本练习将介绍以下内容：

- Copilot 智能体模式如何探索项目、识别相关文件并协调多处变更。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 如何在将变更合并到代码库前审查 Copilot 智能体模式生成的变更和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已经添加 publishers helper，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改之前，先探索 Tailspin Toys 网站，了解它当前的功能。

这个网站是一个以开发者为主题的桌面游戏众筹平台。用户可以浏览游戏列表和详情。网站是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，不存在单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单体 Astro 开发服务器。在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一练习结束后，当前应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，提示为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端中的 localhost 链接时，系统会自动将你转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，现在你的网站服务器已在其中运行。

### 浏览网站

网站启动后，可以探索其功能。主要功能包括：

- **主页**：显示桌面游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的新对话。不同步骤使用的模式和模型可能不同；相关信息会在适用位置说明。

网站的初始实现可以正常运行，但还可以添加新功能。先查看待办列表。使用模板创建项目时，系统会自动创建一组 issue；请 GitHub Copilot 显示这些任务。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现变更、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者不偏好某个特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在说明中列出该模型，希望代码建议相对一致，带来良好体验。不过，大型语言模型具有非确定性，收到的建议可能与工作坊示例不同。这完全正常。

> [!NOTE]
> 由于大型语言模型具有非确定性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 存储库中的待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

注意，Copilot 还根据它认为对用户最有用的任务，对列表进行了排序。

## 查看说明文件

启动智能体生成代码前，先查看用于向 Copilot 提供上下文的说明文件。本练习将使用 [UI 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何向网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对特定文件类型的说明文件链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件以及项目中的文件。路径相对于说明文件所在位置。这种方式支持复用、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并变更前还必须运行并通过测试。Copilot 智能体模式可以帮你完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选择了 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍然是 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层 helper、UI 和测试。之后它会开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。完成后，Copilot 会为所有变更显示 **Keep** 或 **Undo**，因此工作进行期间无需选择这些按钮。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问用户是否同意运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至有测试失败。这没有关系！Copilot 会反复生成代码和运行测试，直到任务完成且没有发现错误。

   ![Copilot 智能体模式完整聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找可能存在的问题。

> [!CAUTION]
> 请务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习探索了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了以下内容：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调多处变更。
- 如何在将变更合并到代码库前审查 Copilot 智能体模式生成的变更和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建 pull request。

### 延伸探索——实现分页

随着游戏列表增长，需要添加分页。使用本练习学到的技巧，请 Copilot 更新网站以实现分页。编写提示和代码时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页的实现方式，例如允许用户选择每页数量，或使用固定数量。
- 编写提示时，应提供足够指导，让 Copilot 按照期望实现功能。
- 可能需要反复与 GitHub Copilot 协作，请求修改并补充上下文。这是使用 Copilot 的常见流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
