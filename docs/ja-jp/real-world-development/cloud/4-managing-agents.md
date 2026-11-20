---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能の追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、一緒に追加するのが合理的です。エージェントにライト モードも実装するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した操作を確認します。次のような操作が含まれます。
    - リポジトリのチェックアウト
    - Copilot が作業する環境の作成
    - MCP サーバーのセットアップ
    - 割り当てられたタスクを完了するための各種操作

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認して、作業を監視できます。

## Cloud agent に指示を出す

実行中のタスクを確認できたので、cloud agent がハイ コントラスト モードを作成する間に、ライト モードの切り替え機能も追加するよう依頼します。

1. ハイ コントラスト モードのタスクに対応するセッションを選択します。Copilot が付けたタイトルや作業状況によって、正確なタイトルは異なります。

    ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェント ページで、Steer active session while Copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めたことを確認します。

## Copilot の作業を待つ

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を終えると、新しい依頼を作業の流れに取り込みます。

今回も完了まで数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、指示を出すための中心的な場所である Copilot エージェント ページを確認しました。mission control では、実行中や完了済みのタスクを追跡し、作業をレビューし、進行中のタスクに指示を出して範囲を調整できます。

次の内容を学びました。

- mission control とエージェント ページで cloud agent のタスクを監視する
- 進行中のセッションに指示を出し、機能を追加で依頼する

Copilot がアクセシビリティ機能を実装する間に、Copilot が作成したプルリクエストを[反復して改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
