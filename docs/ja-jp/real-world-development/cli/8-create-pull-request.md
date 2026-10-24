---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "filtering、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

filtering の実装、instruction の更新、skill の更新、quality assurance (QA) profile、テストが、1 つのブランチに保存されています。まとめてレビューし、プル リクエストを開きましょう。star-rating のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** に処理を任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。ワークショップを簡潔にするため、filtering と品質確認の作業全体を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能全体の変更と検証の根拠を確認する。
- filtering の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

filtering の作業を通して、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プル リクエストを完了させるために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗修正、レビュー コメントへの対応、必要に応じた rebase など、PR をブロックしている問題に対処します。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

ここまでは自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることもできますが、有効にする前に変更を確認し、問題がないと判断してください。

## Agent Merge で PR を管理する

コードが揃ったらまとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` command で新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功するとマージを実行します。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を読み、リポジトリで許可されている方法でレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学んだ。
- 機能全体の変更と検証の根拠を確認した。
- filtering の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意の cloud delegation に役立つ [Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
