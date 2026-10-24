---
title: "レッスン 2 - star rating を追加する: まずは簡単な変更"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。変更は小さくします。ゲームのデータにはすでに star rating がありますが、ホーム ページのゲーム カードには表示されていません。agent に rating を表示してもらい、変更を確認して、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始める。
- agent にプロジェクトの小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリケーションを実行し、転送されたブラウザーで変更を確認する。
- 最初のプル リクエストを開いてマージする。

## シナリオ

Tailspin Toys の各ゲームには star rating があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのはタイトル、カテゴリ、パブリッシャー、説明だけです。最初の練習として、agent に各カードで既存の rating を表示してもらいます。小さく独立した変更なので、最初の session に適しています。

## 会話の構成

**会話** は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話は、ターミナルで現在 checkout しているリポジトリと Git ブランチを使用します。保存した会話を後で再開できますが、ファイルとブランチは通常の Git の状態としてディスク上にあります。

会話では、プロンプトと agent の応答、ファイルを探索・編集する agent のツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を作成します。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. 次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を入力して <kbd>Enter</kbd> を押し、新しい session を開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. rating が設定されているときは数値が表示され、`starRating` が `null` のときは `No rating yet` が表示されることを確認します。
3. テストで両方の状態が確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同じく Copilot は確率的に動作するため、コードは例と完全には一致しません。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで、正しく動作すると判断することはできません。Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから UI を確認します。

1. Copilot にアプリケーションの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の rating が表示されることを確認します。
4. Copilot に戻り、起動したサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを開いてマージする

機能を作成できました。コードをプロジェクトにマージするプル リクエスト (PR) を作成します。

1. default agent に、変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に Copilot CLI が既存の commit を push し、PR の URL を表示します。
3. Copilot CLI に表示された URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選び、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで、目的を絞った Copilot の会話を始めた。
- agent にゲーム カードの小さな変更を依頼した。
- `/diff` で変更をレビューした。
- アプリケーションを実行し、転送されたブラウザーで star rating を確認した。
- 最初のプル リクエストを開いてマージした。

次は [filtering issue を起点に Plan と Autopilot mode を使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
