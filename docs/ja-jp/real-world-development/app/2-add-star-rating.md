---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットも試しました。次は、**エージェント セッション**を始めて、プロジェクトに初めて変更を加えます。変更は小さなものにします。ゲームのデータには星評価がすでにありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更を確認して、最初のプルリクエストとしてマージしましょう。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、その構成を学びます。
- プロジェクトに小さく目的を絞った変更を加えるよう、エージェントに依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、出版社、説明だけが表示されています。最初のセッションの準備運動として、エージェントに各カードへ既存の評価を表示してもらいます。小さく独立した変更なので、最初のセッションに最適です。

## セッションの構成

**セッション**とは、独自の分離されたワークスペースで動作するエージェントとの会話です。すべてのセッションには、**専用の Git worktree とブランチ**が割り当てられます。たとえば、一方で機能を追加しながら、もう一方でバグを修正する場合も、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにサイドバーにまとめられ、選択して切り替えられます。

セッションには、エージェントとの**会話**、エージェントがファイルを調査して編集する際の**ツール操作**、差分を含む**変更ファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを調査して機能を実装します。[アプリのセットアップ][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリの新しいセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択して、セッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択します。次のプロンプトで変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調査し、機能追加のために更新するファイルを探して、必要なコードを作成します。Copilot app を使って新しい機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前に確認します。Copilot app で変更を見てみましょう。

1. アプリ右上の **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細表示に使われる中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" に置き換える、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot はほかの生成 AI ツールと同様に確率的に動作するため、実際のコードは例と異なる場合があります。ただし、おおむね似たコードになるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。テストで数値の `starRating` と、`null` の場合の代替表示が確認されていることを確認してください。前提条件が不足していたり、チェックがスキップされていたりする場合は、成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動作すると判断せず、Copilot に Web サイトを開いてもらい、更新された UI を確認しましょう。Web サイトを起動して、ブラウザー キャンバスで開くよう依頼します。

> [!TIP]
> Canvas は Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンでカスタム Canvas を調べて作成しますが、ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトを送り、アプリを起動してブラウザー Canvas でページを開くよう依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価済みのゲーム カードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使って、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成しました。次に、新しいコードを既存のコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上の **Create PR** を選択します。
2. サインインを求められたら **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーで PR 作成前に対応する issue が必要だと Copilot が説明した場合は、そのプロセスに従うよう確認します。
4. Copilot が PR の作成を開始します。
5. チャットのすぐ上にある **PR** バブルを選択してレビューパネルで開き、プルリクエストを確認します。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しく開いたダイアログで **Merge pull request** を選択し、プルリクエストをマージします。

## まとめと次のステップ

おめでとうございます。GitHub Copilot app を使って最初の変更をリリースしました。

- エージェント セッションを開始し、その構成を学びました。
- ゲーム カードに小さく目的を絞った変更を加えるよう、エージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成してチェックを確認し、明示的にマージしました。

次は、[フィルター issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
