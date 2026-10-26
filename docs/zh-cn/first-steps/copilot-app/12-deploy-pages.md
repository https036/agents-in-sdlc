---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查 pull request，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

通过 GitHub Pages 为 Space Quiz 发布一个公开访问入口。Copilot 可以准备工作流并引导部署，但仍需审查变更并确认网站已上线。

本课将完成以下任务：

- 请 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和 pull request。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已经位于 GitHub 存储库中，但其他人还需要一种简单的方式来体验它。使用 GitHub Pages 发布这个单文件网站，即可通过公开 URL 访问，无需添加服务器或运行时依赖项。

## 请 Copilot 部署网站

在 Copilot app 的 `space-quiz` 会话中发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz pull request；成功运行的 GitHub Actions Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的 Space Quiz 网站。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并 pull request，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查 pull request 中的变更，包括工作流的构建和部署步骤。
3. 仅在所需的 pull request 检查通过后合并。
4. 持续跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 能正常加载。

如果智能体无法启用 Pages 或合并 pull request，请按照它提供的具体说明操作。只有在部署成功且网站可以正常加载后，任务才算完成。

## 总结和后续步骤

Space Quiz 现在已作为实时网站发布，并且你已亲自验证部署，而不是只依赖智能体的报告。继续学习[第 13 课：回顾和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
