---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に把握してほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。主な方法の 1 つが[指示ファイル][instruction-files]です。指示ファイルでは、コード生成に関するガイダンスを伝えます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- *現在の*指示を使ってフィルター用の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 次の演習で Copilot が更新した指示を利用できるよう、指示の変更とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、生成結果を文字単位で一致させるのではなく、指示の更新後に動作がどのように*変化するか*を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの機能を説明するコメント ブロックを追加します。

指示ファイルを使って、これらの開発方針に沿って Copilot がタスクを実行できるようにします。

### カスタム指示

カスタム指示ではコンテキストと希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくします。希望するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定でき、提案を調整するのに役立ちます。リポジトリ全体、または特定の種類のファイル向けに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャット リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成できます。TypeScript などの言語や、UI コンポーネントやユニット テストの作成などについて指示を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルの対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトの例は代表的な方法を示しています。基本的には、次の点に注意してください。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データ層など、特定のファイルやタスクに関する指示を記載します。
- 自然な言葉で、明確な指示を書きます。コードの良い例と悪い例を示します。

指示ファイルの作成方法は 1 つではありません。実際に試し、プロジェクトやシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot を使って指示ファイルを作成することもできます。方法は利用する環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、関連する場合に説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象ファイルの glob (リポジトリのルートからの相対パス) を指定します。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや既存ファイルへのリンクがあります。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参照できる例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

カスタム指示の効果を確認するために、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新して、同じプロンプトを再送信し、違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェント ピッカーで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェント ピッカーを表示した画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを変更します。ヘルパー ファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案されたヘルパーは、`db` クライアントを最初の引数に取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードには、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。この演習では、指示を更新した後の*一貫性*の向上を確認します。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、コーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントの規則はありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` にプロジェクト全体の指示を記載する方法と、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` を使う方法があります。今回追加したドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が適切なファイルに集中できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用する実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも他のリポジトリ資産と同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、Copilot がヘルパーの基盤として変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログの指示ファイル更新に関する Issue と関連付けてプルリクエストを作成します。接続済みの GitHub ツールを使って、Copilot に Issue を探し、Issue をクローズする PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) について説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示の更新と、次のフィルター開発で使う publishers ヘルパーの基盤を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージと、ローカル ブランチを `main` に戻すことを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドを Copilot に許可するよう求められたら、内容を確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認しました。VS Code の Copilot Chat を使って、次の操作を行いました。

- *既存の*指示を使ってコード生成のプロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、生成されたコードが新しい標準に従う様子を確認しました。
- 指示の変更とヘルパーの基盤をコミットして `main` に反映し、次の演習で利用できるようにしました。

次は、Copilot がコードベース全体に機能を追加するときに、[エージェント モード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
