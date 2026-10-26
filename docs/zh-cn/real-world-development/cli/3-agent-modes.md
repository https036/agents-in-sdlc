---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实现方案、使用 Autopilot 根据 issue 构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们从向项目添加一项小功能开始。较大的变更则需要更稳健的流程。幸运的是，GitHub Copilot CLI 能融入组织现有的开发流程，帮助我们用正确的方式构建正确的内容。在本系列课程中，你将遵循典型的智能体驱动开发流程：从 issue 开始生成新功能，确保代码有效、行为符合预期，并最终成功合并。

> [!NOTE]
> 后续功能工作将继续使用同一段对话和分支。通常会为不同文件类型创建不同分支或 PR，但为了聚焦核心概念，本工作坊会采用简化流程。

本课将完成以下任务：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程会继续完善这项功能：更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办列表中的 issue 描述了这项功能，但按多种类别组合筛选等细节需要在编写代码前达成一致。你将先用 Plan 模式确定这些决策，再授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变软件开发的基本原则。事实上，这些原则变得更加重要。大多数开发人员遵循的流程大致如下：

1. 从已提交的 issue 开始，明确要完成的工作。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建 pull request（PR）。
7. 代码通过审查且持续集成检查成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过完整的审查和验证。

## 对话模式

**对话模式**用于控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出变更建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并反复迭代，直到认为任务完成。

先在 Plan 模式中审查计划，然后使用 Autopilot 实现。

## 从 issue 开始

开始筛选功能前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

注意，提示现在会以 `#7`（或类似编号）开头。`#` 可用于将 GitHub 上的 issue 或 pull request（PR）添加到上下文中。

## 规划筛选功能

规划可以在开始实现功能或执行任务之前，先确定采用什么方案。复杂任务通常值得花时间规划。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在前一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案并制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答；没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查完毕，接下来开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

和其他代码一样，生成的代码也应在合并前审查。现在查看代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 以及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并让 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但实际能否运行？像前一课一样启动应用，然后通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商及计划中约定的组合进行筛选。
4. 确认重置和无结果状态符合 issue 和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。本课完成了以下任务：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来将[使用自定义说明][next-lesson]，进一步确保生成的代码遵循已记录的规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
