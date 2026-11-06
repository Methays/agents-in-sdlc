---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

人工知能 (AI) を使うときはコンテキストが重要です。作業方法の決まりや Copilot に知ってほしい背景情報がある場合は、Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは指示を追加する前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを追加します。
- ユーザーインターフェイス (UI) はダークモードにして、現代的な印象にします。
- コードのドキュメントには TypeScript Documentation (TSDoc) コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストです。推奨する規約やライブラリ、コードコメントの方針などを指定できます。リポジトリ全体や、特定のファイルの種類に指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られます。多くのリクエストに関係するプロジェクト情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの特定の言語やファイルの種類、UI コンポーネントやユニットテストなどのタスクに指示を与えます。

統合開発環境 (IDE) で指示ファイルが使われるのは Copilot Chat のコード生成時であり、コード補完や次の編集候補ではありません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示と `applyTo` frontmatter を持つ `*.instructions.md` をコード生成時に使います。Copilot は [AGENTS.md や CLAUDE.md など、他の標準の指示ファイルにも対応しています][custom-instructions-support]。

指示ファイルを管理する際は、次の方法を参考にしてください。

- `copilot-instructions.md` には、プロジェクトの概要、構成、全体に適用する規約など、プロジェクトレベルの情報を記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定のタスク向けの指示を記述します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成に唯一の正解はありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> このプロジェクトには [UI の指示][ui-instructions]や [Astro の指示][astro-instructions]など、さまざまなタスク向けの指示があります。Copilot に指示ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境ごとに方法が異なります。指示ファイル、カスタムエージェント、その他のリソースを探すには [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらの指示は Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これは指示が適用されるファイルを決める、リポジトリルート相対の glob です。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読み、`unit-tests.instructions.md` などの指示ファイルやプロジェクト内ファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内ドキュメントの規則はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体の規約を追加する

Copilot cloud agent が後の演習で割り当てられた Issue に取り組むときに、この規約を参照できるようにします。

1. codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. 27行目前後にある **Code formatting requirements** セクションを探します。既存の規約を確認し、コード内ドキュメントの規則がまだないことを確認します。
4. 既存の規約の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、特定の言語、ファイルの種類、タスクに適用する指示は `*.instructions.md` に記述します。ドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は Issue が対象とするブランチの指示ファイルを読みます。次の演習で `main` を対象に Issue を割り当てるため、cloud agent が指示を使えるように変更を `main` にマージします。

1. 指示の変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けてプルリクエストを作成し、マージします。次の演習で cloud agent が指示を読み込めるようになります。

> [!TIP]
> このワークショップではブランチを使って変更を分けますが、直接 `main` で作業しても構いません。実際のプロジェクトに近い手順を体験できるよう、ここではブランチを作成しています。

## まとめと次のステップ

この演習では、次の内容を学びました。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。

残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この規約が反映されているか確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
