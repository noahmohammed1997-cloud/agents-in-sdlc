---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages のワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを設定してもらいます。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、利用者が簡単に試す方法はまだありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時の依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストの作成時に、コードがコミットおよび push される場合があります。このプロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**Approve all** が有効な場合、別途確認するメッセージは表示されないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

変更内容を確認し、マージを許可する前に、ワークフローが意図したブランチからデプロイし、必要な設定を守っていることを確認します。

1. README はリポジトリにまだ存在しない場合に限って追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限を含め、プルリクエストの変更をレビューします。
3. ワークフローが、マージされていないプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新後の差分をレビューします。マージを許可する前に、必須のプルリクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送ると、レビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、追加の確認なしにこれらの操作が実行されます。

`<PR_NUMBER>` をレビュー済みプルリクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストのマージ、Build website、Upload artifact、Deploy to Pages の各ステップを含む GitHub Actions の Deploy website 実行成功、yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

デプロイが成功し、公開サイトにアクセスできることを確認します。

1. GitHub Actions のデプロイ実行を追跡し、成功するまで待ちます。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは完了と判断しないでください。

## まとめと次のステップ

Space Quiz をサイトとして公開し、エージェントの報告に頼らずデプロイを確認しました。次は、[レッスン 13「振り返りと次のステップ」][next-lesson]に進みます。

[next-lesson]: ../13-review/
