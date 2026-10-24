---
title: "レッスン 3 - agent mode: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue の filtering を実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらにしっかりした手順が必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、正しい方法で必要なものを構築できます。このレッスンから、issue を基に機能を作り、コードが有効で機能が期待どおりに動作することを確かめ、最終的にプロジェクトへマージする、一般的な agent 主導の開発プロセスを学びます。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常は作業対象ごとに別のブランチや PR を使いますが、ここでは基本的な概念に集中できるよう手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を始める。
- Plan mode で要件を定める。
- Autopilot mode で新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

機能開発を続けながら、リポジトリ instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリとパブリッシャーでゲームを絞り込める必要があります。バックログの issue に機能の概要は記載されていますが、カテゴリの組み合わせなど、コーディング前に決めるべき詳細があります。Plan mode でその方針を決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI coding agent を開発フローに導入しても、基本は変わりません。むしろ重要性は高まります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始める。
2. 実装する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新しい機能を手動で確認する。
6. プル リクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れを応用しています。

この標準的な方法に沿って進めると、AI が生成したコードが要件を満たし、手書きのコードと同様の確認プロセスを経るようにできます。

## 会話 mode

**会話 mode** は、agent にどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと mode を切り替えられます。

- **Interactive**: agent と共同で作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent が先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: agent がコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan mode で計画を確認してから、Autopilot で実装します。

## issue から始める

filtering の作業を始める前に Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. star-rating PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、filtering 用のブランチを作成します。

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
8. **Allow users to filter games by category and publisher** というタイトルの issue を矢印キーで選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプル リクエスト (PR) をコンテキストに追加できます。

## filtering 機能を計画する

計画を立てると、機能やタスクの実装を Copilot に任せる前に、進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. filtering の動作について質問された場合は、希望する動作に基づいて回答します。どの回答にも間違いはありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決定した内容と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot は、タスクが完了したと判断するまで Copilot が問題に取り組めるようにします。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンにある同様の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。先に計画を確認してください。依存関係の不足やポートの競合が報告された場合は、チェックが完了したと判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して正しく動作することを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。mode 表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、filtering の実装とテストを確認します。
3. issue と計画時の決定事項に照らし合わせて結果を確認します。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、アプリケーションを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリケーションの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画したカテゴリ、パブリッシャー、および組み合わせの filtering を試します。
4. リセット時や結果が空の場合の動作が issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話 mode を使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を始めた。
- Plan mode で要件を定めた。
- Autopilot mode で新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、コードが文書化されたプラクティスに従うよう、[custom instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
