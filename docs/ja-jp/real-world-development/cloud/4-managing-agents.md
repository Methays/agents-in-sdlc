---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視する方法も重要です。

GitHub の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。ここでは、オープンなプルリクエスト (PR) があるタスクを確認し、実行中のセッションを調べて、追加の指示を出せます。

このレッスンでは、次の内容を学びます。

- エージェントページから cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼して方向付けます。

## シナリオ

高コントラストモードの作成を依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているところなので、作業中のエージェントにライトモードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。コードドキュメントの追加、ゲーム変更用のアプリケーション プログラミング インターフェイス (API) の生成、Web サイトへの高コントラストモード追加などのタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。コードのチェックアウト、作業環境の作成、Model Context Protocol (MCP) サーバーの設定、タスクを完了するための処理などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、高コントラストモードに取り組んでいる Copilot にライトモードも追加するよう依頼します。

1. 高コントラストモードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで実行中のタスクと、強調表示された「Steer active session while Copilot is working」ダイアログを示すスクリーンショット。](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot の作業を続ける

Copilot は、メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼を作業に取り込みます。完了まで数分かかることがあります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、次の内容を学びました。

- エージェントページを確認して cloud agent のタスクを監視しました。
- 実行中のセッションに追加の機能を依頼して方向付けました。

次は[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
