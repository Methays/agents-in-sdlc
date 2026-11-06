---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズしてフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードの手動検証、指示ファイルによる規約の適用、テスト、lint、継続的インテグレーション (CI) などの作業があります。こうしたタスクには **エージェントスキル**が適しています。スキルを使うと、Copilot にテストなどの操作方法を伝えられます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys はユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするため、報告形式を改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要なときに読み込む再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある Markdown ファイル `SKILL.md` で構成されます。Markdown には、スキル名と説明を定義する frontmatter、処理の概要、呼び出すべき状況を示すガイダンスを記載します。必要に応じて、スクリプトやその他のリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するリポジトリの `.github/skills` フォルダーや、ユーザー用の `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行に用意した `quality-checks` スキルを調べます。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するために使います。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すか、自然言語で指示できます。まず3種類のチェックを実行します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示された報告を確認します。

## 報告をカスタマイズする

実行した内容、成功の可否、ツールが報告した情報が分かるように、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

変更したスキルは、会話を再起動せずに Copilot CLI で再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と**同じプロンプト**を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示された報告を確認し、最初の結果と比較します。

## まとめと次のステップ

このレッスンでは、次の内容を学びました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルター機能の PR に含めます。次は[Playwright MCP サーバーで Copilot にサイトを操作させます][next-lesson]。

## その他のスキル例

次のコミュニティ例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
