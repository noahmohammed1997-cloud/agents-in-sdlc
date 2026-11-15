---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は、**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さく保ちます。ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、その構成を学びます。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリ、発行元、説明だけが表示されています。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。小さく独立したこの変更は、最初のセッションに適しています。

## セッションの構成

**セッション**は、独立したワークスペースで動作するエージェントとの会話です。各セッションには**専用の Git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加しながら、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにサイドバーにまとめて表示されます。セッションを選ぶと、会話に切り替わります。

セッション内では、エージェントとの**会話**、ファイルを調べて編集する際の**ツール操作**、差分を確認できる**変更ファイル**の一覧を確認できます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを確認して機能の実装に取りかかります。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼します。

1. GitHub Copilot app を開きます。すでに起動している場合は、そのアプリに戻ります。
2. サイドバーの上部にある **+** を選択して、セッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトを入力して変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。続いてプロジェクトを調べ、新機能に必要なファイルを見つけてコードを作成します。Copilot app で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューします。Copilot app で変更を確認します。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選び、変更されたファイルと差分を確認します。

   ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する主要ファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` の場合は `No rating yet` を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は確率的に動作するため、生成コードは上記と異なる場合があります。比較的似た実装になるはずです。

## 変更を確認する

ブラウザーで確認する前に、エージェントが実行した自動チェックの結果をレビューします。テストが数値の `starRating` と、`null` の場合の代替表示の両方を確認していることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動させ、更新された UI を確認します。アプリを起動して、ブラウザー キャンバスで開くよう依頼します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム Canvas を調べて作成します。ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトで、アプリを起動してブラウザー Canvas に表示するよう Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーが開きます。
3. 評価があるゲームカードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッションが起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、コードベースに新しいコードをマージするためのプルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の案内に従って認証します。
3. リポジトリ ポリシーにより PR の作成前に関連する Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成するまで待ちます。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。ここで PR をレビューできます。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、その構成を学びました。
- エージェントにゲームカードの小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで起動し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェック結果をレビューして、自分でマージしました。

次は、[フィルタリングの Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
