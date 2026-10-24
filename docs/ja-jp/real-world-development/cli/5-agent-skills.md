---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして filtering を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。ここまで、手動でコードを検証し、instruction file で標準に従うようにしました。次はテスト、lint、継続的インテグレーション (CI) のほかの作業を確認します。

このようなタスクには **agent skill** が適しています。skill を使うと、こうした操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認する。
- 結果の形式をカスタマイズする。
- skill を再読み込みして実行する。

## シナリオ

Tailspin Toys は unit test、lint、type check に `quality-checks` skill を使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## instruction、script、resource

agent skill は、再利用可能なタスク instruction、実行可能な script、補助 resource をまとめ、必要に応じて agent が読み込めるようにします。基本的には、skill 名のフォルダーにある `SKILL.md` ファイルで構成されます。Markdown には skill の名前と説明を定義する frontmatter、skill の概要、呼び出す条件のガイダンスが含まれます。フォルダーには script やほかの resource を置くこともできます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトの既存 command を使うため `SKILL.md` だけで構成されています。

skill は、チームが共有・再利用できるリポジトリ内の `.github/skills` フォルダー、またはユーザーの `~/.copilot/skills` フォルダーに置けます。

## skill を確認する

Tailspin Toys チームが unit test、lint、type check のために作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。説明は、Copilot が skill をいつ呼び出すか判断するのに役立ちます。
3. instruction を読み、テストと lint の手順がどう示されているかを確認します。
4. まだ **Results output formatting** セクションがないことに注目します。

## 変更前に skill を実行する

Copilot に 3 つのチェックを実行してもらいます。

1. Interactive mode で filtering の会話に戻ります。
2. 次の prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、tool が実際に報告した内容を示す、より分かりやすいレポートにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

Copilot CLI は会話を再起動せずに、編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じ prompt を入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- unit test、lint、type check 用の `quality-checks` skill を確認した。
- 結果の形式をカスタマイズした。
- skill を再読み込みして実行した。

この変更は機能 PR に含まれます。次は [Playwright MCP server を使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## skill の例

以下のコミュニティの例は参考情報であり、追加の作業ではありません。

- [Agent Skills specification][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
