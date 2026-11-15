---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成系人工知能 (生成 AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう渡るかを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、影響をリアルタイムで並べて示すのは困難です。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず単体テストを用意します。
- ユーザー インターフェース (UI) はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使うと、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡せます。

### カスタム命令

カスタム命令を使うと、コンテキストや希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

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

命令ファイルを作成する唯一の正解はありません。AI の使い方と同じように、試しながらプロジェクトに合う方法を見つけます。

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

このリポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開き、内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスにも注目します。これらの命令は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの相対 glob を指定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルに適用されます。
5. このプロジェクトの単体テストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて最後まで確認します。`unit-tests.instructions.md` などの命令ファイルや、プロジェクトの既存ファイルへのリンクがあります。大きな命令セットを小さな再利用可能ファイルに分け、コード生成時に Copilot が参考にできる例を示せます。ここに記述されたパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

次は、この演習で行う唯一の編集作業です。コード内に TSDoc の doc comment とファイル レベルのコメント ヘッダーを記述する、プロジェクト全体のルールを追加します。後の演習で Issue を割り当てると、cloud agent はこのルールを読み取ります。

編集の前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチにある命令を読み取りますが、ここでは変更を個別にコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コーディング標準を記述し、コード提案の質を高めます。

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

## 命令の更新をコミット、push、マージする

cloud agent は、Issue が対象とするブランチの命令ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、cloud agent は `main` からブランチを作成します。そのため、命令の変更を `main` にマージしておく必要があります。

1. 変更を stage してコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に取り組む際、cloud agent がこの命令を読み取れるようになります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して、`main` に直接コミットできます。ブランチの手順は、実際のプロジェクトでの進め方を想定しています。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が生成するプルリクエストを確認し、標準が適用されたことを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
