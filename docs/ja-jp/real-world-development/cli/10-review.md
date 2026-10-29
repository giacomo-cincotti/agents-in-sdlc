---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター Issue を見つけました。
- 星評価を追加し、ポート転送したブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を確認しました。
- フィルター機能全体をレビューし、Agent Merge を承認してフィルター PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` から作成した個別のブランチで、2 つの PR を作成します。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価であることを明示する表示を追加します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、命令を更新して機能に適用し、`quality-checks` のレポート形式をカスタマイズし、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを効率よく進めるため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは新しい形式のレポートでユニットテスト、lint、型チェックを実行しました。QA は PR 作成前に、要件とテスト範囲のレビューをこれらの結果と組み合わせました。

追加するテストは、実際の不足を解決するものである必要があります。新しいテストが不要という QA の結果が正しい場合もあります。マージを承認する前にコードと根拠を確認し、変更後は影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に渡すコンテキストとツールによって、作業の結果が変わります。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は規約を定め、スキルは繰り返し可能なタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。構築の前に方法を検討するには **Plan**、範囲を絞った変更に参加し続けるには **Interactive**、明確に定義したタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。構築したいもの、その理由、方法を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを完了しました。ほかにも、次の CLI 機能を試せます。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実際のコード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
