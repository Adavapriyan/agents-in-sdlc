---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を確認し、Autopilot で Issue に基づくフィルターを作成して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、もっとしっかりしたプロセスが必要です。GitHub Copilot CLI は組織で既に使っているフローに沿って動作し、適切な方法で適切なものを構築できるようにします。このレッスンから、典型的なエージェント駆動の開発プロセスをいくつかのレッスンにわたって進めます。まず Issue から新機能を作成し、コードが有効で機能が期待どおりに動作することを確かめて、最後にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能の開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードでその点を決めてから、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を説明する Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れに近い手順になります。

標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次の操作の前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集は禁止されます。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初は Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

フィルターの作業を始める前に、Codespace に戻ってリポジトリとターミナルの準備をします。

1. Codespace に戻ります。停止している場合は、続ける前に再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画することで、Copilot に実装を任せる前に機能やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を決めて計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に基づいて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装に進みます。Autopilot を使って Copilot に作業を進めてもらいます。Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が合意済みの計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. Issue と、計画中に決めた内容に結果が合っているか確認します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリを起動するよう依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ フィルター、パブリッシャー フィルター、その組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム指示][next-lesson]を使って、コードが文書化された方針に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
