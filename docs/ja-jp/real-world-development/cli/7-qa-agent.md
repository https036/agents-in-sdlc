---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールをどのように使うかを確認します。
- 再利用できる品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター issue の要件に照らして結果を確認します。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。プロファイルには、エージェントの目的、指示、使用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を記述します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、それらをどのように使うか Copilot に指示します。これらの機能の代わりになるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用できる QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実行するチェックと、従う必要がある制約を定義します。

1. codespace に戻り、フィルター機能の会話が Interactive モードになっていることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと制約を正しく設定したことを確認します。これにより、検証だけを依頼したのに不完全または広すぎる指示でコードを変更することを防げます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は任意ですが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を基準に確認し、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使用することを確認します。
4. QA が根拠とともに結果を報告し、実装コードの変更前に許可を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルター機能の会話を再開し、QA を選択すると、issue と計画時の判断を使えます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントが利用可能な一覧に表示されない場合があります。エージェントは存在しており、上記のコマンドで有効にできます。

2. QA エージェントに、次のプロンプトを送信して機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールをどのように使うかを確認しました。
- 再利用できる品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、issue の要件に照らして結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、それらをまとめて機能の PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
