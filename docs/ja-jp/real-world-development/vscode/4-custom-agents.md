---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、作業に必要な命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門性、命令を説明する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティ用のカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らして確認・更新します。その過程で、プルリクエスト ワークフローのスキルやテスト実行・管理のスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI の他の用途と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマについて、テキストと背景色のコントラストが不十分で読みづらいと感じるユーザーがいることがわかりました。これに対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を読み、Copilot をどのように導くか確認しましょう。

Codespace に戻り、ターミナルを開いて、フィルター PR とは分けてアクセシビリティ作業を行う新しい `main` ブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する主な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題と避けるべき失敗を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの命令とツールに沿ったチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作に進む前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順の中で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選ばれていない場合は **Agent** を選びます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選びます。既定では **default** が選択されています。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集はファイル パスと差分とともにチャットに表示されます。
6. 編集が適用されると **Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、この作業の進捗を確認し、必要に応じて指示を追加します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

このレッスンでは、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタム エージェントを使いました。

次は、同じセッションにライト モードの切り替えを追加しながら、[エージェントの作業を監視して案内します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
