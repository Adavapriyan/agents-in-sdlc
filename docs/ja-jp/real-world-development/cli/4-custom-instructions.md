---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、その指示を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、どのようなコードを作るかだけでなく、コードをどのように構成するかも記載できます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントでは、コードを言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値 (該当する場合は注入可能な `db` 引数を含む) を説明する TSDoc/JSDoc を記載します。
- 再利用可能な Astro コンポーネントには `Props` の仕様を説明し、関連コードが変更されたときにコメントも更新します。
- 既存の書式と lint の指針を維持します。

指示ファイルに必要な情報を記載し、これらの開発方針に沿って Copilot がタスクを実行できるようにします。

## 指示ファイル

カスタム指示ではコンテキストと希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくします。希望するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定でき、提案を調整するのに役立ちます。リポジトリ全体、または特定の種類のファイル向けに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係する技術スタック、開発中の内容、ベスト プラクティスなどのプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関する指針を提供します。

> [!NOTE]
> ほかの形式の指示ファイルとその対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトには、変更前後の影響を比較できるよう、指示ファイルが用意されています。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング指針を確認します。これらの指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象ファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存ファイルへのリンクを確認します。複雑な指示を再利用できる小さなファイルに分け、Copilot が参照できる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルは出発点として適切ですが、追加できる指示があります。TypeScript の新しいコードに TSDoc コメントを追加するよう、基本となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は、会話を開始するときにリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま、新しい指示を適用するために、会話を終了して再開します。

1. フィルターの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## 指示の変更をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内の他のファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、ターミナルを開きます。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、Copilot がヘルパーの基盤として変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けてプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているため、Copilot に Issue を探し、Issue をクローズする PR を作成して、マージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot がツールを初めて使うとき、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習では、この接続の仕組みである Model Context Protocol について説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示の更新と新しい publishers ヘルパーの基盤を説明する PR を現在のブランチから作成し、関連する Issue を探すよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドを Copilot に許可するよう求められたら、内容を確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認しました。VS Code の Copilot Chat を使って、次の操作を行いました。

- コード生成のプロンプトを送り、*既存の*指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい標準に従うことを確認しました。
- 指示の変更とヘルパーの基盤をコミットして `main` に反映し、次の演習で利用できるようにしました。

次は Copilot がコードベース全体に機能を追加するときに、[エージェント モード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
