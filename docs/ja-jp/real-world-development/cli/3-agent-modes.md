---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づいてフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織で既に使われている開発フローに沿って、適切な方法で必要な機能を構築できるよう設計されています。このレッスンから、エージェントを使った一般的な開発プロセスを進めます。Issue を起点に機能を実装し、コードが有効で動作することを確かめ、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- ポート転送したブラウザーで手動検証します。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリーや発行者でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、実装前に合意が必要な詳細があります。Plan モードで決定し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がいっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の流れに沿って進めます。

この標準的な手順に従うことで、AI が生成したコードも、要件を満たし、手作業で書いたコードと同じレビューを受けるようになります。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成し、プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、機能実装やタスクを Copilot に任せる前に、採用する方法を決められます。複雑な作業では、事前に時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を決めて計画を作成します。

3. フィルターの動作について質問された場合は、希望に沿って回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決定した内容に合わない箇所は、Copilot に修正を依頼します。

## Autopilot を承認する

計画が完成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** (またはインストール済みのバージョンで同様の名前の選択肢) を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると報告された場合は、セットアップの問題を解決してからチェック結果を確認します。

## 実装をレビューして検証する

コードの生成後は、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して正しく動作することを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. 結果を Issue および計画で決めた内容と照らし合わせます。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードを確認したら、実際に動かしてみましょう。前と同じようにアプリを起動し、Codespaces のポート転送を使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. Plan で合意したカテゴリー フィルター、発行者フィルター、それらの組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を構築し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- ポート転送したブラウザーで機能を手動検証しました。

次は、コードが文書化された規約に従うように、[カスタム命令を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
