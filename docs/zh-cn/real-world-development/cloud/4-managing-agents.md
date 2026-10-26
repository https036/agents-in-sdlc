---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你已让 Copilot cloud agent 处理了三项任务，重点是改善用户体验和添加功能。Cloud agent 虽然会异步、自主地工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在这个任务控制中心，可以查看所有有未关闭 pull request（PR）的智能体任务，了解已执行的操作，也可以引导正在运行的会话。

本课将完成以下任务：

- 探索 agents 页面，监控 cloud agent 任务。
- 引导正在运行的会话，为其添加额外功能。

## 场景

让智能体添加高对比度模式后，团队意识到也可以顺便添加浅色模式。既然已经在更新网站样式并添加切换功能，加入浅色模式也很合理。现在引导智能体，让它同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到已分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。这些操作可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和处理方式而有所不同。

4. 同时留意右侧显示的 pull request（PR）窗格。这里可以查看 PR 和已更改文件，以便进一步监控。

## 引导 cloud agent

了解当前活动任务后，请 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式有关的会话。具体标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到它显示已完成设置并开始工作。出现类似下方的消息时，表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents 页面中的 cloud agent 任务截图，突出显示引导正在运行的会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成

和之前一样，Copilot 会开始处理更新后的任务。它会在完成发送提示时正在进行的步骤后，将新要求纳入工作流程。

与之前一样，这需要几分钟。你可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课探索了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。在此可以跟踪所有活动和已完成任务，审查正在进行的工作，也可以调整正在运行的任务范围或提供其他指导。

本课介绍了以下内容：

- 使用 mission control 和 agents 页面监控 cloud agent 任务。
- 引导正在运行的会话，为其添加额外功能。

Copilot 正在完成功能更新。接下来可以[迭代 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
