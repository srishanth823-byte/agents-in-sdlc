---
title: "レッスン 4 - custom instruction で Copilot を導く"
description: "リポジトリ instruction を確認し、ドキュメント標準を追加して、filtering のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクの進め方に指定がある場合は、そのガイダンスを Copilot が利用できるようにします。[instruction file][instruction-files] は、必要なコードだけでなく、コードの構成方法も説明します。filtering を実装したので、Copilot が使用した instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ instruction とパスに適用される instruction file が agent にどう渡されるかを確認する。
- コーディング標準に従うよう instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` の export 関数は、用途、パラメーター、戻り値を TSDoc/JSDoc で記述する。`db` 引数を注入する場合は、そのことも記載する。
- 再利用可能な Astro component は `Props` の契約を文書化し、関連するコードを変更したときにコメントを最新に保つ。
- 既存の書式設定と lint のガイダンスを維持する。

instruction file を使って、Copilot がこれらのプラクティスに沿って作業できるようにします。

## instruction file

custom instruction を使うと、コンテキストや希望を Copilot に伝え、コーディング スタイルや要件を理解させることができます。これにより、より適切な提案やコード スニペットを得られます。コーディング規約、使用するライブラリ、コード コメントの形式を指定できます。リポジトリ全体に適用する instruction と、特定のファイル向けの instruction を作成できます。

instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の instruction file です。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式や対応状況は環境によって異なります。特定の形式を使う前に、[custom instruction の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには複数の instruction file が含まれています。変更する前に内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この instruction は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ レイヤー、テストなどの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、instruction の適用対象ファイルを決める glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存ファイルへのリンクを確認します。大きな instruction セットを小さく再利用可能なファイルに分割し、Copilot が参考にする例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルは出発点として役立ちますが、まだ不足があります。中核となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc comment を追加するようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の行を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリ instruction を読み込みます。instruction を保存したら、filtering の会話を終了して再開し、機能のコンテキストを保持したまま新しいガイダンスを適用します。

1. filtering の会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替え、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたら filtering の会話を選び、`game-filters-cli` ブランチが checkout されていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc comment が追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリ instruction とパスに適用される instruction file が agent にどう渡されるかを確認した。
- コーディング標準に従うよう instruction file を更新した。
- instruction file がコードに与える影響を確認した。

次は、[再利用可能な `quality-checks` skill をカスタマイズして実行][next-lesson]し、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用の custom instruction を追加する][instruction-files]
- [custom instruction の対応状況][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
