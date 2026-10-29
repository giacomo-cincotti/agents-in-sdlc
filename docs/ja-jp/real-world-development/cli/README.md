---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys の 2 つの変更を構築、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、ひと続きの Tailspin Toys ワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価の変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの命令を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. quality assurance (QA) カスタム エージェントを作成し、要件、テスト範囲、検証結果をレビューします。
8. フィルター機能全体をレビューし、Agent Merge を使ってフィルター PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウドへの委任に使う便利なスラッシュコマンドを確認します。

ワークショップでは、星評価と、命令の更新・スキルの更新・QA プロファイル・テストを含むフィルター機能の 2 つの PR を作成します。フィルター機能と品質確認の作業では、同じ会話とブランチを使い、ツールを学びながら内容を積み上げます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価の追加: すぐできる改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認・更新して、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルターの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成・選択し、最後に検証結果を収集する |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、任意のクラウドへの委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、次の学習リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいたエージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] リポジトリと Codespace を作成する権限

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
