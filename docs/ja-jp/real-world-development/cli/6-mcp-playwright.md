---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーで filtering 機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱ったり、Copilot が追加の自動化を使えるようにしたりする必要があります。MCP server を使うと、CLI に組み込まれた機能を超えて、Copilot にさらに多くの tool とサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解する。
- まだ利用できない場合は Playwright MCP server を追加する。
- agent にブラウザーを操作させて filtering 機能を調べる。

## シナリオ

unit test と end-to-end test は重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot に開発中のサイトをユーザーとして利用させることで、変更が期待どおりに動作するかをより確実に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部 tool やサービスと通信する方法を提供します。MCP を使うと、AI agent は外部 tool やサービスとリアルタイムに通信し、最新情報にアクセスしたり、代わりに操作を実行したりできます。

これらの tool と resource には、AI agent と外部サービスをつなぐ MCP server を通じてアクセスします。各 MCP server は、agent が利用できる別々の tool と resource のセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プル リクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多数の MCP server が利用できます。GitHub はエコシステムの発見性とコントリビューションを高めるため、[MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーと同じようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、セットアップ手順を飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次の command を入力し、MCP server の追加 dialog を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** dialog に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP dialog を終了します。

## Playwright で機能を調べるよう Copilot に依頼する

先ほどは機能が期待どおりに動作することを手動で確認しました。次は Copilot CLI の Playwright MCP server を使って同じ確認を行います。

1. 次の prompt で、Playwright MCP server を使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自分で MCP server の使用を判断するため、明示的に指示する必要はありません。使う tool が分かっている場合は、prompt に含めると結果がより一貫し、token の節約にもなります。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確かめます。

Copilot はサーバーを起動し、Playwright でサイトを操作して、サーバーを停止した後に結果を報告します。

## まとめと次のステップ

Playwright MCP server を使って、Copilot CLI から実際のブラウザーで機能を調べました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学んだ。
- まだ利用できない場合に Playwright MCP server を追加した。
- agent にブラウザーを操作させて filtering 機能を調べた。

次は skill とブラウザー tool を組み合わせる [QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
