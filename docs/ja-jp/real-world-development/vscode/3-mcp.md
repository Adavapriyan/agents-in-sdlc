---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストし、その後ブランチを公開して PR を開きます。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot のツール拡張について学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするために Copilot に利用を依頼します。
- ブランチを公開し、フィルター作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にアクセスできると、より多くの作業を行えます。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部構造と、コンテキスト、LLM、MCP サーバーおよび VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやり取りできます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービス (既存の API や NPM パッケージなどのローカル ツール) の通信を管理します。各 MCP サーバーは、エージェントがアクセスできる異なるツールとリソースを提供します。

よく利用されている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。AI エージェントは、リポジトリの作成や更新、Issue とプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームの入力、ボタンの選択などを行えます。

さまざまなツールやリソースを提供する MCP サーバーが他にもあります。GitHub は、エコシステム内での発見やコントリビューションを促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用前にソース コードを慎重にレビューし、発行元を確認して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot はバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っており、VS Code が HTTP で接続して Copilot がツールを初めて使うときに GitHub で認証するため、ローカルへのインストールは不要です。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能のテストに最適です。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Codespace にはブラウザーを表示するデスクトップ環境がないため、Playwright はウィンドウを表示せずに実行されます。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストで Playwright を使うため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は**必要に応じて** MCP サーバーを起動します。Copilot が初めてそのサーバーのツールを必要としたときに、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code でサーバーを信頼するか確認されたら、構成をレビューして、サーバーを起動できるように信頼します。

手動での起動は省略できます。スキップした場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code が起動し、信頼するか尋ねます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って要約します。会話を中断せずに機能の動作を確認する方法です。

内部では Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などのラベル付き要素を扱います。簡単な機能テストを、軽いアクセシビリティ確認としても利用できます。

サーバーを接続してアプリを起動した状態で、作成したフィルター機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準に照らして報告を確認してください。問題がある場合は、質問を追加するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンド ライン インターフェイス (CLI) などで作成できます。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する Issue を見つけ、関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得して操作を実行するために、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。指示に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot のツール拡張について学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター作業のプルリクエストを作成しました。

MCP サーバーを使って機能を*テスト*しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
