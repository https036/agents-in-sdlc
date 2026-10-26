---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、専門知識を指定します。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの振る舞い、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の進め方を伝えます。最も分かりやすい区別は、**カスタム エージェントは作業者で、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整して作業できます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに照らしてサイトを確認、更新します。その作業の一環として、プルリクエスト ワークフローのスキルやテストの実行、管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダーク テーマは、テキストと背景色のコントラストが不十分で読みづらいという意見が寄せられました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。機能の生成にはカスタム エージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot にどのような指示を与えるかを把握しましょう。

codespace に戻り、アクセシビリティ作業用に `main` から新しいブランチを作成して切り替えます。演習 3 のフィルター機能 PR とは別に作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続く各セクションを確認します。アクセシブルな Web サイトのコードを生成する際の主な責任、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、その指示とツールを使って会話できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な設定は、該当する手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します（まだ選択されていない場合）。

   ![Chat ビューでエージェント ピッカーを表示](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに、次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始します。コードベースを調査し、変更を提案して、プロジェクト ファイルを編集します。各編集は、ファイル パスとレビュー可能な差分とともにチャットに表示されます。
6. 編集が進むと、**Files changed** インジケーターに、このセッションでエージェントが変更したファイルが反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、作業中に内容を確認し、方向を調整します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた AI アシスタントです。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次のことを学びました。

- カスタム エージェントの定義方法
- Copilot Chat の agent mode でカスタム エージェントを使う方法

次は、[エージェントの作業を監視して誘導します][next-lesson]。同じセッション内で変更を確認し、ハイ コントラスト モードに加えてライト モードを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [Awesome Copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
