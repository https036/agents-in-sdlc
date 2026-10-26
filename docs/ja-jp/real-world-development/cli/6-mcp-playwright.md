---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、さまざまな作業が必要です。データや外部サービスを使い、Copilot が実行できる自動化を追加することもあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれた機能を超えるツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使用するかを学びます。
- まだ使用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには実際に操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにすると、変更を確認する作業をさらに自動化でき、期待どおりに動作するという確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって最新情報にアクセスし、ユーザーに代わって操作できます。

ツールとリソースは、外部のツールやサービスとの橋渡しを行う MCP サーバーを通じて利用できます。各 MCP サーバーが提供するツールとリソースは異なります。

よく利用される MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあり、GitHub はエコシステム内でサーバーを見つけやすくするための [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ使用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が構成されている場合は、セットアップを省略して次のセクションに進みます。

1. codespace に戻ります。
2. Copilot CLI で次のコマンドを実行し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Copilot に Playwright で機能を確認してもらう

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は Copilot から利用できる Playwright MCP サーバーを使って、同じ確認を依頼します。

1. 次のプロンプトを使い、Copilot に Playwright MCP サーバーで機能を検証するよう依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、MCP サーバーを自動的に判断するため、使用を明示する必要はありません。ただし、利用するサーバーが分かっている場合は伝えておくと、より一貫した結果を得られ、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を確認します。
3. 報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から実際のブラウザーで Playwright MCP サーバーを使い、機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使用するかを学びました。
- Playwright MCP サーバーをまだ使用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせた[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
