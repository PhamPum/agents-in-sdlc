---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
description: "Copilot app のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、公開します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションで、エージェント主導の開発を単一の作業用ワークスペースで実現します。並列エージェント セッション、切り替え可能なセッション モード、共有 Canvas、GitHub issue とプルリクエストのネイティブな管理機能を備えています。さらに **Agent Merge** を使うと、プルリクエストを rebase やレビュー フィードバック、継続的インテグレーション（CI）の修正、マージへと進められます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと事前に用意されたバックログを確認します。
2. 星評価に対象を絞った変更を加え、ブラウザーでレビューして、最初のプルリクエスト（PR）を手動でマージします。
3. フィルター機能の issue から始め、**Plan** モードで方法を決め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol（MCP）サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証（QA）用のカスタム エージェントを作成して、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター機能の変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. Canvas が利用できるか確認してから、Database Explorer を使い、リポジトリに保存されるトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録して読み取り専用の代替手段を使うか、まとめの演習に進みます。

ワークショップを簡潔に進めるため、PR は 2 つ作成します。星評価の PR と、指示、スキル、QA プロファイル、テストを含むフィルター機能の PR です。どちらも更新済みの `main` から開始します。フィルター機能と品質に関するワークフローでは 1 つのセッション、worktree、ブランチを共有するため、ツールを学ぶときにそれまでの作業を活用できます。最後の Canvas 演習も同じセッションに保持し、PR ワークフローを繰り返すのではなく、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app をインストールする][ex1] | セットアップ | アプリをインストールし、プロジェクトを接続して、ワークスペースを確認します |
| [2. 星評価を追加する：小さく効果的な変更][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、1 つ目の PR をマージします |
| [3. エージェント モード：Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で作成して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認、更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存スキルを確認し、結果のレポートを変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を設定し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存されるトリアージ Canvas を作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを振り返ります |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は、[GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。そのため、[前提条件の演習][ex0]ではアプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。これは Copilot CLI ポリシーとは別の設定です。

## はじめに

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
