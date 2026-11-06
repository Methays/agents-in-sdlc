---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントの管理][previous-lesson]

## 作業をレビューする

このラボでは、コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストモードとライトモードを含むアクセシビリティ機能を Copilot に依頼しました。コードをレビューし、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

人工知能 (AI) を利用しても、ソフトウェア設計や開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ自律的に作業するため、安全性を保つ制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みは作業用ブランチに限定されます。
- Cloud agent は GitHub Actions 内に一時的な専用環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されており](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)、Model Context Protocol (MCP) サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加のプルリクエスト (PR) を GitHub.com の標準 PR インターフェイスで確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選び、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TypeScript Documentation (TSDoc) コメントやその他のドキュメントを確認します。具体的な変更内容は異なります。演習 2 で確認した指示ファイルに基づく TSDoc、TypeScript 規約、コメントヘッダーがあるかを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちの表示があるか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことを確認します。単一 Astro アプリのプロジェクトチェックが成功するまで待ちます。数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方向ではありません。チームメンバーと同じように PR のコメントやコードのインラインコメントで `@copilot` にメンションできます。Copilot はコメントを受け取ると、新しいセッションを開始して対応します。結果は確率的に変わるため、決まった文面を指定することはできません。たとえば、次のような追加依頼ができます。

- 各コードファイルの先頭に役割の説明を記述するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TypeScript Documentation (TSDoc) コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をタグ付けします。
2. **View Session** を選択し、Copilot が新しいセッションで変更する様子を確認します。
3. **Back to pull request** を選択して PR に戻ります。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択し、変更をレビューします。満足するまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` にマージします。

![ドラフト PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリで **Pull Requests** タブを選択します。
2. **Show related games on the game details page** などの PR を開き、**Files changed** タブで生成コードをレビューします。
3. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
4. ワークフローの承認待ちが表示される場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

5. PR のチェック欄でワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功したことを確認します。
6. **省略可能:** 関連ゲーム機能を手動テストするには、Codespace でブランチをチェックアウトします。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要であれば Copilot にコメントで変更を依頼します。例として、表示する関連ゲームの数を調整する、コメントヘッダーや TSDoc を追加する、といった依頼があります。カスタム指示を更新する前に割り当てた Issue では、cloud agent が古い指示を使っていた点に注意してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 で追加したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリで **Pull Requests** タブを選択します。
2. **Add high contrast mode to website** などの PR を開きます。
3. Copilot がまだ作業中でタイトルに **[WIP]** が付いている場合は、完了まで待ちます。
4. **Files changed** タブを選択して生成コードを確認します。
5. 実装をレビューします。モード切り替えのユーザーインターフェイス (UI)、設定を保存するローカルストレージ、高コントラストとライトモード用の Cascading Style Sheets (CSS)、Accessible Rich Internet Applications (ARIA) 属性やキーボードナビゲーション、モード切り替えを管理する JavaScript/TypeScript に注意します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちが表示される場合は **Approve and run workflows** を選択します。
8. PR のチェック欄でワークフローが実行され、プロジェクトチェックが成功したことを確認します。
9. **省略可能:** Codespace で Copilot のブランチをチェックアウトして手動テストします。`<branch-name>` を Copilot が作成したブランチ名に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーで高コントラストモードとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントは、アクセシビリティのベストプラクティスに沿うよう Copilot をガイドしました。問題や改善点が見つかった場合は、PR コメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 - さらに作業を委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進める練習をします。たとえば、次のような Issue を作成して割り当てられます。

- ゲーム詳細ページに支援者向けの関心フォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、指示ファイルの影響を確認し、cloud agent に Issue を割り当てて、環境の準備、計画、非同期の実行を観察しました。アクセシビリティ向けのカスタムエージェントを作成して高コントラストとライトモードのトグルを追加し、エージェントページでセッションを監視して方向付けました。最後に、Copilot の PR をレビューして `@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。他の環境で同様の開発を体験するには、次を確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) で Copilot Chat のエージェントモードと MCP 連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — コマンドラインインターフェイス (CLI) で、Plan と Autopilot モード、エージェントスキル、品質保証 (QA) エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、レビュー済みの変更を2つリリースします。

自分のリポジトリでは、cloud agent にリファクタリングやテストカバレッジの Issue を割り当てたり、別分野のカスタムエージェントを作成したり、異なる技術スタック向けに `copilot-setup-steps.yml` を設定したりできます。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
