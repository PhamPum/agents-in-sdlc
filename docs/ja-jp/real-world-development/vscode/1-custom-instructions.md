---
title: "演習 1 - カスタム指示（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン：前提条件][previous-lesson] · [次のレッスン：Agent mode →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。コード生成の指針となる[指示ファイル][instruction-files]は、コンテキストを伝える方法の一つです。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 既存の指示を使って、フィルター機能の最初のデータ部分（publishers ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の変更とフィルター機能の土台を `main` にコミットし、次の演習で Copilot が使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れることがあります。Copilot の出力は確定的ではありません。この演習では、指示の変更後に傾向が変わることを確認します。同じ文字列のコードが生成されるとは限りません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、モダンな見た目にする必要があります。
- コードのドキュメントには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って Copilot に適切な情報を伝え、こうした開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝えられます。規約、ライブラリ、コードに含めるコメントの形式などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定のファイルを対象にする指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発内容の概要、ベスト プラクティスなど、チャットや CLI の多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このプロジェクトのサンプルは、実践的なアプローチを示しています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルの情報に絞って記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作り方は一つではありません。AI の使い方と同様、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応するファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境での方法は異なります。このワークショップのレッスンでは、該当する箇所で方法を説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を決めるリポジトリ ルートからの glob パターンです。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. ユニット テストの作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより大きな指示セットを小さな再利用可能なファイルに分け、Copilot に参考例を示せます（ここに記載されたパスはリポジトリ ルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではコーディング標準を説明していますが、コード内ドキュメントについてはまだ指定していません。次の手順で、TSDoc のドキュメント コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ここではベスト プラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新した後、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディター タブを閉じます。
2. ヘルパーを配置する場所を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、必要なコードを変更します。ヘルパーとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、型付きの publisher 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。この演習では、指示を更新した後に動作の一貫性が高まることを確認します。

## リポジトリの新しい標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内ドキュメントのルールはありません。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、Copilot にファイル ヘッダーと TSDoc のドキュメント コメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）で作成して全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` として作成することもできます。ドキュメント コメントの規則など、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルを認識できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、次のようなコメント ブロックがファイルの先頭に追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されたことも確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う実際のコードに適用しました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同じように、ソース管理で管理します。変更をリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示ファイルとヘルパーの基礎をステージしてコミットします。

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

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けたプルリクエストを作成します。手動で行う方法もありますが、GitHub ツールが接続されているので Copilot に依頼します。issue を探し、PR を作成してマージするようにプロンプトを送ります。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに設定されているため、準備は不要です。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。画面の案内に従ってください。この接続方法（Model Context Protocol）については、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関係する issue を探し、現在のブランチから PR を作成して、指示の更新と publishers ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作を許可するよう求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作やシェル コマンドの実行を許可するよう求めたら、内容を確認して **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

この演習では、Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認する
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認する
- 指示の変更とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにする

次は [agent mode][next-lesson] で指示を活用し、Copilot にコードベース全体で新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
