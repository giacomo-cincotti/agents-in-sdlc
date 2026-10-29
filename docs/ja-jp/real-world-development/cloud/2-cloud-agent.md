---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

多くの組織で、未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能など、技術的負債が課題になっています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が完了すると、人がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確にした Issue が重要であることを理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを開発した担当者が十分なドキュメントを残していません。まず、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

デザイン チームは、ゲーム詳細ページに関連ゲーム (同じカテゴリーの他のタイトル) を表示して、ゲームを見つけやすくしたいと考えています。完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、ほかに優先度の高い Issue があります。

このような作業は優先順位が下がりやすく、Copilot cloud agent に適しています。Copilot が非同期で作業している間に別のタスクを進め、後で変更をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]と、Copilot は進捗を記録するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。レビュー可能になると、PR で通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作も、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進めて学び、試し、改善します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、Copilot を導くカスタム命令を確認しました。命令ファイルを扱ったことがあれば、`.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれる命令ファイルを読み取り専用で確認してください。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` に適用されます。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも、もう一度確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、環境の準備やセットアップ スクリプトが必要な場合があります。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係をインストールできます。このラボ用には事前に設定されています。単一の Astro アプリで使う Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使用されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が作業を始めるまで待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、まったく用意されていなかったりするプロジェクトは少なくありません。これは後回しにされやすい技術的負債で、開発効率を下げ、コードベースの保守や新しいメンバーの参加を難しくします。ドキュメントの作成は Copilot が得意とする作業です。Issue を Copilot cloud agent に割り当て、バックグラウンドで作業させます。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作業項目の一覧を作成したことを確認します。

> [!NOTE]
> 作業項目が表示されるまで数分かかる場合があります。Copilot は環境を準備し (先ほどのワークフローを実行します)、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. Copilot が提示した作業項目と、実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** を選択します。

   ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーの更新が必要な場合があります。

16. 実行中のセッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を把握し、計画を考え、コードを作成する様子が表示されます。

処理には数分かかる場合があります。cloud agent の利点は、他の作業をしている間に非同期でタスクを進められることです。もう 1 つタスクを割り当てた後、アプリケーションへの機能追加を進めましょう。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent に作業を分け、自分が別のタスクに取り組めるのは大きな利点です。デザイン チーム向けの関連ゲームの表示は時間のかかる作業ではないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![目の絵文字で Issue の作業中を示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、常時監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]について学びました。Issue を Copilot に割り当て、技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを非同期で進められます。

このレッスンでは、次の操作を行いました。

- コード生成環境をカスタマイズする方法を確認しました。
- 操作を安全に行うことの重要性を確認しました。
- 範囲を明確にした Issue が重要であることを学びました。
- Issue を Copilot に割り当てました。

Copilot がバックグラウンドで作業する間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
