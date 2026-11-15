---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、ユーザー エクスペリエンスを改善して機能を追加する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

Copilot cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。この Mission control では、オープンなプルリクエスト (PR) があるエージェントのタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに指示を追加し、機能を追加で依頼します。

## シナリオ

ハイ コントラスト モードを実装するようエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を更新する作業をすでに行っているため、ライト モードも加えるのが妥当です。ハイ コントラスト モードに加えてライト モードも実装されるよう、エージェントに指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。たとえば、次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更するアプリケーション プログラミング インターフェース (API) を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - Model Context Protocol (MCP) サーバーを設定する。
   - 割り当てられたタスクを完了するための作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認できます。

## cloud agent に指示を追加する

実行中のタスクを確認したので、ハイ コントラスト モードの作業にライト モードも含めるよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。セッション名は、Copilot が付けた名前や作業状況によって異なります。

   ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot が作業中のセッションに指示するダイアログが強調表示されたエージェント ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot に作業を任せる

更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業に反映します。

前と同じように完了まで数分かかることがあります。その間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して指示するための中心となる、Copilot agents ページを確認しました。Mission control では、実行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクの範囲を変更したり追加のガイダンスを与えたりできます。

このレッスンでは、次のことを行いました。

- Mission control とエージェント ページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに指示を追加し、機能を追加で依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復的にレビューします][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
