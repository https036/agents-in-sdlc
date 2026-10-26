---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。打开 pull request 前，应在浏览器中确认它确实可以正常运行。接下来连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，而无需亲自操作应用；之后发布分支并打开 PR。

本练习将完成以下任务：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建 pull request。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以通过编辑器访问外部工具，从而增强功能。Copilot 使用 Model Context Protocol (MCP) 实现这一点——这是智能体与外部工具和服务通信的标准方式。

![示意图：智能体模式如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了一种与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，从而获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体使用。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新存储库、管理 issue 和 pull request 等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器提供各种工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，以提升生态系统中内容的可发现性并促进贡献。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予对敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 其中应该已经为你配置了 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

该 `github` 配置随项目模板提供，因此 Copilot 能在上一练习中读取待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 会通过 HTTP 连接该服务器，并在 Copilot 首次使用其工具时让你通过 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供一个可控浏览器，正好可以用来测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志表示让 Playwright 在无可见窗口的模式下运行。Codespace 没有桌面环境，因此必须使用此模式。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已经安装好。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器。Copilot 首次需要使用某个服务器的工具时，VS Code 会启动服务器，并让你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，服务器才能启动。

手动启动是可选的。如果跳过这一步，下一步中 Copilot 首次需要使用 Playwright 工具时，VS Code 会启动服务器并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供一个可驱动的真实浏览器。无需手动操作应用来检查结果，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的情况。这是在不离开对话的情况下，快速确认功能是否符合预期的方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接和列表项）进行推理。因此，快速的功能检查也可以作为一次简单的无障碍检查。

服务器已连接且应用正在运行时，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步完成测试并报告结果。将它的总结与 issue 中的验收条件进行比较。如果发现异常，可以继续追问，或在打开 pull request 前让它修复代码。

> [!NOTE]
> 应用必须运行在 `http://localhost:4321` 才能进行此测试。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 时，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，就可以打开 pull request（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的变更。
2. 选择 **+** 图标暂存变更。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做变更的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建 pull request

可以通过 github.com 或 GitHub 命令行界面（CLI）等方式创建 pull request。既然已经使用 GitHub Copilot，不如让它来创建 PR。它可以找到相关 issue，并将 PR 与其关联。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 在智能体下拉列表中确认已选择 **Agent**，让 Copilot 可以使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 完成身份验证；按照提示授权即可。
5. 注意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接，审查 pull request，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习完成了以下任务：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在发布前通过浏览器手动测试筛选功能。
- 发布分支，并为筛选功能打开 pull request。

你使用 MCP 服务器*测试*了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
