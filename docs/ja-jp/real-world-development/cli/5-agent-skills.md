---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業が必要です。これまで、コードを手動で確認し、指示ファイルを使って標準に従うようにしました。テストや lint など、継続的インテグレーション (CI) のほかの作業はどうすればよいでしょうか。

このような作業には **エージェント スキル**が適しています。スキルを使うと、テストや lint などの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用できるタスクの指示、実行可能なスクリプト、補助リソースをまとめ、必要に応じてエージェントが読み込めるようにします。基本的には、スキル名のフォルダーにある `SKILL.md` ファイルです。Markdown にはスキルの名前と説明を定義する frontmatter、処理の概要、呼び出すタイミングに関するガイダンスが含まれます。フォルダーにサブフォルダーを追加して、スクリプトやその他のリソースを置くこともできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チーム内で共有して再利用できるリポジトリ アセットとしてプロジェクトの `.github/skills` フォルダーに配置できます。ユーザー スキル用フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、Copilot がスキルをいつ呼び出すか判断するのに役立ちます。
3. 指示を読んで、テストと lint のプロセスをどのように導くか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然な言葉で実行を依頼することもできます。スキルに 3 つのチェックを実行してもらいましょう。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した結果を示す、より詳しいレポートにしましょう。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを使ってみましょう。Copilot CLI は会話を再起動しなくても、編集したスキルを再読み込みできます。

1. 次のコマンドを実行します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと**まったく同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェックを行う `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能と一緒に機能 PR に含めます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルのその他の例

次のコミュニティの例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
