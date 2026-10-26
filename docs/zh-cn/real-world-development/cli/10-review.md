---
title: "第 10 课 - 回顾与后续步骤"
description: "回顾 Copilot CLI 工作流、两个 pull request、可复用的自定义配置和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI，完成以下任务：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已准备好的筛选 issue。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个 pull request（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案、使用 Autopilot 模式实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并通过真实浏览器探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能运行结果和浏览器证据。
- 审查完整的筛选变更，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选委托到 cloud agent 的斜杠命令。

## 交付成果

本工作坊包含两个 PR 里程碑，每个都从更新后的 `main` 分支单独创建分支：

1. **星级评分：** 在游戏卡片上显示现有 `starRating`，并明确显示尚未评分的状态。
2. **筛选和质量工作流：** 实现筛选功能、更新说明并将其应用于功能代码、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到打开其 PR，你一直使用同一段对话和分支。为了简化工作坊流程，这些工作合并到了一个 PR 中。

## 多种验证方式

你通过多种方式检查代码：运行自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并使用新格式报告结果。QA 汇总了这些结果，以及需求和测试覆盖率审查。

新增测试应当弥补实际缺口；如果 QA 运行后发现无需添加测试，也可能是正确结果。授权合并前，请审查代码和证据。代码变更后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作结果。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义配置，并根据团队需求进行调整。说明用于设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。请审查实际变更和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先思考方案，使用 **Interactive** 跟进范围明确的变更，使用 **Autopilot** 处理定义清晰的任务。日常编辑可以选择更快速的模型，复杂任务则可以选择能力更强的模型。

上下文和基础设施同样重要。清楚说明希望构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出结果。

## 继续探索

你已完成核心工作流。还可以进一步了解以下 CLI 功能：

- 使用 `/review` 请代码审查智能体分析变更。
- 使用 `/rubber-duck` 讨论问题并获取不同视角。
- 使用 `/fleet` 编排多个独立子任务并行执行。
- 使用 `/worktree` 隔离独立任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

提升任何工具使用能力的最好方式，就是持续使用它。可以将它用于生产代码、个人项目，或那个想了很多年却一直没开始开发的小应用。与团队分享经验，也向团队学习。并且，一如既往地查阅文档。

如果想从终端继续扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。如果想比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理 pull request][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
