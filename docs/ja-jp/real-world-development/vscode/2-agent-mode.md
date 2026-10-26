---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションを少し変更するだけでも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行してデバッグし、変更漏れのファイルが見つかったら、さらに修正するという作業を繰り返します。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を見てみましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる種類の作業向けに設計されています。agent mode は IDE 内で作業し、すぐにフィードバックを得ながら対話できます。cloud agent はチーム メンバーのように非同期で作業し、issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層にわたって新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更やテストを確認する方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと出版社でゲームを絞り込めるようにしたいと考えています。前の演習で出版社ヘルパーを追加しました。この演習では、Copilot Agent Mode を使って、データ層、UI、テストの残りの作業を行います。

## Tailspin Toys の Web サイトを実行する

変更する前に、Tailspin Toys の Web サイトを実行し、現在の機能を確認しましょう。

この Web サイトは、開発者向けテーマのボード ゲーム クラウドファンディング プラットフォームです。ゲーム一覧と各ゲームの詳細を表示します。Astro アプリは、ビルド時にページを静的 HTML として生成します。ページは frontmatter から Drizzle ORM を使ってローカル SQLite データベースに直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、操作性は標準 DOM API を使う小さな範囲限定の Astro `<script>` で実装します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では、次の手順で実行します。

1. codespace に戻ります。前の演習を終えている場合は、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、codespace 内に新しいターミナルを開きます。
3. フィルター機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザー ウィンドウが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバー アドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、次の主な機能を確認します。

- **ホーム ページ**: ボード ゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると詳細ページが開き、タイトル、説明、出版社、カテゴリなどの情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な設定は、該当する手順で説明します。

Web サイトは動作していますが、機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は複数ファイルにわたる変更を計画して実装し、ターミナル コマンドやツールを実行します。

   ![Chat ビューでエージェント ピッカーを表示](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。似た体験を提供し、提案されるコードの一貫性を高めることを意図しています。ただし、LLM の動作は決定的ではないため、ワークショップの説明と異なる提案が返されることがあります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の性質上、Copilot が異なる MCP コマンドを使用する場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝える指示ファイルを確認します。[UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトの機能を追加する方法が記載されています。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類別の指示へのリンク

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルを基準とするため、大きな指示を再利用できる小さなファイルに分割し、例やテンプレートを参照できます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) にフィルター処理を追加または調整する
- フィルター動作のテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェント ピッカーを表示](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue の要件に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要な機能に関連するファイルを特定します。データ層のヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。コードベースやタスクの複雑さによって数分かかる場合があります。作業中にエディターで **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したりテストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、完了してエラーが検出されなくなるまで作業します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ず確認してください。

10. Web サイトを実行しているブラウザーに戻り、新しい機能を試します。
11. 問題がないことを確認してコードをレビューしたら、Copilot Chat ウィンドウの **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。

- GitHub Copilot Agent Mode が UI とデータ層にわたって新機能を実装できることを確認しました。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数ファイルにわたる変更を行う方法を確認しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更やテストを確認しました。

次は、[Playwright MCP サーバーで機能をテストします][next-lesson]。その後、プルリクエストを作成します。

### おまけの演習 – ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションを実装させてみましょう。次の点を検討してください。

- 既存の指示ファイルを使用するなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定するかなど、ページネーションの仕様を決めます。
- 希望する実装に必要なガイダンスを Copilot に提供できるよう、プロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを提供しながら何度か調整する場合があります。これは通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
