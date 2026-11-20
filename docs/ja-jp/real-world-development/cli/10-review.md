---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター Issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで確認した根拠を評価しました。
- フィルター機能の変更全体をレビューし、フィルターの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済みの `main` から作成したブランチを使う、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルターを計画してから PR を作成するまで、同じ会話とブランチを使いました。ワークショップの手順を簡略化するため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト カバレッジのレビューをまとめてから、PR に進みました。

追加するテストは、実際の不足を補うものでなければなりません。QA を実行しても新しいテストが不要な場合があります。マージを許可する前にコードと根拠をレビューし、変更後は影響を受ける根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業に影響します。このワークショップでは指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整しましょう。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、対象を絞った変更に参加し続けるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作成したいのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。Copilot CLI には、他にも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールの使い方を向上させる最善の方法は、使い続けることです。本番コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してみましょう。チームで学んだことを共有し、互いの経験から学んでください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を発展させる場合は、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
