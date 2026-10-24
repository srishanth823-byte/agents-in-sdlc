---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、次のレッスンで Copilot CLI をインストールして実行するための統合ターミナルを備えた [Codespace][codespaces] を起動します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ワークショップ用リポジトリをセットアップする

自分専用の Tailspin Toys プロジェクトを使います。[テンプレート リポジトリ][tailspin-template] からコピーを作成してください。新しいリポジトリには、ワークショップに必要なすべてのファイルが含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template] に移動します。
2. **Use this template** を選択し、続けて **Create a new repository** を選択して、自分専用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えてください。ワークショップの後半で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこれらの issue を使用するため、自分で登録する必要はありません。

ワークショップ用テンプレートの最新コピーを使用してください。必要なリポジトリ instruction、アプリケーション コード、テスト、`quality-checks` skill、バックログが含まれています。古いコピーを使う場合は、必要なファイルが揃っているか講師に確認してください。

## Codespace を作成する

次に、Codespace でワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応したエディターを利用できます。

1. 作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカル データベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択して Tailspin Toys サイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備が整いました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

次は Codespace に [GitHub Copilot CLI をインストール][next-lesson] し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
