---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を Tailspin Toys の一連のワークフローで使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、事前に用意されたフィルター issue を見つけた
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動でマージした
- フィルター issue から始め、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューした
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行した
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した
- 品質保証（QA）カスタム エージェントを作成、選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価した
- フィルター機能の変更全体をレビューし、Agent Merge による PR のマージを許可した
- コンテキスト、モデル、共有、省略可能な cloud agent への委任に使うスラッシュ コマンドを確認した

## 公開した内容

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を作成します。

1. **星評価：** 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **フィルターと品質ワークフロー：** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成し、関連するテストを含めます。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、すべての作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果と、要件やテスト カバレッジのレビューをまとめて PR 前の確認に使いました。

テストは実際に不足している箇所を補うものにしてください。QA の実行時に追加テストが不要と判断されることもあります。コードと根拠をレビューしてからマージを許可し、変更後は関連する根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの概要だけでなく、実際の変更やツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方法を検討する場合は **Plan**、作業に関わりながら範囲を絞った変更を行う場合は **Interactive**、範囲の明確なタスクを任せる場合は **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、結果は大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI の機能をさらに確認する場合は、次のコマンドを試せます。

- `/review` でコード レビュー エージェントに変更を分析してもらう
- `/rubber-duck` で問題を相談し、別の視点を得る
- `/fleet` で独立したサブタスクを並行して調整する
- `/worktree` で別のタスクを分離する
- `/delegate` で Copilot cloud agent にタスクを送信する

## 次のステップ

どのツールも、使い続けることが上達につながります。プロダクション コード、趣味のコード、何年も作ろうと思っていた小さなアプリなどで試してみてください。学んだことをチームと共有し、チームのメンバーからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
