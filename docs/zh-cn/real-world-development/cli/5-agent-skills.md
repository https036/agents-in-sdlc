---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码能否运行，也使用说明文件确保代码符合标准。接下来还要考虑测试、lint 和持续集成（CI）的其他环节。

**智能体技能**非常适合处理这类任务！技能可以帮助 Copilot 正确执行这些操作。

本课将完成以下任务：

- 探索现有的 `quality-checks` 技能。
- 自定义结果报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和辅助资源打包在一起，供智能体按需加载。技能的核心是一个文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件通过 frontmatter 中的名称和描述定义技能，还会说明技能的用途以及何时调用。技能文件夹还可以包含脚本和其他资源子目录，供智能体调用。

> [!NOTE]
> 技能不一定需要包含其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述会帮助 Copilot 判断何时调用技能。
3. 阅读技能说明，查看它如何引导 Copilot 执行测试和 lint。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以通过 Copilot CLI 直接调用技能，也可以使用自然语言请求。现在请 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 输入以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看末尾生成的报告。

## 自定义报告

现在需要一份更好的报告，明确指出运行了什么、是否成功，以及工具实际报告了哪些内容。更新技能，让它按此格式生成报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

变更完成后，来看看实际效果。Copilot CLI 无需重启对话即可重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看末尾的报告，并将其与第一次报告进行比较。

## 总结和后续步骤

你已自定义并使用现有的智能体技能。本课完成了以下任务：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

筛选功能 PR 会包含这项改进。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
