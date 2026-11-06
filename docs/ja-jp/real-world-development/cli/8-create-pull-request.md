---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは簡略化のため、フィルター機能と品質に関する作業を1つの会話とブランチにまとめています。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージのライフサイクルを自動化する方法を理解します。
- 機能全体の変更と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認します。

## シナリオ

フィルター機能の開発では、Copilot を使って計画、実装、検証を行いました。次は、マージの承認権限を開発者の管理下に置いたまま、残りの PR 作業を自動化します。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために残っている作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースして PR を解決します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは **Merge pull request** を自分で選択してきました。Agent Merge にその作業を任せる場合も、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コード全体をレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot が `PR #14 was squash-merged successfully.` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

このレッスンでは、次の内容を学びました。

- Agent Merge と、マージのライフサイクルを自動化する方法を理解しました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認しました。

次は[コンテキスト、モデル、共有、cloud agent への任意の委任に使える Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
