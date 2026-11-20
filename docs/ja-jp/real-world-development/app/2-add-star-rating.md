---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェント セッションを開始し、その構成を学びます。
- エージェントにプロジェクトへの小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、パブリッシャー、説明だけが表示されています。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、小さく独立した変更です。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方のセッションでは機能を追加し、もう一方ではバグを修正するなど、変更を競合させずに複数のセッションを実行できます。セッションはサイドバーでリポジトリごとにグループ化され、選択すると切り替えられます。

セッション内では、エージェントとの**会話**、ファイルの調査や編集などの**ツール操作**、差分付きの**変更ファイル一覧**を確認できます。

## セッションを開始して変更を依頼する

新しいセッションでプロジェクトを調べ、機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻ります。閉じている場合はアプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを送信します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、プロジェクトの分離されたコピーである新しい worktree を作成して作業を開始します。プロジェクトを調べて、機能を追加するために更新するファイルを特定し、必要なコードを作成します。Copilot app を使って新しい機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択すると、変更ファイルと差分が表示されます。

    ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら "No rating yet" を表示する、次のような小さなブロックが追加されています。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に決定論的ではなく確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方をテストしていることを確認します。前提条件が不足していたりチェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、更新された UI を確認します。ブラウザー キャンバスで Web サイトを開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを調べて作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトで、アプリを起動してブラウザー キャンバスで開くよう依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするためのプルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. PR を作成する前に一致する Issue が必要であると Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューパネルで PR を開きます。ここで必要に応じて PR をレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログ ウィンドウで **Merge pull request** を選択してマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースできました。次の操作を行いました。

- エージェント セッションを開始し、セッションの構成を学びました。
- エージェントにゲーム カードの小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は、フィルターの Issue から始めて Plan モードと Autopilot モードを使い、より大きな機能を作成します。[レッスン 3][next-lesson]に進みます。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
