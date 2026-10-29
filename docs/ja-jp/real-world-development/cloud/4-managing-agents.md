---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進捗を確認することも重要です。

cloud agent に割り当てたタスクを管理するには、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールを使えます。このミッション コントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを作成するよう依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業が進んでいるため、ライト モードも含めるのが合理的です。エージェントに追加の指示を送り、ライト モードとハイ コントラスト モードの両方を実装させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にタスク一覧が表示されます。次のようなタスクが含まれていることを確認します。
   - コードベースのドキュメントを更新する
   - ゲーム変更用の API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択して、Copilot が行った作業を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各手順などが表示される場合があります。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## Cloud agent に追加の指示を送る

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。タイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールに表示されたアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分待ちます。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![cloud agent タスクの画面で、作業中のセッションに指示するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込んだことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要件を作業に加えます。

前と同じように完了には数分かかります。その間に、ここまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視・案内する中心的な場所である Copilot agents ページを確認しました。進行中と完了済みのタスクを追跡し、実行中の作業を確認し、進行中のタスクに追加の指示を送って範囲を調整できます。

このレッスンでは、次の操作を行いました。

- ミッション コントロールとエージェント ページを使って cloud agent のタスクを確認しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、作成されたプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
