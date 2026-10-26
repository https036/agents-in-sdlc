---
title: "演習 1 - カスタム指示（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。Copilot にコンテキストを伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成でどのようなコードを作るかだけでなく、その方法も指示できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ スライス（出版社ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 指示の更新とフィルター機能のスライスを `main` にコミットし、次の演習で Copilot が新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準と異なる場合があります。Copilot の動作は決定的ではありません。この演習では、出力を文字単位で一致させるのではなく、指示を更新した後の動作の傾向を確認します。

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
5. プロジェクト固有のユニット テスト作成の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、例を示せます（パスはリポジトリ ルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc のドキュメント コメントとファイル ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、ベスト プラクティスに沿って変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な設定は、該当する手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して結果の違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを作成する場所を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。ファイルへの変更を適用できるようになります。

   ![Chat ビューでエージェント ピッカーを表示](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、コードを更新します。ヘルパー ファイルとテストを変更する場合もあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、出版社の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規則によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は決定的ではないため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作の一貫性が改善されることが、この演習の目的です。

## リポジトリ全体の標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるファイルです。コード提案を改善できるよう、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加して、ファイル ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、プロジェクト全体の指示には `.github/copilot-instructions.md` を、言語、ファイルの種類、特定のタスクには `*.instructions.md` を使います。ドキュメント コメントのルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話のコンテキストをリセットします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム用の出版社データ アクセス ヘルパー。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * 出版社の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 出版社オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナル ウィンドウを開きます。
2. ターミナルで `git status` を実行し、指示ファイルの更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示ファイルとヘルパーの変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成し、バックログにある指示ファイル更新の issue に関連付けます。手動でもできますが、GitHub Copilot に依頼できます。issue を検索し、PR の説明に issue を解決することを記載してもらいます。

> [!NOTE]
> この演習では、GitHub 上で Copilot に操作を依頼します。プロジェクトのテンプレートには GitHub のツールがすでに接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従ってください。後の演習では、この接続の仕組み (Model Context Protocol) について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関係する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には、指示の更新と、今後のフィルター機能で使う出版社ヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue の検索と PR の作成を開始します。
5. Copilot による GitHub 操作の **Allow** を求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、Copilot に PR をマージし、ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot による GitHub 操作やシェル コマンドの実行を求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示のままコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認しました。
- 指示の更新と出版社ヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は、この指示を使って Copilot がコードベース全体に機能を追加する[agent mode][next-lesson]を学びます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
