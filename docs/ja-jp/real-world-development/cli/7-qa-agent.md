---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。今度は、これらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な quality assurance プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター Issue に対する調査結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントで QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルにはエージェントの目的、命令、使用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズは、それぞれ役割が異なります。リポジトリの命令はチームの規約を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。QA が行うチェックと守るべき範囲を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約を反映していることを確認します。これにより、検証だけを行いたいときに、エージェントの範囲が広すぎたり、意図せず機能を変更したりするのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定すると表示名がわかりやすくなります。
3. プロファイルの命令を読み、QA が要件から始め、リポジトリの命令に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. これらの役割や制約が欠けている場合は、続ける前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択すると、すでに共有されている Issue と計画を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、使用可能なエージェントの一覧にまだ表示されないことがあります。上記のコマンドを入力すると有効になります。

2. 次のプロンプトを使い、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的な役割を追加し、その動作を確認しました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター Issue に対する調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、機能 PR にまとめ、[Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
