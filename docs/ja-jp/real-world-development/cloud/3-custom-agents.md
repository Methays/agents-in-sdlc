---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフロー内の特定のタスクや分野に合わせた人工知能 (AI) アシスタントです。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベストプラクティス、コードパターン、分野固有の知識を与えます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントをチームで再利用できます。

プロジェクト内の `.github/agents` またはユーザー全体に適用される `~/.copilot/agents` の Markdown ファイルで定義します。ファイルには少なくとも `name` と `description` を含む YAML frontmatter を指定し、その後に動作、専門知識、指示を記述します。

### カスタムエージェントとエージェントスキルの違い

[エージェントスキル][agent-skills-concept]とカスタムエージェントはどちらも AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整します。このラボのアクセシビリティエージェントは、サイトをアクセシビリティガイドラインに照らしてレビューおよび更新し、プルリクエストのワークフローやテスト実行などのスキルを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、反復して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次の内容を確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを使えるようにしたいと考えています。現在のダークテーマでは、テキストと背景色のコントラストが不足し、読みにくいというフィードバックが寄せられています。デザインチームは、利用者が切り替えられる高コントラストモードを求めています。アクセシビリティ用カスタムエージェントを使って機能を実装します。

この演習では、既存のアクセシビリティ用カスタムエージェントをレビューし、そのエージェントに GitHub Issue を割り当てます。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。内容を読み、Copilot をどのようにガイドするかを確認します。

1. codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. アクセシブルな Web サイトのコード生成における中核的な責任、アクセシビリティのベストプラクティス、HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

タスクを Copilot に割り当てるため、リポジトリに高コントラストモードの Issue を作成します。

1. リポジトリを開きます。
2. **Issues** タブを選択し、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Add high contrast mode to website` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. **Create** を選択して Issue を作成します。
6. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
7. 一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントとアクセシビリティが強調表示された cloud agent 割り当て画面のスクリーンショット。](../../../_images/ex5-select-custom-agent.png)

8. **Assign** を選択します。

Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法を確認しました。
- カスタムエージェントにタスクを割り当てました。

次は[エージェントページからセッションを監視して方向付け][next-lesson]ます。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
