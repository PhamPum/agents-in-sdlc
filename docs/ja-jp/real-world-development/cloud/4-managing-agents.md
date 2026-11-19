---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、Copilot cloud agent に 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクの管理には、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールを使えます。この mission control では、オープンなプルリクエストがあるエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加のガイダンスを送ったりできます。

このレッスンでは、次のことを行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を変更する作業をすでに進めているため、同時に追加するのが合理的です。エージェントの作業を調整し、ハイ コントラストとライト モードの両方を実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイ コントラスト モードの追加
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーの設定
   - 割り当てられたタスクを完了するための各種作業

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認します。PR や変更されたファイルを監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードのセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を始めるまで数分待ちます。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、作業中のセッションを調整するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に組み込むことを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼が作業に反映されます。

これには数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent タスクの監視とガイダンスに使う中心的な場所である Copilot agents ページを確認しました。mission control で、進行中と完了済みのタスクを追跡し、作業を確認し、実行中のタスクに追加の指示を送れます。

次の内容を学びました。

- mission control と agents ページで cloud agent のタスクを監視する
- 実行中のセッションに機能追加を依頼する

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成した[プルリクエストを反復して改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン：カスタム エージェント][previous-lesson] | [次のレッスン：Copilot の作業を反復して改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
