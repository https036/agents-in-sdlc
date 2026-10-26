---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集や cloud agent の作業の確認に使う [codespace][codespaces] を起動します。

## ラボ用リポジトリを設定する

作成するコード用のリポジトリを用意するには、[テンプレート][template-repository]から自分用のインスタンスを作成します。新しいインスタンスにはラボに必要なファイルが含まれており、演習を通じて使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリ ページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。ラボの後半で使用します。

## codespace を作成する

次に、codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードの記述、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりずっと早く済みます。その間に GitHub Copilot のほかの機能を試してみましょう。次の演習で詳しく説明します。

> [!CAUTION]
> 今後の演習で codespace に戻ります。それまでは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらにも、スムーズに進めるための前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と作業するための codespace の作成を開始しました。

## 次のステップ

cloud agent が使うカスタム指示を追加しましょう。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
