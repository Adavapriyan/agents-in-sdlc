---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義し、特定の種類の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、テスト パターンを統一するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML フロントマターと、エージェントの振る舞い、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには専用のコンテキスト ウィンドウがあり、作業の一部としてスキルや他のエージェントを呼び出すよう調整できます。このラボでは、アクセシビリティ カスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI に関する他のことと同じように、実際の環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、ダーク テーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題を解決するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタム エージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使って、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントがすでに用意されています。内容を確認し、Copilot への指示を把握しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します (演習 3 のフィルター PR は別のブランチに保ちます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成を担う主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と間違い

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選ぶと、そのエージェントの指示とツールに応じたチャット セッションが始まります。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェント ドロップダウンで **Agent** を選びます。

   ![Chat ビューでエージェント ピッカーを表示した画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェント ドロップダウン (アクティブなエージェント名が表示され、既定では **default**) を選択します。
3. 利用可能なエージェント一覧から **Accessibility agent** を選びます。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集ごとに、ファイル パスとレビューできる差分がチャットに表示されます。
6. 編集の進行に合わせて **Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この作業には数分かかります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集し、リポジトリに変更を加えています。

次の演習では、作業中のセッションを確認して指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。特定のタスクや分野に合わせた AI アシスタントです。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が作業を効果的に進められるようにできます。

このレッスンでは、次の内容を学びました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は[エージェントの作業を監視して指示を出し][next-lesson]、変更を確認しながら、同じセッションでライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
