---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都已保存在同一分支上。现在将它们一并审查，然后打开 pull request。星级评分 PR 是你手动合并的；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到多个 PR 中。为了简化工作坊流程，筛选和质量工作流都保留在同一段对话和分支中，并一起放入这个 PR。

本课将完成以下任务：

- 了解 Agent Merge 及其如何自动管理合并生命周期。
- 检查完整的功能变更和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你已使用 Copilot 规划、实现并验证功能。Tailspin Toys 现在希望自动处理剩余的 PR 工作，同时由开发人员掌握合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并 pull request 所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项——修复失败的持续集成（CI）检查、回复审查意见，并在需要时进行变基——然后在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，你一直是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前请审查变更，并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码已经完成。接下来一起审查、创建 PR，并让 Agent Merge 管理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似 `"PR #14 was squash-merged successfully."` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理 pull request。本课完成了以下任务：

- 了解了 Agent Merge 及其如何自动管理合并生命周期。
- 检查了完整的功能变更和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的 cloud agent 委托。

## 资源

- [使用 Copilot CLI 管理 pull request][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
