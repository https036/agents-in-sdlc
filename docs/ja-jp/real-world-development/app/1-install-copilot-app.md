---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、それぞれ独立したワークスペースで複数のエージェントを並行して指示し、繰り返しの作業を自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーも用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認します。
- アプリについて学ぶため、簡単なチャットを試します。

## シナリオ

チームは増え続けるバックログに対応するため、AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージなどを 1 か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> Copilot Free、Copilot Student、またはいずれかの有料プラン (Pro、Pro+、Business、Enterprise) の対象プランが必要です。Copilot Business または Copilot Enterprise を使用する場合は、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 用のバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディング ページの手順に従ってインストールします。
3. インストール後にアプリを開きます。
4. **Sign in to GitHub** を選択して、画面の指示に従い認証します。データ所在地機能を備えた GitHub Enterprise Cloud を使用している場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択を求められた場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択を省略して、オンボーディング後に追加することもできます。
6. テーマを求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧にあれば、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーは **Sessions** の下にまとめられ、各リポジトリの下に個別のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーには、主に次の領域があります。

- **New** — Copilot との新しいチャット セッションを開始します。
- **Pull requests** — アプリの GitHub ネイティブ統合によるプルリクエストです。アプリを離れずに PR を参照、絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** — GitHub の issue が表示されます。issue を参照、絞り込み、issue からのセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。ToDo リストや定期的なプロジェクトの保守など、手間のかかる作業を任せるのに役立ちます。振り返りで次のステップとして紹介しますが、追加のワークショップ演習ではありません。
- **Customize** — MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して Copilot app の機能を拡張します。Playwright MCP の構成に使います。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを始めるとこの項目が表示されます。表示されていない場合は **Sessions** の横にある **+** を選択して、最初のチャットを始めます。
- **Sessions** — リポジトリ フォルダーとエージェント セッションをまとめて表示します。`tailspin-toys` などのリポジトリ名を選択するとリポジトリ ビューが開き、その下にあるセッションを選択すると会話に戻ります。各セッションは専用の独立したワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。星評価の追加で、最初のセッションを始めます。

ワークショップを進めながら、ワークスペースの使い方を確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で、Tailspin Toys のリポジトリ フォルダーを選択します。個別のセッションではありません。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. issue 番号ではなく、次のタイトルで検索します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。各 issue はエージェント セッションの開始点にもなります。簡単な最初の変更を終えた後、フィルター issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** に表示される項目は、自動的に絞り込まれ、Copilot app に追加したリポジトリのものだけが表示されます。ほかのリポジトリの作業項目も表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するとよいでしょう。その用途には **quick chat** が適しています。Quick chat ではブランチや worktree を作成せずに質問やブレインストーミングができるため、セッションを必要としない簡単な質問に使えます。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。開始後に **Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは独立した Git worktree で実行されるため、変更を競合させずに複数のエージェントを並列で実行できることがわかります。会話は続けることも、いつでも新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで簡単な質問をする。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
