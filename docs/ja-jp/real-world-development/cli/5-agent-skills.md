---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

コードを書くこと以外にも、テスト、lint、その他の継続的インテグレーション (CI) に関する作業があります。これらのタスクには、Copilot が適切に実行する方法を理解するための**エージェントスキル**が役立ちます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` スキルを確認する。
- 結果の形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要なときに読み込む再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキル名と説明を定義する frontmatter、概要、呼び出すべき状況の説明が含まれます。スキルのフォルダーには、スクリプトやその他のリソース用のサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクト既存のコマンドを使うため `SKILL.md` のみで構成されています。

スキルは `.github/skills` に配置してチームで共有・再利用できるリポジトリアセットにするか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックに使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot がいつスキルを呼び出すかを判断しやすくなります。
3. 指示を読み、テストと lint の手順がどのように示されているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。単体テスト、lint、型チェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したか失敗したか、ツールが実際に何を報告したかが分かるレポートにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前と同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

このレッスンでは、単体テスト、lint、型チェックのための `quality-checks` スキルを確認し、結果の形式をカスタマイズして、再読み込みして実行しました。この変更は、フィルター機能の PR と一緒に提出します。

次は、[Playwright MCP サーバーを使って][next-lesson] Copilot がサイトとやり取りできるようにします。

## スキルの例

以下のコミュニティの例は参考資料であり、このレッスンで追加作業を行う必要はありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
