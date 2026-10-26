---
title: "练习 5：迭代完善 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你已使用 GitHub Copilot 完成多项改善用户体验和添加功能的任务：请 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现高对比度和浅色模式切换等无障碍功能。现在查看代码变更；如果有必要，也可以向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍然需要审查生成的代码，并遵循常规 DevOps 流程。现在先查看 GitHub Copilot 对代码文档、相关游戏功能和无障碍功能的建议，再为团队其他成员启用审查。

## GitHub Copilot cloud agent 的安全性

Copilot cloud agent 会异步且无人监督地执行任务，因此系统采取了一些安全措施：

- Copilot 只能读取存储库，并且**只能**写入它用于修改代码的分支。
- Cloud agent 在 GitHub Actions 中运行，会创建单独的临时环境来处理任务。
- 任何 GitHub Actions 工作流都必须经人工批准才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 pull request（PR），其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问许多外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 在 GitHub.com 上打开自己的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 或更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。此时请等待 Copilot 完成。可能需要几分钟，可以趁此休息一下或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查变更。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新创建的 TSDoc 文档注释和其他文档。具体变更会因情况而异。

    查看时，注意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自你在练习 2 开始时查看的自定义说明文件。

6. 审查完成且一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)
9. 工作流应会在 PR 的检查区域中排队。顺利的话，单个 Astro 应用的项目检查会全部通过。可能需要几分钟才能完成。

## 请求 GitHub Copilot 修改代码

与 Copilot 协作并非单向过程。你可以像联系团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，无法规定唯一的提示内容。可以考虑要求 Copilot：

- 在每个代码文件顶部添加简短的文件注释头，说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用结构。

1. 添加评论，请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他你希望添加到代码库的文档要求。
2. 选择 **View Session**，观察 Copilot 如何完成工作。注意它会启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回 PR。

    ![Back to pull request 按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成变更后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查变更。

可以继续迭代，直到对结果满意。满意后，可将 PR 从草稿状态转换为准备审查，并合并到主分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 在 GitHub.com 上打开自己的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

8. 工作流应会在 PR 的检查区域中排队。顺利的话，单个 Astro 应用的项目检查会全部通过。可能需要几分钟才能完成。
9. **可选：**可以切换到 Codespace 中的这个分支，手动测试相关游戏功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能。和之前一样，可以继续向 Copilot cloud agent 请求更新。例如，可以调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（请记住，分配该任务时还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及在练习 4 的 mission control 中追加的浅色模式。

1. 在 GitHub.com 上打开自己的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现时，重点查看：
   - 用于切换模式的 UI 组件
   - 用于保存用户偏好的 local storage
   - 高对比度和浅色模式对应的 CSS 或样式变更
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 审查完成且一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 PR 的检查区域中排队。顺利的话，单个 Astro 应用的项目检查会全部通过。可能需要几分钟才能完成。
10. **可选：**可以切换到 Codespace 中的这个分支，手动测试无障碍功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置会在页面重新加载后保留。

无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像对前面的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习——继续委派任务

Cloud agent 最适合处理真实待办事项，让你可以转而处理其他工作。可以在存储库中再创建一些 issue 并分配给 Copilot，练习这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 执行环境的工作坊课程。通过这些课程，你：

- **审查了存储库附带的自定义说明文件**，了解它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，观察它设置环境、规划并异步执行任务。
- **创建并使用了自定义智能体**，添加高对比度和浅色模式切换功能。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导正在运行的无障碍会话。
- **审查并迭代了 cloud agent 创建的 pull request**，通过标记 `@copilot` 请求变更并批准工作流。

## 回顾和后续步骤

Cloud agent 执行环境的工作坊已经完成。如果还想继续探索，其他执行环境的工作坊可以补充这里学到的内容：

- 🖥️ **[VS Code 执行环境](../../vscode/)**——在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 执行环境](../../cli/)**——在终端中交付两项经过审查的变更，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接流程。

也可以在自己的存储库中尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
