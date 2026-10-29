---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際、コンテキストは重要です。作業方法が決まっている場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパス別の命令ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント規約が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue を非同期で処理するため、リアルタイムで影響を並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを作成します。
- UI はダーク モードで、現代的な印象にします。
- TSDoc コメントを使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの内容を説明するコメントを追加します。

命令ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝えられます。リポジトリ全体、または特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一のファイルです。多くの Copilot Chat または CLI リクエストに関係する技術スタック、プロジェクトの概要、ベスト プラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript などの言語、UI コンポーネントの作成、新しいテストの作成などを対象にできます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、別の規格を使う命令ファイルにも対応しています。詳細は[カスタム命令の対応状況][custom-instructions-support]を確認してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な構成の例があります。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体に適用されるコーディング規約など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する命令を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

命令ファイルの作成方法は 1 つではありません。AI の使い方と同じように、実際に試して自分の環境やシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成させることもできます。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。利用中の環境に関連するレッスンで、操作方法を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、主要な `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob で、たとえば `**/*.test.ts` に該当する TypeScript のテスト ファイルに命令を適用します。
5. プロジェクトでユニットテストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな命令を再利用可能な小さなファイルに分け、コード生成の参考となる例にリンクできます。ここで使われるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルのコメント ヘッダーを追加します。

## リポジトリの標準を追加する

この演習で編集するのは、コード内のドキュメントを TSDoc コメントとファイル レベルのコメント ヘッダーで記述するという、プロジェクト全体の規則です。後続の演習で Issue を割り当てると、cloud agent がこの規則に従います。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチから命令を読み込みますが、変更を分けてコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト レベルの情報を記載して、コードの提案を改善します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング規約はありますが、コード内のドキュメント作成ルールがありません。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、命令ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。ここで追加したドキュメント作成規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 命令の変更をコミット、プッシュ、マージする

cloud agent は、対象 Issue のブランチにある命令ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。命令を使えるようにするには、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent がこの命令を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成手順を省略して、`main` に直接コミットします。ブランチを使うのは、実際のプロジェクトでの手順を再現するためです。

## まとめと次のステップ

Copilot がこのプロジェクトの命令ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にプロジェクト全体の標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この規則を確認します。

次は [Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
