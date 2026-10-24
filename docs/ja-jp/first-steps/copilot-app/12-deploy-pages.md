---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プル リクエストを確認して、Space Quiz の公開を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更を確認し、サイトが公開されたことを確かめるのは自分自身です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイ設定を依頼する。
- マージする前にワークフローとプル リクエストを確認する。
- デプロイの進行を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプル リクエストのマージ、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の実行、yourname.github.io/space-quiz/ で公開された Space Quiz を示す 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、agent にプル リクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローを確認してデプロイを検証する

1. `README` がリポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プル リクエストの変更を確認します。
3. 必須のプル リクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

agent が Pages を有効にできない場合やプル リクエストをマージできない場合は、提示された具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、作業が完了したとは判断しないでください。

## まとめと次のステップ

Space Quiz をライブ サイトとして公開し、agent の報告だけに頼らずデプロイを確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson] に進みましょう。

[next-lesson]: ../13-review/
