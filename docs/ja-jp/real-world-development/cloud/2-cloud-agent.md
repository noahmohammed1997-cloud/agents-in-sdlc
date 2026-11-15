---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム命令][previous-lesson]

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。手間のかかるタスクを委任することで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成用の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを残しておらず、アプリケーションにはほとんどドキュメントがありません。まず、アプリケーション内のすべてのエクスポート関数に TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、利用者がほかのゲームも引き続き閲覧できるようにします。まだ完成度の高い実装は必要なく、ユーザー エクスペリエンス (UX) の受け入れテストに使える状態にしたいと考えています。現在は作業の妨げになっていますが、より優先度の高い Issue もあります。

こうしたタスクは後回しになりやすく、Copilot cloud agent に任せるのに適しています。cloud agent が非同期で作業している間、開発者は別の作業に取り組み、後で結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]と、Copilot は進捗を追跡するドラフトのプルリクエスト (PR) を作成し、環境を準備して作業を開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、Copilot が PR で開発者に通知します。

## 範囲を明確にした命令の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見簡単そうな操作も、詳細を調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。人工知能 (AI) ペア プログラマーとして扱い、段階的に進め、学びながら試して調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習ではカスタム命令と、Copilot の作業に与える影響を確認しました。カスタム命令の演習をすでに完了した場合は、`.github/copilot-instructions.md` が動作する様子を見ています。まだ確認していない場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された命令ファイルを読み取り専用で確認してください。

実行中の Codespace がある場合はそこで、ない場合はリポジトリの GitHub Web UI で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成された PR に TSDoc の doc comment、TypeScript 規約、コメント ヘッダーが反映されます。後の演習で PR をレビューするときに、これらの命令の影響を確認します。

## Copilot cloud agent 用の開発環境を準備する

コードの作成には、特定の環境やセットアップ スクリプトが必要になることがあります。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[特別なセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。この演習ではワークショップを円滑に進めるため、あらかじめ設定されています。単一の Astro アプリケーションのために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、マイグレーションとシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して、可能な限り最小の権限を設定します。Copilot は操作用の専用トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリを複製する場合（依存関係のインストールなど）は、`contents: read` 権限が必要です。複製しない場合、手順の完了後に Copilot が自動で複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ: Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同じように見えますが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、Copilot が実行するのを待たずに、リポジトリの **Actions** タブからワークフローを手動でテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかないプロジェクトや、ドキュメントがまったくないプロジェクトもあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を支援できるため、このタスクを cloud agent に割り当てます。cloud agent はバックグラウンドで必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかることがあります。Copilot は先ほど紹介したワークフローを使って環境を準備し、プロジェクトを分析して、作業に適した方法を検討しています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザー ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調査して現在の状態を理解し、方針を検討したうえで、コードを作成していきます。

この作業には数分かかる場合があります。Copilot cloud agent の主な利点は、非同期に作業して開発者が別のタスクに集中できることです。もう 1 つのタスクも cloud agent に割り当ててから、アプリケーションの機能開発に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。デザイン チームが希望する関連ゲーム セクションの実装に時間がかからないとしても、その時間を別の作業に使えます。このタスクも Copilot cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことを示します。

![目の絵文字で Copilot が Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアに似た方法で作業するため、常に監視する必要はありません。作業が完了してからレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agent に Issue を割り当てると、非同期で作業を進められます。技術的負債の解消、新しい機能の作成、別のフレームワークへのコード移行などを依頼できます。

この演習では、次のことを確認しました。

- コード生成の環境をカスタマイズしました。
- 操作が安全に行われるようにしました。
- 範囲を明確に定めた Issue の重要性を確認しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
