---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際、コンテキストは重要です。作業方法が決まっている場合は、Copilot がそのガイダンスを参照できるようにします。特定のコンテキストを Copilot と共有する方法はいくつかあります。コード生成のガイダンスを提供する[命令ファイル][instruction-files]は、その主な方法の 1 つです。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパス別の命令ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント規約が Copilot にどう伝わるかを確認します。
- 既存の命令を使って、フィルター機能の最初のデータ部分 (発行者ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従うことを確認します。
- 命令の変更とフィルターの一部を `main` にコミットし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した規約の一部から外れる場合があります。Copilot の動作は確率的です。この演習では、出力の一字一句を一致させることではなく、命令を更新した後の動作の変化を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一のファイルです。多くのリクエストに関係する技術スタック、プロジェクトの概要、ベスト プラクティスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript などの言語、UI コンポーネントの作成、新しいテストの作成などを対象にできます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> AGENTS.md や CLAUDE.md など、別の規格を使う命令ファイルにも対応しています。詳細は[カスタム命令の対応状況][custom-instructions-support]を確認してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な構成の例があります。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体に適用されるコーディング規約など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する命令を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

命令ファイルの作成方法は 1 つではありません。AI の使い方と同じように、実際に試して自分のプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] などのタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成させることもできます。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。利用中の環境に関連するレッスンで、操作方法を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイルやカスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、主要な `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートから見たパスで、たとえば `**/*.test.ts` に該当する TypeScript のテスト ファイルに命令を適用します。
5. プロジェクトでユニットテストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや既存ファイルへのリンクを確認します。命令を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。ここで使われるパスは命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルのコメント ヘッダーを追加します。

## 変更用ブランチを作成する

ここではベスト プラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作に進む前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順の中で説明します。

まず現在の命令ファイルでプロンプトを送り、カスタム命令の影響を確認します。後で命令を更新した後、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習で使ったエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパー ファイルとテストなどにコードを適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きの発行者配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に沿っています。
7. 提案されたコードには、TSDoc コメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習では、命令を更新した後の一貫性の向上に注目します。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載し、コードの提案を改善します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング規約はありますが、コード内のドキュメント作成ルールがありません。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルは、リポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。ここで追加したドキュメント作成規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選び、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を追加して、次の演習につながる実際のコードに適用できました。

## 命令とブランチをコミットしてプッシュする

命令ファイルも他の資産と同じようにソース管理で扱います。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令ファイルと Copilot がヘルパーのために変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に他の関連変更 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けたプルリクエストを作成します。GitHub のツールはプロジェクトに接続済みなので、Copilot に Issue を探し、PR を作成してマージするよう依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクト テンプレートでは GitHub のツールがワークスペースに設定済みです。初回利用時に VS Code から GitHub へのサインインを求められた場合は、表示される手順に従ってください。後の演習で、この接続に使われている Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 命令ファイルの更新と発行者ヘルパーの土台を説明し、現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して適切な操作を **Allow** します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの許可を求められたら、操作を確認して適切に **Allow** します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、Copilot Chat の VS Code agent mode を使って次の操作を行いました。

- 現在の命令でコード生成のプロンプトを送り、その結果を確認しました。
- `.github/copilot-instructions.md` にプロジェクト全体の標準を追加しました。
- 同じプロンプトをもう一度送り、提案されたコードに新しい標準が反映されることを確認しました。
- 命令の変更とヘルパーの土台をコミットし、次の演習で利用できるよう `main` に反映しました。

次は、Copilot がコードベース全体に新機能を追加するときに、[agent mode で命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [カスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
