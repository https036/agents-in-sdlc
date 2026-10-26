---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而一直积压的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。任务完成后，它会生成草稿 PR，供开发人员审查。这样可以将繁琐工作委派给智能体、加快开发进度，并让开发人员专注于更重要的工作。

本课将探索 Copilot cloud agent 的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 了解清晰界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受聘创建网站的承包商没有做好文档工作——实际上，代码几乎完全缺少文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们想在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏，让访客可以继续浏览。目前不需要完善实现，只需要能用于 UX 验收测试。这项任务暂时受阻，因为还有优先级更高的 issue。

这两项工作都可能很快被降级处理，因此适合分配给 Copilot cloud agent。它可以异步处理任务，让开发人员先完成其他工作，之后再回来审查 Copilot 的工作并确认结果符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。与和人类开发人员协作类似，可以通过多种方式分配工作，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 pull request 来跟踪进度、设置工作环境并开始处理任务。会话进行期间或完成后，都可以查看 Copilot 的工作。解决方案准备好审查时，Copilot 会在 pull request 中提及你。

## 清晰界定任务范围的重要性

GitHub Copilot 没有魔法——使用它并不意味着可以跳过思考。即使是看起来简单的操作，仔细拆解后也可能相当复杂，因此[要认真界定分配给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的引导作用。如果已经做过自定义说明练习，就看过 `.github/copilot-instructions.md` 的效果；如果还没有，现在正好可以快速查看。将工作分配给 Copilot cloud agent 前，只读查看存储库中已有的说明文件，之后便能观察它们的影响。

在 GitHub Web UI 或已启动的 codespace 中查看以下文件：

- `.github/copilot-instructions.md`——审查 **Code standards** 部分，特别留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`——留意 `applyTo` frontmatter，它将说明的适用范围限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给 cloud agent。查看生成的 pull request，注意其中的 TSDoc 文档注释、TypeScript 约定和文件注释头；这些内容来自上述说明文件。后续练习还会再次检查这些要求。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要指定环境并运行设置脚本，确保开发环境正常。将任务分配给 Copilot 时也是如此，它的工作方式与软件工程师类似。

Cloud agent 在 GitHub Actions 中运行。可以在 `.github/workflows/copilot-setup-steps.yml` 文件中配置[专用设置工作流][setup-workflow]，让它在开始工作前运行，从而获得所需开发工具和依赖项。为方便实验，本项目已预先配置该工作流。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充种子数据的 SQLite 数据库（用于单个 Astro 应用）：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为这些步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆存储库，Copilot 会在步骤完成后自动完成克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，而无需等待 Copilot 运行。

## 添加文档

每个人都知道文档的重要性，但许多项目中的信息已经过时，或者完全缺失。这类技术债务常常得不到处理，降低了开发效率，也增加了维护代码库和引入新开发人员的难度。Copilot 擅长编写文档，因此可以把这项任务分配给 Copilot cloud agent，让它在后台生成所需文档。后续练习会回来审查其工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 pull request（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等一两分钟，然后刷新浏览器。
12. 几分钟后，Copilot 应会创建待办任务列表。

> [!NOTE]
> Copilot 创建的待办列表可能需要几分钟才会显示在 PR 中。它正在设置环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查任务列表及其计划完成的工作。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到状态指示器更新。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、分析当前状态、暂停思考并确定计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一就是异步执行任务，让我们有时间处理其他工作。接下来利用这一点，再分配一项任务给 Copilot cloud agent，同时开始为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot cloud agent 的一个优势是可以拆分工作：你专注于一项任务，它同时处理另一项任务。为设计团队添加相关游戏区域也许不需要很久，但这段时间仍可用于处理其他工作。现在将这项任务交给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在 issue 的第一条评论上看到一组 👀，表示 Copilot 正在处理任务。

![Copilot 用眼睛表情表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二项请求。Copilot cloud agent 的工作方式与软件工程师类似，因此无需持续监控；完成后再回来审查即可。现在开始创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 Copilot，让它异步处理。Copilot 可以帮助处理技术债务、添加功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 了解清晰界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义智能体。Copilot cloud agent 也可以[使用 MCP 服务器][cloud-agent-mcp]，并支持前面模块介绍过的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
