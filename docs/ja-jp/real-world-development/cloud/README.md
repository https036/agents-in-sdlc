---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査して変更を加え、プルリクエストを作成します。その間、ほかの作業を続けられます。

各演習では、まず cloud agent が使うカスタム指示を追加し、GitHub issue を割り当てて実装を任せます。カスタム エージェントを確認して、cloud agent のアプローチを調整する方法を学びます。エージェントのダッシュボードでセッションを監視、誘導し、最後にプルリクエストを確認して結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が使うカスタム指示を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して誘導する |
| [5. 反復][ex5] | レビュー | PR を確認し、Copilot の作業を改善して次の手順を決める |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] GitHub アカウントと、有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プラン
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれており、すべての演習を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise の場合、組織の管理者が有効にする必要があります。

> [!NOTE]
> この演習では MCP を扱いません。Copilot で MCP サーバーを使用する方法については、[CLI 演習](../cli/)または [VS Code 演習](../vscode/)を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
