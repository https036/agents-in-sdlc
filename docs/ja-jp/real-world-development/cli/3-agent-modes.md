---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針に合意し、Autopilot で issue に基づくフィルターを構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しましたが、大きな変更には、より堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに合わせて使えるため、適切な方法で適切なものを構築できます。このレッスンから数回にわたり、エージェントを活用した一般的な開発プロセスを進めます。issue から新機能を実装し、コードが有効で、機能が期待どおりに動作することを確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能のワークフローを続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中心的な概念に集中できるよう簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

機能の開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや出版社でゲームを絞り込める必要があります。バックログの issue には機能の概要がありますが、カテゴリの組み合わせなど、実装前に合意が必要な詳細もあります。Plan モードで要件を決め、範囲を明確にしてから Autopilot に実装を依頼します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、さらに重要になります。多くの開発者は次のようなフローで作業します。

1. 必要な内容を説明する issue を作成します。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって細部は異なりますが、多くの場合、上記に似た流れで作業します。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントがどの程度自律的に動くかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントが変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで繰り返します。

まず Plan モードで計画をレビューし、その後 Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、codespace に戻り、リポジトリとターミナルを準備します。

1. codespace に戻ります。停止している場合は、再起動します。
2. 星評価の PR がマージされたことを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストにできます。

## フィルター機能を計画する

計画を立てることで、Copilot に作業を任せる前に、機能やタスクの実装方針を定義できます。複雑な作業を始める前には、時間をかけて計画することをお勧めします。Plan モードに切り替え、計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、最適な方法を判断して、計画を作成します。

3. フィルターの動作について Copilot が質問する場合があります。希望に基づいて回答してください。間違った答えはありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 希望と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装を依頼します。Autopilot を使って Copilot に作業を任せます。

Autopilot は、タスクが完了したと判断するまで問題への対応を繰り返します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同等の表示を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。計画をよく確認してから承認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、セットアップの問題を解決してからチェック結果を完了とみなします。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して動作を確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を実行して、フィルター機能の実装とテストを確認します。
3. 結果を issue と、計画時に決めた要件と照らし合わせます。
4. コードを確認したら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードは正しく見えますが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリを起動するよう依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を使えると通知したら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリと出版社の組み合わせでフィルターを試します。
4. リセットと結果なしの場合の動作が、issue と合意した内容に一致することを確認します。
5. codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、文書化されたプラクティスに従ってコードを生成する方法をさらに詳しく確認し、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
