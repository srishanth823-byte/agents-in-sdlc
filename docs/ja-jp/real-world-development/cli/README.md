---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プル リクエストの自動化を学びながら、Tailspin Toys の変更を 2 件実装、検証、完了します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、GitHub Copilot をエージェント型のコーディング アシスタントとしてターミナルから利用できます。コードベースの探索、コードの生成、コマンドの実行、外部ツールへの接続をすべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連の作業に取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
2. star rating の小さな変更を行い、ブラウザーで確認して、最初のプル リクエスト (PR) を手動でマージします。
3. filtering の issue を起点に、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの instruction を更新し、filtering 作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーで filtering を調べます。
7. quality assurance (QA) custom agent を作成し、要件、テスト範囲、検証の根拠を確認します。
8. filtering の変更全体をレビューし、Agent Merge を使って filtering PR をマージします。
9. コンテキスト、モデル、共有、任意の cloud delegation に役立つ slash command を確認します。

ワークショップでは、star rating と、instruction・skill の更新、QA profile、テストを含む filtering の 2 件の PR を作成します。filtering と品質確認の作業では同じ会話とブランチを使い、各ツールを学びながら変更を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----------- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. star rating を追加する: まずは簡単な変更][ex2] | 最初の変更 | 既存 rating と null の場合の代替表示を行い、最初の PR をマージする |
| [3. agent mode: Plan と Autopilot][ex3] | agent mode | issue から機能を計画し、Autopilot で実装して Interactive mode でレビューする |
| [4. custom instruction で Copilot を導く][ex4] | コンテキスト | instruction を確認・更新し、filtering に適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 再利用可能なチェック | 既存 skill を確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、filtering の動作を調べる |
| [7. QA agent を作成して使う][ex7] | 要件とテスト範囲 | 専門 profile を作成・選択し、最終的な検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、次のリソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログを参照する agent をデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、code review、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
