---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成系人工知能 (生成 AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot にコンテキストを伝える方法はいくつかあります。その中でも重要なのが、コード生成のガイダンスを提供する[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令と、パスに応じて適用される命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう渡るかを確認します。
- *現在の*命令を使って、フィルタリングに必要な最初のデータ スライス（publishers ヘルパー）を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 命令の更新とフィルタリングの基礎を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、命令を更新した後の動作の傾向を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず単体テストを用意します。
- ユーザー インターフェース (UI) はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot が理解するためのコンテキストや希望を伝えられます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の命令ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用します。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加に関するガイダンスを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) で作業する場合、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令ファイルと、`applyTo` frontmatter がある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に基づく命令ファイル][custom-instructions-support]にも対応しています。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトの例から、代表的な方法を確認できます。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` ファイルは、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル種別や、特定のタスクに関する命令に使います。
- 自然言語で明確に記述し、コードの望ましい例と望ましくない例を示します。

命令ファイルの作成方法にも、AI の使い方にも唯一の正解はありません。実際に試しながら、プロジェクトに合った方法を見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、生成方法は環境ごとに異なります。各環境のレッスンで、関連する手順を紹介します。
>
> テンプレートや出発点を探す場合は、命令ファイルやカスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリにある、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルを読みます。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスにも注目します。これらの命令は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob を指定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルに適用されます。
5. このプロジェクトの単体テストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて最後まで確認します。`unit-tests.instructions.md` などの命令ファイルや、プロジェクトの既存ファイルへのリンクがあります。大きな命令セットを小さな再利用可能ファイルに分け、コード生成時に Copilot が参考にできる例を示せます。ここに記述されたパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

変更を作業用ブランチに分けるため、前の演習の Codespace に戻り、新しいターミナルを開いてブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する*前に* Copilot Chat を使う

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送ります。その後で命令を更新し、同じプロンプトをもう一度送って違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で案内します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビュー下部のエージェント ドロップダウンから **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューの agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーとそのテストなど、複数のファイルが変更される場合があります。
6. 提案されたヘルパーが型付き関数であり、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づきます。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習では、命令を更新した後に動作の一貫性が高まることを確認します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コーディング標準を記述し、コード提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントのルールはありません。
3. 既存の標準の直後に、次の Markdown 行を追加します。これにより、ファイル コメント ヘッダーと TSDoc の doc comment を追加するよう Copilot に指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルは全体的なガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク用の `*.instructions.md` にも作成できます。今回のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

新しい命令がプロンプトに与える影響を確認するため、Copilot Chat で新しい会話を開始し、同じファイルに同じプロンプトを送信します。

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選び直し、Copilot の対象ファイルにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習の基礎となるコードに適用できました。

## 命令とブランチをコミットして push する

命令ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。変更をコミットし、ブランチをリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、命令の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 次のコマンドで、命令の更新とヘルパーの基礎に関して Copilot が変更したファイルを stage してコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログのドキュメント標準に関する Issue と関連付けてプルリクエスト (PR) を作成します。GitHub ツールがプロジェクトに接続されているため、Copilot に Issue を見つけ、Issue をクローズする PR を作成してから、マージを依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、これが最初の演習です。プロジェクト テンプレートでは GitHub のツールがあらかじめ設定されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の案内に従って認証してください。この接続で使われる Model Context Protocol (MCP) については、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択から **Agent** が選ばれていることを確認します。
3. 命令ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成して、命令の更新と発行元ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成する様子を確認します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作やシェル コマンドの実行を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- *既存の*命令を使ってコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 命令の更新とヘルパーの基礎を `main` にコミットし、次の演習で作業を続けられるようにしました。

次は、[agent mode][next-lesson]でこれらの命令を使い、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
