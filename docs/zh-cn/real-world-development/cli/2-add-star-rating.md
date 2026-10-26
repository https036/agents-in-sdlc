---
title: "第 2 课 - 添加星级评分：快速见效"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查变更，并将其合并为第一个 pull request。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过一次对话后，就可以开始对项目进行第一个变更了。先从简单的小改动入手：游戏数据中已经包含星级评分，但主页上的游戏卡片尚未显示评分。请智能体添加评分，然后审查并合并变更，完成第一个 pull request。

本课将完成以下任务：

- 在功能分支上启动一个专注于当前任务的 Copilot 对话。
- 请智能体对项目进行小幅修改。
- 使用 `/diff` 审查变更。
- 运行应用，在转发的浏览器中确认变更。
- 创建并合并第一个 pull request。

## 场景

Tailspin Toys 中的每款游戏都可以设置星级评分，而且评分已经显示在游戏详情页中。不过，主页的游戏卡片目前只显示标题、类别、发行商和说明。作为热身练习，请智能体在每张卡片上显示现有评分。这个小改动范围明确，很适合作为首次会话的任务。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作空间。与 Copilot app 不同，普通的 CLI 对话会使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话让你可以稍后返回同一段交流；文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示三类内容：你发送的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可使用 `/diff` 检查的变更。

## 开始对话并请求变更

现在开始一个新对话，实现这项功能。

1. 返回 Codespace。
2. 如果终端没有保持打开状态，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot CLI 尚未运行，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动了新会话。
5. 使用以下提示请求变更：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。这样就通过 Copilot CLI 为项目添加了一项功能。

## 审查差异

所有 AI 生成的变更——即使很小——在合并前都应审查。现在就在 Copilot CLI 中查看这些变更。

1. 输入 `/diff`，检查每个已更改的文件。
2. 确认游戏卡片会在评分存在时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 审查 Copilot 运行的检查结果，并让它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此实际代码可能有所不同。请审查行为，不要期待实现与某个固定示例完全一致。

## 检查变更

不能只读代码就假设功能可用。请 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的界面。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示的评分为满分五分制。
4. 返回 Copilot，请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个 pull request

现在功能已经完成，可以创建 pull request（PR），将代码合并到项目中。

1. 请默认智能体提交变更：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。创建 PR 时，Copilot CLI 会推送已有提交并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查已更改的文件和检查结果。
5. 确认无误后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，你已使用 GitHub Copilot CLI 完成并交付第一个变更。具体来说，你：

- 在功能分支上启动了一个专注于当前任务的 Copilot 对话。
- 请智能体修改了游戏卡片。
- 使用 `/diff` 审查了变更。
- 运行应用，在转发的浏览器中确认了星级评分。
- 创建并合并了第一个 pull request。

接下来将[从筛选 issue 开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更复杂的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
