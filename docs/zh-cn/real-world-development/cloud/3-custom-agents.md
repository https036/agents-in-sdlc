---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以为 Copilot 提供专门的说明、最佳实践、编码模式和领域知识，从而帮助它更高效地完成特定类型的工作。团队可以把自身经验固化为可复用的智能体，例如强制遵循 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体可通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存放在 `~/.copilot/agents`。每个文件都有 YAML frontmatter，至少包含 `name` 和 `description`，后面跟着用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有一些重叠。两者主要都通过 Markdown 文件定义，也都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**负责执行工作，**技能**提供工具。

自定义智能体拥有自己的上下文窗口，并且可以在工作过程中编排技能，甚至其他智能体。在本实验中，无障碍自定义智能体会根据无障碍准则审查并更新网站；在执行过程中，它可以调用 pull request 工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方法。和 AI 中的其他工作一样，需要通过测试和迭代，找到最适合当前环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将探索自定义智能体的以下方面：

- 自定义智能体的定义方式。
- 如何为自定义智能体分配任务。

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户开放，无论用户的视觉能力或偏好如何。最近的用户反馈指出，由于文本和背景颜色对比度不足，一些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求添加一个可开关的高对比度模式。

无障碍功能至关重要，因此需要尽快实现。接下来使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

项目中已预先创建了无障碍自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 Codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，查看其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见陷阱和错误。

## 创建并分配 issue

Mission control 是管理环境中所有智能体的中心位置。你可以在此向 Copilot cloud agent 分配任务、监控任务，也可以引导正在运行的会话。现在向 Copilot 分配一项添加高对比度模式的任务。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

    ![云端智能体分配界面截图，突出显示自定义智能体和无障碍选项](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课探索了 GitHub Copilot 中的[自定义智能体][custom-agents]。自定义智能体是面向特定任务和领域定制的 AI 助手，可以将团队的经验和标准固化为可复用的指引，帮助 Copilot 更有效地完成特定工作。

本课介绍了以下内容：

- 自定义智能体的定义方式。
- 如何为自定义智能体分配任务。

Copilot 正在实现高对比度模式。现在可以转到[mission control 监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中启用自定义智能体的准备工作][org-custom-agents]
- [在企业中启用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
