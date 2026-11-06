---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフロー内の特定のタスクや分野に合わせた人工知能 (AI) アシスタントです。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベストプラクティス、コードパターン、分野固有の知識を与えます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントをチームで再利用できます。

カスタムエージェントはプロジェクト内の `.github/agents`、またはユーザー全体に適用される `~/.copilot/agents` の Markdown ファイルで定義します。ファイルには少なくとも `name` と `description` を含む YAML frontmatter を指定し、その後に動作、専門知識、指示を記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]は、どちらも主に Markdown ファイルで定義し、AI に作業方法を伝える点で似ています。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整して作業します。この演習では、アクセシビリティ用カスタムエージェントがサイトをガイドラインに照らしてレビューし、プルリクエストのワークフローやテスト実行などのスキルを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、反復して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを使えるようにしたいと考えています。現在のダークテーマでは、テキストと背景色のコントラストが不足し、読みにくいというフィードバックが寄せられています。デザインチームは、利用者が切り替えられる高コントラストモードを求めています。アクセシビリティは重要なため、カスタムエージェントを使って機能を実装します。

この演習では、既存のアクセシビリティ用カスタムエージェントを確認し、Copilot Chat で選択して高コントラストモードを実装します。

この演習では、次の内容を学びます。

- 既存のアクセシビリティ用カスタムエージェントをレビューします。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。内容を読み、Copilot をどのようにガイドするかを確認します。まず `main` からアクセシビリティ作業用のブランチを作成します。フィルター機能のプルリクエスト (PR) とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成における中核的な責任、アクセシビリティのベストプラクティス、HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` で定義されたカスタムエージェントが表示されます。エージェントを選ぶと、その指示とツールに対象を絞った会話を始められます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

1. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを開きます。既定では有効なエージェントとして **default** が表示されています。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトファイルに適用する様子を確認します。編集はファイルパスと差分とともにチャットに表示されます。
6. エージェントが変更した作業セットに合わせて、**Files changed** の表示が更新されることを確認します。

この処理には数分かかる場合があります。Copilot は作業中、Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。次の演習では、進行中の作業をレビューして方向付けます。

## まとめと次のステップ

この演習では、次の内容を学びました。

- 既存のアクセシビリティ用カスタムエージェントをレビューしました。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装しました。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認し、同じセッションにライトモードのトグルを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

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
