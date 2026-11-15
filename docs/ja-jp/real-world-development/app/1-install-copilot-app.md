---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、作業を指示して、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーができたので、次はアプリをインストールして、そのリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートから作成したバックログなど、ワークスペースを確認します。
- クイックチャットでアプリ自体について質問します。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue を選び、エージェントを実行し、変更をレビューして、プルリクエストをマージする作業を一か所から指示できます。このレッスンでは、アプリをインストールしてリポジトリを接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の案内に従って GitHub.com または GHE.com のアカウントでサインインします。データ所在地に対応した GitHub Enterprise Cloud を使う場合は、**GitHub Enterprise Cloud** を選び、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、最初にクラウド アカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインイン ガイド][ghes-sign-in]に従って、ブラウザーでアプリを承認します。

5. オンボーディングでリポジトリの選択を求められたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択を省略して、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを尋ねられたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されているか確認します。すでに表示されている場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した所有者名とリポジトリ名を指定します。
9. サイドバーにリポジトリが表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、その下に個別のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、ワークスペースの構成を確認します。サイドバーには、次の領域があります。

- **New** - Copilot との新しいチャット セッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合から表示されるプルリクエストです。アプリを離れずに、プルリクエストの閲覧や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - 同じ方法で表示される Issue です。Issue の閲覧や絞り込み、Issue からのセッション開始ができます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。To Do リストの管理、定期的なプロジェクト保守、その他の面倒な作業に役立ちます。振り返りでは次のステップとして紹介しますが、ワークショップ内の別の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して Copilot app の機能を拡張します。Playwright MCP の設定に使います。
- **Chats** - 独自のブランチやワークスペースが不要な質問やブレインストーミング向けの軽量な会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択してチャットを開始します。
- **Sessions** - リポジトリ フォルダーと、その中で動作するエージェント セッションをグループ化します。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。セッションごとに独立したワークスペースがあるため、変更を競合させずに複数のセッションを実行できます。最初のセッションは星評価を追加するレッスンで開始します。

ワークショップを進めながら、ワークスペースの各機能を確認します。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法が分からない場合や、何かが可能か知りたい場合は、Copilot が案内します。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** の下にある Tailspin Toys のリポジトリ フォルダーを選びます。フォルダーの下にある個別のセッションではありません。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して詳細を読みます。各 Issue はエージェント セッションの開始点にもなります。簡単な最初の変更が終わったら、フィルタリングの Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を表示する場合は、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。**クイックチャット**では、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後 **Chats** が表示されます。
2. 次の質問で、アプリのセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した Git worktree で実行されるため、変更を競合させずに複数のエージェントを並列に動かせることが分かります。会話は続けることも、新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールし、GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースの構成を確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの質問をする。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
