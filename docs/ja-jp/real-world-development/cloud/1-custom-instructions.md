---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に把握しておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に依頼内容だけでなく、作業方法も伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すことが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの機能を説明するコメント ブロックを追加します。

指示ファイルを使って、これらの開発方針に沿って Copilot がタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストと希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、追加してほしいコメントの種類などを指定できるため、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベスト プラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成できます。TypeScript などの言語や、UI コンポーネントやユニット テストの作成などのタスクに関する指示を記載します。

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
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルが見つかります。
>
> Copilot を使って指示ファイルを作成することもできます。方法は利用する環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、関連する場合に説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認してください。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象ファイルの glob (リポジトリのルートからの相対パス) を指定します。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参照できる既存の例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## リポジトリ標準を追加する

この演習で行う編集は、コード内に TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーを追加するよう、プロジェクト全体に適用する規則を設定することです。後続の演習で Issue を割り当てると、cloud agent がこの規則を使います。

編集の前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、ドキュメント コメントに関するルールを追加しましょう。

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

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が指示を使えるように、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。ブランチを作成する手順は、実際のプロジェクトでの作業方法に合わせています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストをレビューすると、追加した標準が使われていることを確認できます。

次は、Copilot cloud agent に最初の Issue を[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
