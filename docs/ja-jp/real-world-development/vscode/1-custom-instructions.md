---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

人工知能 (AI) を使うときはコンテキストが重要です。作業方法の決まりや Copilot に知ってほしい背景情報がある場合は、Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、生成するコードだけでなく、その構成方法も伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- 現在の指示でフィルター機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規約に従う様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で新しい指示を使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約から外れる場合があります。Copilot は確率的に動作します。この演習では、出力の文字列を完全に一致させるのではなく、指示の更新後に動作がどのように変化するかを確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベストプラクティスなど、プロジェクト全体に関係する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語やファイルの種類、タスク (TypeScript、Astro、UI コンポーネント、テストなど) に指示を与えます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。Copilot は AGENTS.md や CLAUDE.md などの形式にも対応しています。

指示ファイルを管理する際は、次の方法を参考にしてください。

- `copilot-instructions.md` には、作成するもの、プロジェクト構成、全体に適用する規約など、プロジェクトレベルの情報を記述します。
- `*.instructions.md` には、テスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定のタスク向けの指示を記述します。
- 自然な言葉で、明確なガイダンスと良い例・避けるべき例を示します。

すべてのプロジェクトに共通する唯一の正解はありません。プロジェクトでの試行を通じて、自分に合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを活用できます。このプロジェクトには [UI の指示][ui-instructions]や [Astro の指示][astro-instructions]などがあります。Copilot に指示ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境ごとに方法が異なります。テンプレートを探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

変更による影響を確認する前に、スタータープロジェクトに含まれる指示ファイルを調べます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示も確認してください。これらの指示は、このリポジトリで Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに、その指示が適用されます。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読み、`unit-tests.instructions.md` など他の指示ファイルやプロジェクト内ファイルへのリンクを確認します。リンク先を使って大きな指示を小さく再利用しやすいファイルに分け、Copilot に既存の例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する規約はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用ブランチを作成する

変更に取り組む前に、新しいブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

最初に現在の指示を使ってプロンプトを送り、結果を確認します。後で指示を更新して同じプロンプトを再送信し、違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。通常、ヘルパーファイルとテストが変更されます。
6. 提案されたヘルパーが、型付けされた関数として `db` クライアントを最初の引数に受け取り、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## リポジトリ全体の規約を追加する

`.github/copilot-instructions.md` にプロジェクトレベルの情報を記述し、コード提案を改善するために規約を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目前後にある **Code formatting requirements** セクションを探します。プロジェクトの規約はありますが、コード内ドキュメントの規則がまだないことを確認します。
3. 既存の規約の直後に次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` と、言語やファイルの種類、タスク向けの `*.instructions.md` のどちらにも作成できます。ドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約に従うよう Copilot をガイドし、次の演習で使う実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリ資産と同じようにソース管理します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. `git status` を実行し、指示とヘルパーの変更が存在することを確認します。
3. 指示の更新と、ヘルパーの基盤に Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチでプルリクエストを作成し、バックログにあるドキュメント規約の Issue と関連付けます。GitHub ツールを使って Copilot に作業を依頼します。

> [!NOTE]
> Copilot が GitHub 上で操作するのは、この演習が初めてです。テンプレートプロジェクトでは GitHub ツールがワークスペースに接続済みのため、セットアップは不要です。初めてツールを使うときに VS Code からサインインを求められた場合は、指示に従って許可します。後の演習で、この接続に使われる Model Context Protocol (MCP) について学びます。

1. codespace で Copilot Chat を開きます。
2. Mac では <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>、Windows/Linux では <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新と発行元ヘルパーの基盤を説明するプルリクエスト (PR) を現在のブランチから作成するよう、次のプロンプトを送信します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェルコマンドを許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

この演習では、次の内容を行いました。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認しました。
- 現在の指示でフィルター機能の最初のデータ部分を生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい規約に従う様子を確認しました。
- 指示の更新とフィルター機能の一部を `main` にマージし、次の演習で新しい指示を使えるようにしました。

次は[エージェントモード][next-lesson]で指示を使い、Copilot にコードベース全体にまたがる機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
