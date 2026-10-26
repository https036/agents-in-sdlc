---
title: "演習 1 - カスタム指示（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前と変更後のプロンプトを比較しません。Copilot cloud agent は GitHub issue に対して非同期で動くため、リアルタイムでの比較が難しいからです。このハーネスの後半で cloud agent が作成するプルリクエストを確認するときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc のドキュメント コメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。好みのコーディング規則、ライブラリ、コメントの形式などを指定できます。リポジトリ全体、または特定の種類のファイルやタスクに対する指示ファイルを作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリのすべてのリクエストで Copilot に送信される単一の指示ファイルです。プロジェクトの技術スタック、概要、ベスト プラクティスなど、多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対するガイドラインを提供します。たとえば TypeScript や Astro、UI コンポーネントの作成、ユニット テストなどを対象にできます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` frontmatter がある `*.instructions.md` ファイルの両方を、コード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明するには、この演習の範囲を超えます。ここでは、サンプル プロジェクトの例を参考に、代表的な方法を紹介します。

- `copilot-instructions.md` には、作成するもの、プロジェクト構造、全体的なコーディング標準など、プロジェクト全体に適用する指示を記載します。
- `*.instructions.md` を使い、ユニット テスト、Astro コンポーネント、データ層など、ファイルの種類や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確な指示とコード例を示します。望ましい例と避ける例の両方を記載できます。

AI の使い方に決まった方法がひとつあるわけではありません。プロジェクトに適した指示ファイルの形は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI の変更][ui-instructions]や [Astro][astro-instructions] など、さまざまな作業を対象にしたファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。操作は利用する環境によって異なります（VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` など）。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェント、その他のリソースが集まったリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、`copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開いて確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** の各セクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを指定する glob です。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. ユニット テスト作成の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、例を示せます（パスはリポジトリ ルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc のドキュメント コメントとファイル ヘッダーのルールを追加します。

## リポジトリ全体の標準を追加する

ここで行う編集は、コード内のドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーとして記述するプロジェクト全体のルールを追加することです。以降の演習で cloud agent が issue に取り組む際に、この指示が使われます。

編集を始める前に、作業ブランチを作成します（cloud agent は issue の対象ブランチにある指示を読み込みますが、変更を整理してコミットします）。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるファイルです。コーディング標準を記載し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを確認します。プロジェクトの標準が記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加して、ファイル ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、プロジェクト全体の指示には `.github/copilot-instructions.md` を、言語、ファイルの種類、特定のタスクには `*.instructions.md` を使います。ドキュメント コメントのルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、issue が対象とするブランチの指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が新しい指示を使えるように、変更を `main` に取り込みます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で issue を割り当てたときに、cloud agent がこの指示を読み込めるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略し、変更を `main` にコミットできます。ここでは、実際のプロジェクトでの進め方に合わせてブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りの演習で、cloud agent が作成するプルリクエストを通じて、その標準が適用される様子を確認します。

次に、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
