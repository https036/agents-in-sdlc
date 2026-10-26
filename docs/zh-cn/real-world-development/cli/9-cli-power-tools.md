---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委托给 cloud agent 的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以使用高级功能、查看幕后信息，并调整配置。你已经用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，现在来探索其他实用命令。

本课将完成以下任务：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委托给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他功能——管理上下文、切换模型、共享会话，以及选择是否将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未运行，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意当前模型、token 使用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话内容本来就很少，变化可能不会很明显。

> [!NOTE]
> 上下文窗口快满时，Copilot CLI 会自动压缩。想自行选择压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有不同偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享经验，有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给其他人或发布前，先审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展或委托工作

核心工作坊已经完成。Copilot CLI 还提供了可处理较大任务的命令：

- `/fleet` 可以将独立子任务分配给多个子智能体并行执行。
- `/worktree` 可以为独立任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot cloud agent，由它异步处理，并可能打开 pull request。

这些命令都是可选的，因为它们可能创建额外工作树或执行远程操作。尝试前，请先开始一个范围明确的新任务，并通过常规工作流审查结果。想深入了解异步智能体工作，可继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 中的斜杠命令可用于调整配置、共享会话和查看幕后情况。本课完成了以下任务：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委托给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索。接下来通过[回顾已学内容][next-lesson]并了解后续步骤，为本次学习收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
