---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson]

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装の時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に行い、完了すると人間がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を委任し、開発者はより大きな課題に集中できます。

この演習では、次の内容を学びます。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲が明確な Issue の重要性を理解します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作った開発者は、アプリケーションのドキュメントをほとんど残していません。まず、エクスポートされる関数すべてに TypeScript Documentation (TSDoc) コメントを追加します。

デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示する関連ゲームのセクションを追加します。完成度の高い実装ではなく、ユーザー体験 (UX) の受け入れテストに使える状態を求めています。優先度の高い他の Issue があるため、この作業は後回しになっています。

こうした作業は優先順位が下がりやすく、Copilot cloud agent に適しています。開発者が別の作業をしている間に非同期で処理し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行します。[GitHub Issue を Copilot に割り当てる][assign-issue]などの方法で作業を依頼できます。割り当てられると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備して作業を開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、PR で開発者に通知します。

## 範囲が明確な指示の重要性

GitHub Copilot に魔法はありません。考える作業が不要になるわけではありません。単純に見える操作にも複雑さがあります。[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決め、人工知能 (AI) ペアプログラマーとして扱います。段階的に作業し、学び、試し、状況に応じて調整してください。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。すでに指示を追加している場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent の作業を割り当てる前に、このリポジトリに含まれる指示ファイルを確認します。

github.com のリポジトリ、または起動済みの codespace で次のファイルを読み取り専用で確認します。

- `.github/copilot-instructions.md` — **Code standards** セクションの TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、作成される PR にこれらの指示に基づく TSDoc コメント、TypeScript 規約、コメントヘッダーがあるか確認します。

## Copilot cloud agent の開発環境を準備する

コードを作成するには、環境の準備やセットアップスクリプトが必要なことがあります。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]で、必要な開発ツールや依存関係を準備できます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub Actions ワークフローと同じですが、次の点を確認します。

- `copilot-setup-steps` という1つのジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` を使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot による作業を待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり、まったくなかったりするプロジェクトもあります。技術的負債の一種であり、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot にドキュメントを作成させる Issue は、cloud agent に適しています。後の演習で変更をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue**、**Blank issue** の順に選択します。
4. **Title** に `Code lacks documentation` と入力します。
5. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. **Create** を選択して Issue を作成します。
7. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

8. **Assign** を選択します。
9. **Pull Requests** タブを選択します。
10. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。まだ表示されない場合は、少し待ってからブラウザーを更新します。
11. 数分後に、Copilot が作成した To Do リストを確認します。

> [!NOTE]
> Copilot が PR に To Do リストを表示するまで数分かかることがあります。Copilot はセットアップワークフローを実行して環境を作り、プロジェクトを分析して、タスクの進め方を決めます。

12. タスクの一覧を読み、Copilot が何を行う予定か確認します。
13. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを確認します。
14. **View session** を選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新するために、ブラウザーを更新する必要がある場合があります。

15. 実行中のセッションをスクロールして、Copilot がコードを調べ、状態を理解し、方針を決め、コードを作成する様子を確認します。

非同期作業には数分かかる場合があります。その間に別の作業を続けるという cloud agent の利点を活用するため、もう1つのタスクを割り当ててからアプリの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

作業を分け、cloud agent に別のタスクを任せます。デザインチームが求める関連ゲームの追加に Issue を割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue**、**Blank issue** の順に選択します。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. **Create** を選択します。
7. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

8. **Assign** を選択します。

Issue の最初のコメントに目の絵文字が表示されると、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼を処理しています。人間のソフトウェアエンジニアのように作業するため、常時監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

この演習では、次の内容を学びました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲が明確な Issue の重要性を理解しました。
- Issue を Copilot に割り当てました。

[GitHub Copilot cloud agent][copilot-agents]は、技術的負債への対処、新機能の作成、フレームワーク間のコード移行などに利用できます。

次はカスタムエージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバー][cloud-agent-mcp]とカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
