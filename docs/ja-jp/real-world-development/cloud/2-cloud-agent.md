---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。解決されていないセキュリティの問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に行うよう設計されています。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を効率化できるため、開発者はより大きな課題に集中できます。

Copilot cloud agent について、次のことを学びます。

- コードを生成する環境をカスタマイズする。
- 操作を安全に実行する。
- issue の範囲を明確にすることの重要性。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys は、解決したい技術的負債を抱えています。最初のサイトを作成した委託先がドキュメントを十分に整備しなかったため、コードにドキュメントがほとんどありません。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がさらに高い issue もあります。

このようなタスクは、優先順位が下がって後回しになりがちです。Copilot cloud agent に割り当てれば、開発者が別の作業に集中している間に非同期で進められます。完了後に結果を確認し、期待どおりであることを検証できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。作業中または完了後に Copilot のセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような機能があるわけではありません。考える作業を省略できるわけではなく、一見単純な作業でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を明確にしてください。AI ペア プログラマーとして扱い、段階的に作業しながら、学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot の作業を導く方法について学びました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が機能する様子を確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み、後でその影響を確認しましょう。

GitHub の Web UI、または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規則と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。その後、作成されたプルリクエストに TSDoc のドキュメント コメント、TypeScript の規則、コメント ヘッダーが含まれているか確認してください。これらは指示ファイルによるものです。後の演習で PR を確認するときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには適切な環境とセットアップが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent は SWE と同じような方法で作業します。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。ラボの進行を助け、学習に集中できるように、あらかじめ設定されています。cloud agent が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用ブラウザー、単一 Astro アプリ用のマイグレーションとシード済み SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は操作用に独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。手順内でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL）
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

      # ビルドやテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様のファイルですが、次の点に注意してください。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たず、正しく動くことを確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く知られていますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に適しているため、この issue を Copilot cloud agent に割り当てましょう。cloud agent はバックグラウンドで必要なドキュメントを作成します。後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に ToDo リストを表示するまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、タスクの進め方を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されるまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を決めるために作業を一時停止し、コードを作成する手順が表示されます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的のひとつは、タスクを非同期に実行し、別の作業に集中できるようにすることです。この機能を活用し、Copilot cloud agent に別のタスクも割り当ててから、アプリケーションの機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点のひとつは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業に時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めたことを示しています。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、積極的に監視する必要はありません。作業が完了してからレビューできます。次に、カスタム エージェントの作成と使用に進みましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期でタスクを実行できます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次のことを学びました。

- コードを生成する環境をカスタマイズする。
- 操作を安全に実行する。
- issue の範囲を明確にすることの重要性。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みましょう。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も使用できます。これらは前のモジュールで説明しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
