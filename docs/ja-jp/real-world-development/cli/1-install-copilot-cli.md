---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意された filtering issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースの探索、コードの生成、コマンドの実行、外部 tool との連携をすべてコマンド ラインから行えます。タスクを任せたり変更を依頼したりしながら、作業に集中できます。まずはツールをインストールします。すでに使い慣れているツールを使って簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP server で filtering issue を見つける。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に作業するターミナルにその機能をもたらします。このレッスンでは、インストールと認証を済ませ、残りのワークショップで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次の prompt が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選びます。

> [!NOTE]
> Codespace では、GitHub session を通じてすでに認証されていることがあります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めてください。

## 基本を確認する

通常の shell prompt に入力した command は、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語は agent に送られ、slash command は会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みバージョンで利用できる command を確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の簡単な質問をして動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が利用可能であることを確認します。
6. 次の prompt で filtering issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の変更を終えた後、この issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI session は、ターミナルで現在 checkout しているブランチを使い、自動的に worktree を作成することはありません。変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、既定の権限範囲外の tool を使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動し、Codespace 内での承認 prompt を省略して演習に集中します。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境だけで使い、日常の開発用の既定値として alias に設定しないでください。詳しくは [tool の許可と拒否][allow-all-warning] を確認してください。

`--enable-all-github-mcp-tools` を使うと、後のレッスンで issue やプル リクエストを扱うための、読み取り / 書き込み GitHub MCP tool が有効になります。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub resource は実際のものです。公開またはマージする前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で instruction を変更したり agent を追加したりしたとき、`copilot --resume` を使って同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP server で filtering issue を見つけた。

次は、[最初の変更を行い][next-lesson]、Copilot CLI でゲーム カードに star rating を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
