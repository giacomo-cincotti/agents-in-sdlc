---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、作業に必要な命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門性、命令を説明する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。この演習では、アクセシビリティ エージェントがサイトをアクセシビリティ ガイドラインに照らして確認・更新します。その過程で、プルリクエスト ワークフローのスキルやテスト実行・管理のスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI の他の用途と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを学びます。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマについて、テキストと背景色のコントラストが不十分で読みづらいと感じるユーザーがいることがわかりました。これに対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を読み、Copilot をどのように導くか確認しましょう。

Codespace に戻り、アクセシビリティ エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべき失敗

## Issue を作成して割り当てる

エージェントのミッション コントロールは、環境内のエージェント作業を管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを伝えられます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選びます。

    ![カスタム エージェントとアクセシビリティが強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

このレッスンでは、次の操作を行いました。

- カスタム エージェントの定義方法を学びました。
- タスクをカスタム エージェントに割り当てました。

Copilot がハイ コントラスト モードの実装に取り組んでいる間に、ミッション コントロールから[エージェント セッションを監視して案内します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
