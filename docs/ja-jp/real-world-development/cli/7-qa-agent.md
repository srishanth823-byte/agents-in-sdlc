---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上の filtering を確認しました。次は、明確な QA プロセスを定義する custom agent にこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する方法を確認する。
- 再利用できる quality assurance profile を作成して確認する。
- QA agent を選択し、filtering issue に照らして結果をレビューする。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。custom agent を使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## custom agent とは

custom agent は、Markdown profile で定義する Copilot の専門的な役割です。profile には目的、instruction、使用可能な tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instruction はチームの標準を説明し、`quality-checks` skill は繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー操作 tool を提供します。QA profile は、これらの機能を使って要件を評価し、結果を報告する方法を Copilot に伝えます。これらに置き換わるものではなく、別の会話も必要ありません。

## QA profile を作成する

機能 PR を開く前に、Copilot に再利用可能な QA profile を作成してもらいます。profile では、実施するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、filtering の会話が Interactive mode であることを確認します。
2. default agent に次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使用する前に、意図した QA ワークフローと境界が profile に反映されているかを確認します。これにより、検証だけを行いたいときに、agent が機能を変更してしまうことを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、指定すると agent の表示名が明確になります。
3. instruction を読み、QA が要件から始め、リポジトリ instruction に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. QA が根拠を含めて報告し、実装コードを変更する前に確認を求め、commit やプル リクエストの作成をしないことを確認します。
5. いずれかの責務や境界が不足している場合は、続ける前に default agent に修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile を作成したら同じ filtering の会話を再開し、QA を選択します。issue と計画時の決定事項をそのまま利用できます。

1. 次の prompt で agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりの agent は、利用可能な agent の一覧に表示されないことがあります。上記の command で有効にできます。

2. 次の prompt で機能をレビューするよう QA agent に依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を始めます。
4. 作業が終わったら、レポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門の役割を追加し、その結果を確認しました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool と連携する方法を確認した。
- 再利用できる quality assurance profile を作成して確認した。
- QA agent を選択し、filtering issue に照らして結果をレビューした。

実装、skill の更新、QA profile、テスト、検証レポートが揃いました。次は[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用の custom agent を作成する][create-agents]
- [custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
