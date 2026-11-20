---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能の追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザイン チームが改善を重ねられる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。その後、チームの他のメンバーにもレビューを依頼します。

### シナリオ

これまでに何度も確認したとおり、生成 AI を利用してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で、監視されていない状態でタスクを実行するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業に使うブランチに**のみ**許可されます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成したプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、ドキュメントの追加を示すタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ってください。数分かかることがあります。休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメント コメントや、その他のドキュメントを確認します。具体的な変更内容は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容を確認できたら、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローの承認が必要な場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストで作業するとき、一方向に指示するだけではありません。チームのメンバーに依頼するように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確定的ではないため、必ず同じ形になる依頼文はありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する
- Astro アプリの構造を説明する README を作成する

1. コードのドキュメントに関して Copilot に更新してほしい内容をコメントし、他のユーザーと同じように **@copilot** をメンションします。上記の例から選んでも、別の提案でもかまいません。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、新しいコミットが PR に追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善できます。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲーム機能を示すタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認できたら、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を探します。
7. ワークフローの承認が必要な場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整するように依頼できます。また、カスタム指示を変更する前にこの Issue を割り当てたため、コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう依頼するのもよいでしょう。前の PR と同様に、**Conversation** タブにコメントを追加すると Copilot が確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイ コントラスト モードを示すタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。数分かかることがあります。Copilot が完了するまで待ってください。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認します。特に、次の点に注意してください。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の使い方
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認できたら、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローの承認が必要な場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードを切り替えて、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、Copilot の機能実装をアクセシビリティのベスト プラクティスに沿うように導いたことを確認してください。アクセシビリティ上の懸念や改善点があれば、以前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 作業の委任を続ける

Cloud agent は、バックログの実際の項目を渡し、その間に別の作業を進める場合に効果を発揮します。習慣として身に付けるために、リポジトリでいくつか Issue を作成し、Copilot に割り当ててみましょう。次のような作業を試せます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データ アクセス ヘルパーに入力検証とエラー処理を追加する

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の内容を学びました。

- **このリポジトリにあるカスタム指示ファイルを確認**し、その内容が後の cloud agent の成果にどう反映されるかを把握しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用のカスタム エージェントを作成して使用**し、ハイ コントラストとライト モードの切り替え機能を追加しました。
- **Copilot エージェント ページを mission control として使用**し、アクセシビリティ セッションを監視して指示を出しました。
- **cloud agent のプルリクエストをレビューして反復改善**し、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。引き続き学ぶ場合は、他のハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる
- 別の分野向けにカスタム エージェントを作成する
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent ファイアウォールの構成][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
