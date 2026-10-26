---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察筛选功能。现在通过自定义智能体将这些能力结合起来，并建立一套明确的 QA 流程。

本课将完成以下任务：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在打开 pull request（PR）前，始终对需求、代码质量、自动检查、测试覆盖率和浏览器行为进行一致的审查。自定义智能体可以协调这套 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件会说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能打包可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力来评估需求并报告发现。它不会取代这些功能，也不需要另开一个对话。

## 创建 QA 配置

打开功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置会定义 QA 要执行的检查，以及必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，请先检查其配置，确认 Copilot 已准确记录预期的 QA 工作流和边界。这样可以避免在只要求验证功能时，智能体修改功能代码或执行过宽的任务。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需项；`name` 是可选项，但填写后会为智能体提供清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、在修改实现代码前征求同意，并且不会提交变更或打开 pull request。
5. 如果生成的配置遗漏了任何职责或边界，请让默认智能体修改后再继续。

## 根据 issue 运行 QA

Copilot CLI 会在会话开始时加载项目智能体。创建配置后，恢复同一个筛选功能对话并选择 QA，这样它就能使用已有的 issue 和计划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但它仍然存在；上述命令会启用它。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后，阅读它生成的报告。

## 总结和后续步骤

你已在工作流中添加了可复用的专用角色，并审查了它的工作。本课完成了以下任务：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备就绪。接下来将[将这些内容整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
