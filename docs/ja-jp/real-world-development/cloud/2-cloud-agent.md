---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。解決されていないセキュリティ上の問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発プロセスを加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境のカスタマイズ
- 操作を安全に実行する方法
- Issue の範囲を明確に定めることの重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した委託業者はドキュメントを十分に作成していませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの他のタイトルを「関連するゲーム」として表示し、閲覧を続けられるようにします。洗練された実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い Issue がほかにあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てれば、非同期で作業させ、その間に別の作業を進めてから、戻って成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][start-copilot-sessions]方法などがあります。割り当てると Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップしてタスクに取りかかります。セッションは作業中でも完了後でも確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその働きを学びました。カスタム指示の演習をすでに完了した場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで内容を簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認して、後でその効果を見つけられるようにします。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。`**/*.test.ts` に一致するファイルに指示が適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、指示ファイルの内容が TSDoc コメント、TypeScript の規約、コメント ヘッダーに反映されます。後続の演習で PR をレビューするときに確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを作成する場合でも、必要な開発ツールや依存関係を整える環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent は SWE と同じようにタスクを実行します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。学習をスムーズに進められるよう、このラボでは事前に構成済みです。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行およびシードされた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最低限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローとよく似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストで作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成させ、後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールすると、Copilot が Issue の作業を始めたことを示す更新があります。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウを再読み込みする必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて決定し、コードを作成していきます。

完了まで数分かかります。Copilot cloud agent の主な目的は、非同期でタスクを実行して、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能を追加するコードを作成します。

## ゲーム詳細ページに関連するゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チームが求める関連ゲームの表示に時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように作業するので、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする
- 操作を安全に実行する
- Issue の範囲を明確に定める
- Copilot に Issue を割り当てる

cloud agent がバックグラウンドで作業する間に、カスタム エージェントを作成して使用する方法を確認しましょう。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も利用でき、前のモジュールで扱ったカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
