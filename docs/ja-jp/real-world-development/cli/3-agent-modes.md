---
title: "レッスン 3 - エージェント モード：Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue からフィルター機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、適切な方法で適切な機能を作るために利用できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを進めます。まず issue から新機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能の作業は、同じ会話とブランチを使って続けます。通常はファイル種別ごとに異なるブランチや PR を使うでしょうが、ここでは主な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を始める
- Plan モードで要件を決める
- Autopilot モードで新しい機能を実装する
- コードをレビューする
- 転送されたブラウザーで機能を手動で検証する

機能の開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、利用者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意する詳細があります。Plan モードで設計上の判断を確定し、その後、範囲を明確にした作業を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト（PR）を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合はこの流れを基本にしています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**：エージェントが変更を提案し、次の操作に進む前にユーザーの入力を待ちます。
- **Plan**：エージェントは計画を作成しますが、プロジェクト ファイルは編集できません。
- **Autopilot**：エージェントが自律的にコードを書き、テストを実行し、作業が完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から作業を始める

フィルター機能に取り組む前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

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
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトに `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト（PR）をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を依頼する前に機能や作業の方法を決められます。複雑なタスクは、少し時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作ってもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプト下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を決めて計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に合わせて回答します。正解は一つではありません。
4. 計画ができたら <kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して、計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認して問題がなければ、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が問題の解決を完了したと判断するまで、処理を繰り返します。

1. **Accept plan and build on autopilot (recommended)** または、インストールしたバージョンに表示される同様の選択肢を選びます。
2. プロンプト下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 計画を承認すると、実装がすぐに始まることがあります。必ず計画を確認してから承認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

ほかのコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを実行して動作を確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. 結果を issue と計画時に決めた内容に照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実行できるとは限りません。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトを使って、Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリ フィルター、パブリッシャー フィルター、それらを組み合わせた状態を確認します。
4. リセットや該当結果がない場合の動作が、issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を始めた
- Plan モードで要件を決めた
- Autopilot モードで機能を実装した
- コードをレビューした
- 転送されたブラウザーで機能を手動で検証した

次はカスタム指示を使って、コードが文書化された規約に従うようにする方法を[確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
