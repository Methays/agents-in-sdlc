---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイル変更やテスト実行が必要になることがあります。Copilot エージェントモードは統合開発環境 (IDE) 内でより自律的に動作し、プロジェクトを調べて関連ファイルを特定し、必要な変更を行い、テストを実行し、見つかった問題を修正します。Tailspin Toys に機能を追加する方法を確認しましょう。

> [!NOTE]
> エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、すばやくフィードバックを得て対話できます。cloud agent はチームメンバーのように非同期で動作し、Issue やプルリクエストを通じてやりとりします。

この演習では、次の内容を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行います。
- ユーザーインターフェイス (UI) とデータレイヤーにまたがる機能を実装します。
- コードベースにマージする前に、生成された変更とテストをレビューします。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を確認します。Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディングサイトです。Astro アプリ1つで構成され、ビルド時に静的な HyperText Markup Language (HTML) を生成します。ページは frontmatter 内で Drizzle オブジェクト関係マッピング (ORM) を使ってローカルの SQLite データベースを直接参照します。別のバックエンドのアプリケーション プログラミング インターフェイス (API) やクライアント側ユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準の Document Object Model (DOM) API を使う、小さな範囲に限定された Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動します。

1. Codespace に戻ります。前の演習を終えて `main` に戻っていることを確認します。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して Codespace で新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドでサイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような Uniform Resource Locator (URL) が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーの `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してサイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストする codespace へのプライベートトンネルです。

サイトでは、ホームページにボードゲームの一覧、各ゲームの詳細ページにタイトル、説明、発行元、カテゴリなどの情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されています。Copilot に項目を表示させます。

1. Chat ビューのエージェント選択から **Agent** を選びます。Agent は複数ファイルにまたがる変更を計画して実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時には Claude Sonnet 4.5 を使ったため、手順に含めています。大規模言語モデル (LLM) は確率的に動作するため、提案内容が例と異なることがあります。これは通常の動作です。

> [!NOTE]
> 確率的な動作により、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信し、GitHub リポジトリの Issue バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーにとって有用な順に項目を優先付けします。

## 指示ファイルを確認する

コードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。この演習では Web サイトの機能追加方法を説明する [ユーザーインターフェイス (UI) の指示](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性、アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル別指示へのリンクが含まれます。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示を管理しやすい小さなファイルに分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、データレイヤー (`src/lib/`) のロジック、テスト、ゲーム一覧ページの UI を更新する必要があります。Copilot エージェントモードに作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット。](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択で **Agent** が選ばれていることを確認します。
4. モデルに **Claude Sonnet 4.5** が選ばれていることを確認します。
5. 次のプロンプトで、バックログの関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べて、データレイヤーのヘルパー、UI、テストを見つけ、変更とテスト実行を行う様子を確認します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テスト実行などを行います。タスクやコードベースによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に対する選択肢が表示されるため、作業中に選ぶ必要はありません。

7. Copilot に促されたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログのスクリーンショット。](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。
9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の人工知能 (AI) ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、次の内容を学びました。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行いました。
- UI とデータレイヤーにまたがる機能を実装しました。
- コードベースにマージする前に、生成された変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 - ページネーションを実装する

ゲーム一覧が増えた場合に備えて、ページネーションを追加できます。既存のベストプラクティスと指示ファイルを使い、ページサイズを利用者が選択するか固定値にするかなどの要件を明確にして Copilot に依頼します。必要に応じて追加のコンテキストを与え、変更を反復するのは通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
