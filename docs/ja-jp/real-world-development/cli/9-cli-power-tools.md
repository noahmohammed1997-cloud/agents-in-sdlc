---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、多くの slash command が用意されています。これらのコマンドを使うと、高度な機能や内部状態の情報を確認したり、追加設定を行ったりできます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどのように管理するかを確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが終わりました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の作業委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模で複雑なタスクに取り組むと、モデルのコンテキスト ウィンドウが上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command を使って、状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別のタスクに切り替え、新しい会話を始める場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用するモデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。一覧を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力し、学んだことを共有すると、全員が AI ツールをより効果的に使えるようになります。`/share` を使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合った出力先を選びます。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並列化する、または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分け、並列に実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で処理させることができます。完了後にプルリクエストが作成される場合があります。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の操作です。試すときは、範囲の明確な新しいタスクから始め、通常の手順に沿って結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[クラウド エージェントのワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更したり、セッションを共有したり、内部状態を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどのように管理するかを確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認しました。

ほかにも多くの slash command と機能があります。最後に[ここまでの内容を振り返り][next-lesson]、次のステップを確認して締めくくります。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
