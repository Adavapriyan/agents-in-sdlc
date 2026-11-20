---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、省略可能なクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

他の CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュ コマンドがあります。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、他の便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次はコンテキストの管理、モデルの切り替え、セッションの共有などの機能を確認し、省略可能な場合は [Copilot cloud agent][about-cloud-agent] に作業を委任します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使うと、自分で状態を確認して圧縮できます。

1. Codespace に戻ります。まだ Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き容量にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを指定したい場合は `/compact` を使います。関連のない別タスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルと使用状況を確認し、使うモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使う、別のモデルを選ぶ、または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンにおける `/share` の選択肢を確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルに Markdown ファイルをエクスポートします。
3. エクスポート内容を他の人に送ったり公開したりする前にレビューします。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

共有リンクや gist の公開は省略可能です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスクに使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分け、並行して実行します。
- `/worktree` は、別のタスク用に独立した Git worktree を作成します。
- `/delegate` は、非同期に作業し、プルリクエストを作成することがある Copilot cloud agent にタスクを送信します。

これらのコマンドは追加の worktree やリモート作業を作成できるため、省略可能です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

Copilot CLI には他にもスラッシュ コマンドがあり、学べることがあります。[これまでの内容を振り返り][next-lesson]、学習を続けるための次のステップを確認して、ワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
