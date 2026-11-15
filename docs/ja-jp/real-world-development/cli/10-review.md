---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を行いました。具体的には、次のとおりです。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されていたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始めて、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選び、要件、カバレッジ、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルタリングの変更全体をレビューし、その PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使う slash command を確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` からそれぞれ独立したブランチを作り、2 つの PR を作成しました。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がない場合の状態を明示して表示します。
2. **フィルタリングと品質確認のワークフロー:** フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、これらの作業を 1 つの PR にまとめました。

## 異なる検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式でレポートしました。QA はこれらの結果を、要件とテスト カバレッジのレビューにまとめました。

追加するテストは、実際に存在する不足を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを承認する前にコードと根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。チームのニーズに合わせてこれらのカスタマイズを調整し、会話をまたいで再利用できます。命令は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲の狭い変更で作業に参加し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

環境と同じように、コンテキストも重要です。何を作るのか、なぜ必要なのか、どう進めるのかを明確に伝えると、出力が大きく変わります。

## さらに確認する

主要なワークフローを終えました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。チームと学びを共有し、チームからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

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
