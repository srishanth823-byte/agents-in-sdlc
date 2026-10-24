---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデル選択、セッション共有、任意の cloud delegation に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、CLI を操作するための slash command が多数あります。高度な機能、内部の状態、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利な command を確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認する。
- `/model` で利用できるモデルを確認する。
- `/share` でセッションを export または共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使う任意の command を確認する。

## シナリオ

CLI の基本的なワークフローが終わりました。ここからは、コンテキスト管理、モデル切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を見ていきます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大 context window に近づくことがあります。Copilot CLI は必要に応じて会話を自動で compact します。slash command を使って状態を確認したり、自分で compact したりすることもできます。

1. Codespace に戻り、まだ開いていなければリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、system instruction、tool、message、空き領域の内訳を確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比べます。会話が短い場合は、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI は context window がいっぱいになると自動で compact します。タイミングを選びたい場合は `/compact` を使います。関係のないタスクに切り替えて新しい会話を始める場合は、代わりに `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI tool の使い方を改善できます。`/share` command はセッションを Markdown または HTML ファイルに export したり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合う方法を選びます。たとえば `/share file` は Markdown ファイルとしてローカルに export します。
3. export した内容を送信または公開する前に確認します。セッションの export には prompt、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 並列作業や委任を行う

ワークショップの基本部分は完了しました。Copilot CLI には、大きなタスク向けの command もあります。

- `/fleet` は独立したサブタスクを subagent に分け、並列に実行できます。
- `/worktree` は別のタスク用に分離された Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で作業させます。プル リクエストが作成される場合もあります。

これらの command は追加の worktree やリモート作業を作成することがあるため、任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期 agent の作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定の変更、セッションの共有、内部状態の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認した。
- `/model` で利用できるモデルを確認した。
- `/share` でセッションを export または共有する方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使う任意の command を確認した。

slash command はほかにもあります。最後に[学んだ内容を振り返り][next-lesson]、今後の学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
