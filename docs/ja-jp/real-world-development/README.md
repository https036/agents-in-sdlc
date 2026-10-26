---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、チームで使うバックログを通じて、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画してコードを作成し、動作をテストして、プルリクエストをレビューし、エージェントで作業を管理する流れを繰り返し実践します。

## シナリオ

開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys の新しい開発者として作業します。チームのバックログは GitHub issue としてすでに登録されており、すぐに取り組めます。アクセシビリティやコーディング標準などの品質改善に加えて、フィルターやページネーションの機能開発も含まれています。

## 作業する環境を選択する

GitHub Copilot は、作業場所に合わせて利用できます。開発方法に合った環境を選択してください。どのワークショップも、共通の Tailspin Toys シナリオと独自のセットアップから始まります。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを利用します。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業を行い、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、その作業を導き、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
