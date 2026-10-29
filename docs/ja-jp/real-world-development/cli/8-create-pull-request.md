---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令の更新、スキルの更新、quality assurance (QA) プロファイル、テストが、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質確認の一連の作業を同じ会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルター PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、開発者がマージを承認する権限を保ちながら、PR の残りの作業を自動化しようとしています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合のリベースなどを行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。今後は Agent Merge にその作業を任せられます。有効にする前に変更をレビューし、準備ができていることを確認します。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべてのチェックが成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、アクセス許可の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている手順に従って、レビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の段階を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う便利な [Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
