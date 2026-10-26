---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使えるスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にはさまざまなスラッシュ コマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを終えました。次に、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウを超える場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って自分で確認、圧縮できます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを実行します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行し、結果を比較します。会話が短い場合は、大きく変わらないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを自分で決める場合は `/compact` を使います。関係のないタスクに切り替えるときに会話を新しくするには、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに異なる強みがあり、開発者の好みもさまざまです。Copilot CLI では、利用できるモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを実行します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより活用できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで `/share` を使う方法を確認します。
2. セッションを共有する場合は、ニーズに合う出力先を選択します。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡大または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、より大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割して並列実行できます。
- `/worktree` は、別のタスク向けに独立した Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送り、非同期で作業させてプルリクエストを作成できます。

これらのコマンドを実行すると、worktree の追加やリモートでの作業が発生する場合があるため、ワークショップでは任意にしています。試す前に、範囲を明確にした新しいタスクを始め、通常のワークフローで結果を確認してください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成、セッションの共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。次は、これまで学んだことと今後の学習方法を[振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
