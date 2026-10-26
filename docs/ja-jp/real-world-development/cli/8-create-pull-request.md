---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、ひとつのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。ワークショップを簡略化するため、フィルター機能と品質ワークフロー全体をひとつの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能変更全体と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフロー全体を通して、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys は残りの PR 作業を自動化したいと考えていますが、マージを許可するかどうかは開発者が決めます。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（継続的インテグレーション (CI) チェックの失敗、レビュー コメントへの対応、必要な場合のリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることができます。作業をレビューし、準備ができていることを確認してから有効にしてください。

## Agent Merge で PR を管理する

コードがそろったので、レビューして PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を実行してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` コマンドを実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限不足を回避しません。ブロックされた場合は報告された理由を読み、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コード生成、テストとコードの検証に加えて、プルリクエストのプロセスも自動化しました。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を学びました。
- 機能変更全体と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[ほかの Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
