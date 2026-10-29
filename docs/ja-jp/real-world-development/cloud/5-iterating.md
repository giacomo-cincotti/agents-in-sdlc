---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的とする複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チームが試せる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ自律的に作業するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みできるのはコード用ブランチだけです。
- Cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行されます。
- GitHub Actions のワークフローを実行するには、人による承認が必要です。
- MCP サーバーなど、外部リソースへのアクセスは既定で制限されています。

[外部リソースへのアクセス制限][agent-firewall]も確認してください。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加する Copilot cloud agent の最初のプルリクエスト (PR) を、GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR を確認するときに、ファイアウォールで GitHub Copilot がブロックされたという警告が表示されることがあります。Copilot は既定で外部 MCP サーバーなどへのアクセスが制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は完了するまで待ちます。数分かかることがあります。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントなどのドキュメントがコードに追加されていることを確認します。具体的な変更内容は異なります。演習 2 の開始時に確認した命令ファイルが反映されているか、TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同様に、PR コメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、指定のプロンプトはありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、内容を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントに対する変更をコメントで依頼します。他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して、Copilot が作業する様子を確認します。変更用の新しいセッションが始まります。
3. **Back to pull request** を選ぶと PR に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブで変更をレビューします。

問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` にマージできます。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルの PR を選びます。
4. **Files changed** タブで生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。単一の Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を構築しました。前と同じように、Copilot cloud agent に新しいコメントを追加して変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc コメントを追加したりできます。この Issue はカスタム命令を更新する前に割り当てたため、その点も確認してください。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 のミッション コントロールから依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルの PR を選びます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。数分待って完了を確認してください。

4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 次の項目に注意して実装を確認します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の利用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - 表示モードの切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録され、単一の Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルで Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリを起動し、ハイ コントラスト モードとライト モードが期待どおりに動作し、ページの再読み込み後も選択が保持されることをブラウザーで確認します。

アクセシビリティ用のカスタム エージェントが、ベスト プラクティスに沿った実装を Copilot に促していることを確認できました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

実際のバックログ項目をエージェントに任せ、自分は別の作業に取り組めるのが cloud agent の利点です。いくつか Issue を作成して Copilot に割り当てる練習をしましょう。たとえば、次のような内容があります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の操作を行いました。

- リポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力への影響を確認しました。
- Issue を Copilot cloud agent に割り当て、非同期に環境をセットアップし、計画して実行する様子を確認しました。
- アクセシビリティ向けのカスタム エージェントを作成して使用し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使い、アクセシビリティ セッションを監視して案内しました。
- Copilot のプルリクエストをレビューして反復し、**@copilot** に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスが完了しました。さらに学びたい場合は、ほかのハーネスも活用してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの 2 つの変更をリリースします。

自分のリポジトリで、次のアイデアも試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
