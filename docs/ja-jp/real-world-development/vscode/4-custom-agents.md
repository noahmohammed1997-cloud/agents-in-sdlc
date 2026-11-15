---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにある特定のタスクや分野に特化した人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業を効果的に進めるための命令、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に渡します。チームの専門知識を、たとえば Web Content Accessibility Guidelines (WCAG) への準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントとして再利用できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、命令を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、概念上の共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言えば、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の中でスキル（場合によってはほかのエージェントも）を調整するように設計されています。このラボでは、アクセシビリティ用のカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロー用スキルや、テストの実行と管理を行うスキルを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作成する唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合わせて試し、繰り返し改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、ダーク テーマではテキストと背景色のコントラストが不十分で、読みにくい利用者がいます。このアクセシビリティの問題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。ここでは、カスタム エージェントに機能を生成させます。

この演習では、次のことを確認します。

- 既存のアクセシビリティ用カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装する。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot を導く方法を理解するため、内容を確認します。ファイルを確認する前に、フィルタリングのプルリクエスト (PR) とアクセシビリティ作業を分けるブランチを作成します。

1. Codespace に戻り、ターミナルを開きます。
2. `main` からブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. `.github/agents/accessibility.md` を開きます。
4. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

5. 続いて、次の内容を扱うセクションを読みます。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選ぶと、そのエージェントの命令とツールを使うようにチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で案内します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** を選択します。すでに選択されている場合は、そのまま続けます。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定の名前は **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分とともにチャット内に表示されます。
6. 編集が追加されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリを実際に変更し、Astro コンポーネント、CSS、関連テストなどの既存ファイルを更新します。

次の演習では、実行中の作業をレビューし、エージェントに追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について確認しました。カスタム エージェントは特定のタスクや分野に特化した AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントとして定義し、特定の作業を効果的に進めるよう Copilot を導けます。

この演習では、次のことを確認しました。

- 既存のアクセシビリティ用カスタム エージェントを確認しました。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装しました。

次は、エージェントの作業を[監視して指示を追加し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替え機能を追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
