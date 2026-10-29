---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを円滑に進めるため、フィルター機能と品質ワークフロー全体を1つのセッションとブランチで進め、すべての作業をこの PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学ぶ。
- 機能の PR 全体と検証の証拠を確認する。
- レビュー後にのみ Agent Merge を承認し、PR のマージを確認する。

## シナリオ

フィルター機能のワークフローを通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、GitHub Copilot app で pull request を取り込むために必要な残りの作業を自動化します。有効にすると、アプリのセッションが PR を読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、マージを妨げている問題に対処します。GitHub で許可され次第 PR をマージします。バックグラウンドで動作し、アプリを再起動しても継続し、PR がマージされると自動的に無効になります。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、コードの編集やマージには明示的な承認が必要です。マージを許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したので、Agent Merge に PR の処理を任せます。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。ドロップダウンの項目を選ぶだけでは開始されません。

Agent Merge は次の処理を行います。

- タイトルと説明を含む pull request を作成する。
- Issue からセッションを開始した場合は、説明本文で関連する Issue を参照する。
- リベースを行うか、ターゲットブランチとのマージ競合に対処する。
- CI を監視し、すべてのチェックが成功するようにする。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントを解決するために更新する。
- すべて成功した後に PR を自動マージできるようにする。

すべてが成功したら Agent Merge が PR もマージするように設定しましょう。

5. マージを許可する前に、PR の差分と検証の証拠を確認します。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントにマージを明示的に依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 処理を確認し、報告された阻害要因を解決します。続行前に PR の状態が **Merged** であることを確認してください。マージ権限が有効になっていることや、エージェントが動作中であることだけではマージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前に阻害要因を解決してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学んだ。
- 機能の PR 全体と検証の証拠を確認した。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認した。

次は、[既存のキャンバスを使ってトリアージキャンバスを作成し][next-lesson]、エージェントと作業を確認、計画、視覚化するための、より豊かな方法を学びます。

## リソース

- [GitHub Copilot app での Issue と pull request の管理][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
