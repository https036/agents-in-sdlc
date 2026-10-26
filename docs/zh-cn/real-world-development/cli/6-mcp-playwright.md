---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还需要处理数据和外部服务，也需要为 Copilot 提供更多自动化能力。这正是 MCP 服务器发挥作用的地方。MCP 服务器可以扩展 Copilot CLI 的内置能力，为它提供更多工具和服务。

本课将完成以下任务：

- 了解 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，还需要实际与界面交互。你希望允许 Copilot 像用户一样使用正在开发的网站，从而进一步自动化变更验证，并更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了一种与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，从而获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体使用。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和 pull request 的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 托管了一个 [MCP 注册表][mcp-registry]，以提升生态系统中内容的可发现性并促进贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 请 Copilot 探索功能

前面已经手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的验证。

1. 使用以下提示，请 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会自行判断。不过，既然你知道应该使用哪个工具，明确指出也有帮助！这可以让结果更一致，还能节省一些 token。

2. 观察 Copilot 在浏览器中执行的各个步骤，确认功能是否正常。
3. 阅读报告，确认所有行为均符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你报告结果。

## 总结和后续步骤

恭喜，你已使用 Playwright MCP 服务器通过 Copilot CLI 在真实浏览器中探索功能。本课完成了以下任务：

- 了解了 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了该服务器。
- 请智能体驱动浏览器并探索筛选功能。

接下来将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具结合起来，构建专用角色。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
