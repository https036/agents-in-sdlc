---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 智能体模式后，它会自主探索代码库、提出变更、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以内嵌审查差异、接受或拒绝单项变更，也可以发送后续提示，在不离开编辑器的情况下调整或扩展工作。

本练习将完成以下任务：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，同时添加浅色模式切换功能。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作尚未完成时，设计团队又提出了一个要求：为觉得默认主题太暗的用户添加浅色模式切换功能。与其从头开始新的对话，不如引导现有会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以差异形式内嵌在聊天中，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录，观察每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内嵌显示：
   - 选择 **Keep** 接受变更。
   - 选择 **Undo** 撤销变更，让智能体重新处理。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本会话中修改的完整工作集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。完成后会看到总结变更的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一个对话中继续添加浅色模式切换功能。由于使用的是同一个聊天会话，智能体会保留前面工作的所有上下文，包括已修改的组件、采用的样式方案和配置的 local storage 模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续前面的工作，先探索已有变更，再提出浅色模式的补充修改。
3. 像审查高对比度变更一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期，例如打算以你不喜欢的方式重构切换 UI，可以使用聊天输入框旁的 **Stop** 按钮停止它，然后发送更明确的后续消息。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS——使用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板”），或扩展范围（“为两个切换开关都添加键盘快捷键”）。

## 审查变更集

提交工作前，快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查完整的已更改文件列表。应能看到 Astro 组件、样式和相关测试的变更。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体所要求的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和通过 local storage 保留设置。
4. 确认无误后，在 Source Control 面板中暂存并提交变更。下一课会发布这些变更。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并引导正在运行的会话：逐项审查文件编辑、接受或拒绝变更，并发送后续提示扩展工作，同时保留已有上下文。

本练习探索了以下内容：

- 实时监控智能体的工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

接下来将在[下一课][next-lesson]中发布无障碍功能 PR，并审查本实验中本地自定义智能体生成的变更。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代完善 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
