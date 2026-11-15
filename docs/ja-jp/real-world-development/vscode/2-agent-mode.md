---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに小さな変更を加える場合でも、複数のファイルを更新し、テストを実行するなどの作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更を行い、テストを実行し、問題を修正します。変更が足りなければ、別のファイルを更新して、もう一度テストすることもあります。

このような作業には Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業するように設計されています。プロジェクトの構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じて開発者とやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- GitHub Copilot Agent Mode でユーザー インターフェース (UI) とデータ レイヤーにわたる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けのテーマを使ったボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリケーションで、ビルド時に静的 HTML を生成します。ページは Drizzle のオブジェクト リレーショナル マッピング (ORM) を通じて frontmatter からローカルの SQLite データベースを直接照会します。別のバックエンド アプリケーション プログラミング インターフェース (API) やクライアント側ユーザー インターフェース (UI) フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準ドキュメント オブジェクト モデル (DOM) API を使う小さな範囲の Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後なので、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような Uniform Resource Locator (URL) を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost のアドレスを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Codespace 内の Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、現在の機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で案内します。

Web サイトの初期実装は動作しますが、新機能を追加して改善します。まずバックログを確認します。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルにわたる変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案をできるだけ一貫させ、操作を分かりやすくすることが目的です。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップと異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信し、バックログを表示します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. Copilot が生成した Issue の一覧を確認します。

Copilot が利用者にとって有用だと判断した項目に優先順位を付けたことを確認します。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを伝える命令ファイルを確認します。Web サイトに機能を追加する方法を説明する [user interface (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する際の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの命令ファイルへのリンク。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルとプロジェクト内のファイルの両方を参照できます。パスは命令ファイルからの相対パスです。命令を再利用しやすく分割し、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリングの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードベースにマージする前にテストを実行し、成功することも確認します。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 命令ファイルを追加します。

   ![UI 命令ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを見つける様子を確認します。データ レイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行するなど、複数の作業を行います。タスクやコードベースによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。完了後はすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot は、タスクが完了しエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode との会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードを確認し、問題がないか調べます。

> [!CAUTION]
> Copilot などの人工知能 (AI) ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して複数ファイルを変更しました。
- GitHub Copilot Agent Mode でユーザー インターフェース (UI) とデータ レイヤーにわたる新機能を実装しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の命令ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを補いながら何度かやり取りする場合があります。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使いどころ][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
