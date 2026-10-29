---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent mode でフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にフィルター機能をブラウザーでテストさせます。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部にあるツールを利用するとさらに便利になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準的な仕組みです。

![コンテキスト、LLM、MCP サーバーと VS Code 拡張機能が提供するツールを含む、agent mode の仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスとリアルタイムに通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

MCP サーバーは AI エージェントと外部ツールやサービスの橋渡しをし、利用できるツールとリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP を見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用前にソース コードを確認し、発行者を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密情報や操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれています。前の演習で Copilot がバックログの Issue を読み取れたのはこのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能をテストするために追加します。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行する指定です。デスクトップ環境のない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンド テストに Playwright を使うため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを必要に応じて起動します。Copilot が初めてサーバーのツールを使うときに起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して設定を確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、設定を確認して信頼を選択します。

手動で起動する必要はありません。スキップした場合は、次の手順で Copilot が必要としたときにサーバーが起動し、信頼の確認が表示されます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot はブラウザーでページを開き、移動し、フィルターを適用して結果を読み取れます。自分でアプリを操作せず、会話内で動作を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ラベル付きのボタン、リンク、リスト項目などの構造化された要素を確認します。そのため、簡単な機能確認がアクセシビリティの基本的な確認にもなります。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を確認して結果を報告します。Issue の受け入れ条件と結果を照らし合わせます。問題があれば、PR を作成する前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が初めて Playwright MCP サーバーを使うときにブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Source Control パネルに変更が表示された画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub CLI など、複数の方法で PR を作成できます。GitHub Copilot の GitHub ツールを使い、関連 Issue を探して関連付けた PR を作成してもらいます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときは認証を求められる場合があります。その場合は表示される手順に従います。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選んで PR をレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能をテストしました。
- ブランチを公開し、機能のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
