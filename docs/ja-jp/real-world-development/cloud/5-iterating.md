---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業を確認する

このラボでは、ユーザー エクスペリエンスの改善や機能の追加を目的とする複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能の実装、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更内容を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

何度も説明してきたように、生成 AI を追加してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ず確認し、通常の DevOps プロセスに沿って作業します。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は、監視なしで非同期に作業するため、安全を保つための制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスを持ち、コード変更用のブランチに対してのみ書き込みアクセスを持ちます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- MCP サーバーなどの[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントを確認する

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認し、コードにドキュメントを追加します。GitHub.com の通常の PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。これは想定どおりです。Copilot は MCP サーバーなどの外部リソースへのアクセスを既定で制限されています。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、PR に **[WIP]** が表示されます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまで学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントなど、コードの変更を確認します。実際の変更内容は異なる場合があります。

    変更内容を確認するときは、演習 2 の最初に確認したカスタム指示ファイルが適用されているか、TSDoc のドキュメント コメント、TypeScript の規則、コメント ヘッダーを確認します。

6. 問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot と PR で作業するときは、一方的に結果を受け取るだけではありません。チーム メンバーと同じように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は決定的ではないため、依頼する文面をひとつに限定できません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例など、コードベースに追加したいドキュメントについて、**@copilot** をメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更内容を確認します。

納得できるまで作業を続けてください。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` にマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能を確認する

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同じように、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます（カスタム指示を更新する**前**にこのタスクを割り当てたことを思い出してください）。これまでと同様に、**Conversation** タブにコメントを追加すると Copilot が内容を確認し、新しいセッションを開始します。

## アクセシビリティ機能を確認する

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認しましょう。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカル ストレージの使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って Copilot の作業を導いたことが分かります。アクセシビリティに関する問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 作業を委任する

cloud agent は、実際のバックログ項目を任せて、ほかの作業に集中するときに役立ちます。リポジトリで issue をいくつか作成し、Copilot に割り当ててみましょう。たとえば、次のような作業を依頼できます。

- ゲーム詳細ページに支援者向けの興味登録フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンで、次のことを行いました。

- **このリポジトリにあるカスタム指示ファイルを確認し、cloud agent の出力に反映されることを確認しました。**
- **issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ向けのカスタム エージェントを作成して使用し、ハイ コントラストとライト モードの切り替え機能を追加しました。**
- **Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して誘導しました。**
- **Copilot のプルリクエストを確認して改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent のハーネスを完了しました。引き続き学習する場合は、ほかのハーネスも試してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を使用します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ提供します。

自分のリポジトリで、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- ほかの分野向けにカスタム エージェントを作成する。
- 異なるスタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
