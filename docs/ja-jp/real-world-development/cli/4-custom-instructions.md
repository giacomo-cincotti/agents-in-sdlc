---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の規約を追加してフィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストは重要です。作業の方法を指定したい場合は、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルター機能を構築したので、このレッスンでは Copilot が使った命令を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパス別の命令ファイルがエージェントにどう適用されるかを確認します。
- コーディング規約を守るために命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入する場合は、その内容も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変更された際にコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使い、Copilot が適切なガイダンスを得て、チームの開発方針に沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝えられます。命令により、より適切な提案やコード スニペットを得やすくなります。リポジトリ全体、または特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一のファイルです。多くの Copilot Chat または CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> その他の命令形式と対応状況は、環境によって異なります。特定の形式を利用する前に、[カスタム命令の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

開始しやすいよう、スターター プロジェクトには命令ファイルが用意されています。変更の影響を確認する前に、既存のファイルを確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、リポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、命令の適用対象となるファイルを指定する glob を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイルや既存ファイルへのリンクを確認します。命令を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルはよい出発点ですが、追加できる規約があります。`copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルター機能のコンテキストを保ったまま新しい命令を読み込めるよう、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの命令とパス別の命令ファイルがエージェントにどう適用されるかを確認しました。
- コーディング規約を守るために命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令の対応状況][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
