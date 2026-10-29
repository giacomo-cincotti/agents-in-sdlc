---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作に慣れて、登録済みのフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[GitHub Copilot CLI][about-copilot-cli] はターミナルで動作するエージェント型コーディングアシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップはツールのインストールです。使い慣れたツールを使って簡単に行えます。

このレッスンでは、次の内容を学習します。

- npm で GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーでフィルター Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI はその機能を、多くの開発者が日常的に使うターミナルに提供します。このレッスンでは、インストールと認証を済ませ、ワークショップで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動すれば、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェルプロンプトではコマンドが Codespace で実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> で確定します。
2. `/help` を入力して、インストールしたバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. Copilot に簡単な質問をして動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot がリポジトリを調べてから答えていることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルター Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の簡単な変更を行った後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使用し、自動的には worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの許可範囲外でツールを使う前に確認します。このワークショップでは、Codespace 内の承認プロンプトを省いて演習に集中できるよう、`--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳細は [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` は、後続レッスンで Issue や pull request を扱うための読み書き可能な GitHub MCP ツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際に操作されます。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話できることを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、npm を使って GitHub Copilot CLI をインストールし、GitHub アカウントで認証しました。また、ワークショップのリポジトリを信頼し、簡単な会話を試し、組み込みの GitHub MCP サーバーからフィルター Issue を見つけました。

次は、ゲームカードに星評価を表示する[最初の小さな変更][next-lesson]を行います。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
