---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルタリングの実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。すべてをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを効率よく進めるため、フィルタリングと品質確認の一連の作業を 1 つのセッションとブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能の PR 全体と検証の証拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR のマージを確認します。

## シナリオ

フィルタリングのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、アプリのセッションがプルリクエストを確認し、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど、マージを妨げている問題に取り組みます。GitHub が許可するとマージし、バックグラウンドで動作します。アプリを再起動しても作業は続き、PR がマージされると自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその処理を任せることもできますが、コードの編集やマージには、引き続き明示的な許可が必要です。マージを許可する前に、許可する操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが終わったら、Agent Merge に PR の処理を任せます。

1. エージェント ピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。ドロップダウンの項目を選択しただけでは、処理は開始されません。

Agent Merge は次の作業を行います。

- タイトルと説明を付けてプルリクエストを作成します。
- Issue からセッションを開始した場合は、説明本文で関連 Issue を参照します。
- ターゲット ブランチに対するリベースやマージ競合に対処します。
- CI を監視し、すべてのチェックが成功することを確認します。
- ほかの開発者や Copilot code review からのフィードバックを確認し、コメントを解決するために変更を行います。
- すべてが成功した後、PR を自動的にマージすることもできます。

すべてのチェックが成功した後に Agent Merge が PR をマージできるように設定します。

5. マージを許可する前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のプロンプトでエージェントに明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進捗を確認し、報告された問題を解決します。続行する前に、PR の状態が **Merged** になっていることを確認します。マージの許可を有効にしたことや、エージェントが作業中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限の不足を回避しません。続行する前に、これらの問題を解決してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能の PR 全体と検証の証拠を確認しました。
- レビュー後に Agent Merge を許可し、PR のマージを確認しました。

次は、エージェントと一緒に作業を確認、計画、視覚化する方法を詳しく知るため、[既存の Canvas を使い、トリアージ Canvas を作成します][next-lesson]。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
