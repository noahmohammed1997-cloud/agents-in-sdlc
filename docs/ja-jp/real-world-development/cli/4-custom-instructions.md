---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、求めるコードの内容だけでなく、コードの構成方法も記述できます。フィルタリング機能を実装したので、Copilot が参照した命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令ファイルと、パスに応じて適用される命令ファイルがエージェントにどう渡されるかを確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも更新します。
- 既存の書式と lint のガイダンスを維持します。

命令ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## 命令ファイル

カスタム命令でコンテキストや希望を伝えると、Copilot がコーディング スタイルや要件を理解しやすくなります。たとえば、コーディング規約、ライブラリ、コードに含めるコメントの種類を指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の命令ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別にガイダンスを適用します。

> [!NOTE]
> ほかの命令形式への対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム命令の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが用意されています。変更を加える前に、既存の内容を確認して、命令の影響を把握します。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。この命令は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令を適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな命令セットを小さな再利用可能ファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルには十分な情報がありますが、新しく生成される TypeScript に TSDoc コメントを追加する標準がまだありません。中核となる `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の項目として、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの命令を読み込みます。フィルタリングのコンテキストを保ったまま、新しいガイダンスを読み込めるよう、いったん会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替わっていることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   メッセージが表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの命令ファイルと、パスに応じて適用される命令ファイルがエージェントにどう渡されるかを確認しました。
- 命令ファイルを更新して、コーディング標準を追加しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令の対応状況][custom-instructions-support]
- [効果的なカスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
