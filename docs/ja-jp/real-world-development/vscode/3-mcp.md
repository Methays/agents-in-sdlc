---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで動作を確認します。**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせて、ブラウザーで機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にあるツールを利用すると、さらに幅広い作業ができます。Model Context Protocol (MCP) は、エージェントが外部のツールやサービスと通信するための標準です。

![コンテキスト、大規模言語モデル (LLM)、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) とエージェントモードの連携を示す図。](../../../_images/ex1-mcp-diagram.png)

[MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) エージェントはリソースを使って最新情報にアクセスし、ツールを使って開発者に代わって操作できます。MCP サーバーは AI エージェントと外部サービスの間を仲介します。既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカルツールと通信し、それぞれ異なるツールやリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作し、ページの移動、フォームの入力、ボタンの選択などを行います。

利用可能な MCP サーバーは他にもあります。GitHub は、エコシステム内での発見や貢献を促進する [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーも他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを慎重に許可してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーが構成済みであることを確認します。

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

この `github` 設定はプロジェクトテンプレートに含まれています。前の演習で Copilot が Issue のバックログを読めたのはこのためです。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は Hypertext Transfer Protocol (HTTP) で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]を追加すると、Copilot が操作できるブラウザーを利用できるようになります。

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

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面にブラウザーウィンドウを表示しない設定です。デスクトップ画面のない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストにも Playwright を使うため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使う際、VS Code が起動を促し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するよう求められた場合は、構成を確認して信頼する選択肢を選びます。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに VS Code が起動し、信頼するか確認します。

## フィルター機能をテストする

Playwright MCP サーバーが接続され、アプリが起動している状態で、Copilot にフィルター機能を試させます。エージェントはページを開き、移動し、フィルターを適用して結果を読み上げ、確認内容をまとめます。ページの[アクセシビリティツリー][playwright-mcp-server]を参照するため、機能確認と簡単なアクセシビリティ確認を同時に行えます。

次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ基準と比較します。問題があれば、プルリクエストを作成する前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テスト時にアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP の初回使用時にブラウザーをダウンロードすることがあります。不足が報告された場合は `npx playwright install chromium` を実行するよう依頼して、再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるよう `filtering-vscode` ブランチを公開し、プルリクエスト (PR) を作成します。

1. codespace で **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot が行った変更を示す Source Control パネルのスクリーンショット。](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) などで PR を作成できますが、ここでは GitHub ツールに接続済みの Copilot に作成を依頼します。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールの初回使用時に認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR を確認します。**まだマージしないでください。**

## まとめと次のステップ

この演習では、次の内容を学びました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Copilot にサーバーを使わせて、ブラウザーで機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

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
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
