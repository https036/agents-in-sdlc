---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の 2 つの演習では、ユーザー エクスペリエンスの改善や機能の追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。たとえば、GitHub.com の[エージェント ページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を確認し、進行中のセッションを誘導できます。

このレッスンでは、次のことを行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、機能の追加を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加するのに適したタイミングだと気付きました。サイトのスタイルと切り替え機能をすでに更新しているため、ハイ コントラストに加えてライト モードも追加するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインにタスクの一覧が表示されます。たとえば、次のタスクがあります。
    - コードベースのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような作業が含まれます。
    - リポジトリからコードをチェックアウトする
    - Copilot が作業する環境を作成する
    - MCP サーバーを設定する
    - 割り当てられたタスクを完了するためのさまざまな手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更ファイルを確認し、さらに状況を監視できます。

## cloud agent を誘導する

進行中のタスクを確認できたので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。Copilot が付けたタイトルや作業状況によって、表示名は異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。作業が始まると、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents ページで、作業中の cloud agent タスクへの指示ダイアログを表示](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot の作業を待つ

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中のステップが終わると、新しい要件を作業に取り込みます。

今回も数分かかります。その間に、これまで学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページについて学びました。このページは、GitHub Copilot cloud agent のタスクを監視して誘導するための中心的な場所です。進行中や完了済みのタスクを追跡し、実行中の作業を確認し、進行中のタスクに追加の指示を送れます。

次のことを学びました。

- cloud agent のタスクを監視するために、mission control とエージェント ページを使用しました。
- 進行中のセッションに機能の追加を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
