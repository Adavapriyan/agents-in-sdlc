---
title: "レッスン 8 - 機能 PR のレビューとマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップでは手順を簡略化するため、フィルター機能と品質ワークフロー全体を 1 つのセッションとブランチで進め、1 つの PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能 PR 全体と検証の根拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするまでに必要な残りの作業を自動化します。有効にすると、アプリのセッションがプルリクエストを読み込み、CI チェックの失敗の修正、レビュー コメントへの対応、必要な場合のリベースなど、マージをブロックしている問題に取り組みます。GitHub が許可するとマージし、バックグラウンドで実行され、アプリの再起動後も続きます。プルリクエストがマージされると自動的に停止します。

これまでは自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることができますが、コードの編集やマージには、引き続き明示的な許可が必要です。マージ権限を与える前に、許可される操作と作業内容をレビューします。

## Agent Merge で PR を管理する

すべてのコードを作成してレビューできたので、Agent Merge に PR プロセスを任せます。

1. エージェント ピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンの表示が **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンで項目を選ぶだけでは開始されません。

Agent Merge が開始され、次の処理を行います。

- タイトルと説明を含むプルリクエストを作成します。
- Issue からセッションを開始した場合は、説明の本文で関連する Issue を参照します。
- ターゲット ブランチにリベースするか、マージ競合に対処します。
- CI を監視し、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントを解決するための変更を行います。
- すべて成功した後で、必要に応じて PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR をマージさせます。

5. マージを許可する前に、PR の差分と検証の根拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進行状況を確認し、報告された問題を解決します。Agent Merge が有効になっていることや、エージェントが動作中であることだけでは、マージ完了の確認になりません。続行前に PR の状態が **Merged** であることを確認してください。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行する前に問題を解決してください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能 PR 全体と検証の根拠を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は[既存のキャンバスを使い、トリアージ キャンバスを作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
