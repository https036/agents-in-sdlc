---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。具体的には、次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで実装方針を定義し、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで確認した根拠を評価しました。
- フィルター機能全体をレビューし、その PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

ワークショップでは、更新した `main` からそれぞれ独自のブランチを作り、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、未評価であることを明示する表示をゲーム カードに追加しました。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加しました。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業をひとつの PR にまとめました。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは要件とテスト カバレッジのレビューにこれらの結果を加え、PR の前にまとめて確認しました。

追加したテストで実際の不足を補う必要があります。QA を実行してテストを追加する必要がない場合も正しいことがあります。マージを許可する前にコードと検証の根拠を確認し、変更後は影響する根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。こうしたカスタマイズは会話をまたいで再利用でき、チームのニーズに合わせて変更できます。指示は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタム エージェントは専門的な役割を定義します。MCP サーバーは外部ツールに接続します。エージェントの概要だけでなく、実際の変更内容とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。作業前に方針を考えるときは **Plan**、範囲を絞った変更で状況を把握し続けたいときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るのか、なぜ必要なのか、どのように作るのかを明確に説明すると、生成される内容が変わります。

## さらに試す

中心的なワークフローを完了しました。Copilot CLI の機能をさらに試してみましょう。

- `/review` でコードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、何年も前から作りたいと思っていた小さなアプリなどで試してください。学びをチームと共有し、メンバーの経験からも学びます。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、[Foundry Backer Concierge シリーズ][foundry]（任意）に進んでください。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
