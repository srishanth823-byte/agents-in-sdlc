---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意された filtering issue を見つけた。
- star rating を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージした。
- filtering issue を起点に、Plan mode で方針を決め、Autopilot mode で実装し、Interactive mode でレビューした。
- custom instruction で agent を導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、type check を実行した。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーで filtering を確認した。
- quality assurance (QA) custom agent を作成・選択し、要件、テスト範囲、skill の結果、ブラウザーの根拠を評価した。
- filtering の変更全体をレビューし、filtering PR に Agent Merge を許可した。
- コンテキスト、モデル、共有、任意の cloud delegation に使う slash command を確認した。

## 作成したもの

ワークショップでは、更新した `main` から作成した別々のブランチで、2 件の PR に取り組みました。

1. **star rating**: ゲーム カードに既存の `starRating` と、rating がない場合の明確な表示を追加する。
2. **filtering と品質確認のワークフロー**: filtering を実装し、instruction を更新して機能に適用し、`quality-checks` のレポート形式をカスタマイズし、QA profile と関連テストを追加する。

filtering の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 件の PR にまとめています。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot に行わせるブラウザー確認を使ってコードを検証しました。`quality-checks` skill は unit test、lint、type check を実行し、新しい形式で結果を報告しました。QA は要件とテスト範囲のレビューにそれらの結果を加え、PR の前に一緒に確認できるようにしました。

追加するテストは、実際の不足を補うものである必要があります。QA の結果、テストを追加する必要がない場合もあります。マージを許可する前にコードと根拠を確認し、変更後は関連する検証を再実行してください。

## ベスト プラクティス

Copilot に与えるコンテキストや tool が作業に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作成して、MCP server を設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。instruction は標準を設定し、skill は繰り返し可能なタスクを記述し、custom agent は専門的な役割を定義し、MCP server は外部 tool に接続します。agent の要約だけではなく、実際の変更と tool の結果を確認してください。

タスクに合わせて **mode とモデル** を選びます。実装前に方針を検討するには **Plan**、範囲を絞った変更を一緒に進めるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。日常的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

環境と同様にコンテキストも重要です。作りたいもの、理由、方法を明確に説明すると、出力が大きく変わります。

## さらに試すこと

基本のワークフローを終えました。ほかにも、次の CLI 機能を試せます。

- `/review` で code review agent に変更を分析してもらう。
- `/rubber-duck` で問題について話し、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どの tool も、使い続けることが上達への近道です。本番コードや趣味のコード、長年作りたいと思っていた小さなアプリケーションで試してみましょう。チームと学びを共有し、お互いの経験から学んでください。ドキュメントも確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
