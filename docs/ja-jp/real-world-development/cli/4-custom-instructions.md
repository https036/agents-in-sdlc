---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]は、作成したいコードだけでなく、その構成方法も説明します。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示ファイルとパスを指定した指示ファイルがエージェントに届く方法を確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントにはコードの言い換えではなく、意図や判断が自明でない理由を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` の注入引数がある場合は、その引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述します。関連コードが変更されたら、コメントも最新にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot が適切な情報に基づいて作業し、これらのプラクティスに従うようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。好みのコーディング規則、ライブラリ、コメントの形式などを指定できます。リポジトリ全体、または特定の種類のファイルやタスクに対する指示ファイルを作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリのすべてのリクエストで Copilot に送信される単一の指示ファイルです。プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類（特定の言語、ファイル形式、作業など）に対するガイドラインを提供します。

> [!NOTE]
> ほかの指示ファイル形式のサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

まず、変更を加える前に、スターター プロジェクトに含まれている指示ファイルを確認します。

1. codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. ファイルのプロジェクト概要とコーディング ガイダンスを確認します。これらの指示は、このリポジトリにおける Copilot とのあらゆるやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト ファイルへのリンクを確認します。指示を再利用できる小さなファイルに分けられます。

## チームのガイダンスに合わせて指示ファイルを更新する

指示ファイルはよい出発点ですが、追加する情報があります。新しく生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。更新したガイダンスを使うため、フィルター機能のコンテキストを保ったまま会話を終了し、再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を実行して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから次のコマンドを実行し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行して変更された TypeScript ファイルを確認します。新しい TSDoc コメントがあり、コードを正確に説明していることを確認します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- リポジトリの指示ファイルを確認し、プロジェクトのガイダンスを把握しました。
- TypeScript の TSDoc コメントに関する標準を追加しました。
- 会話を再開して変更内容を確認し、新しいガイダンスが適用されていることを確認しました。

次は、繰り返し実行するチェックをまとめる [quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [Copilot CLI のカスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
