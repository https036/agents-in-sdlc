---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーに Markdown ファイルを作成し、目的に合わせた指示、ベスト プラクティス、コーディング パターン、専門知識を指定することで、Copilot が特定の作業をより効果的に行えるようにします。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの振る舞い、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の進め方を伝えます。最も分かりやすい区別は、**カスタム エージェントは作業者で、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整して作業できます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに照らしてサイトを確認、更新します。その作業の一環として、プルリクエスト ワークフローのスキルやテストの実行、管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントについて、次のことを学びます。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダーク テーマは、テキストと背景色のコントラストが不十分で読みづらいという意見が寄せられました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。機能の生成にはカスタム エージェントを使用します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot にどのような指示を与えるかを把握しましょう。

codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続く各セクションを確認します。アクセシブルな Web サイトのコードを生成する際の主な責任、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを扱う中心的な場所です。ここから Copilot cloud agent にタスクを割り当て、進行状況を監視し、指示を変更できます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントとアクセシビリティが選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、特定のタスクや分野に合わせた専門エージェントを作成できます。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次のことを学びました。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して誘導します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
