---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能获取这些信息。[指令文件][instruction-files]可为代码生成提供指导。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 在当前指令下生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 提交指令更新和筛选功能基础代码，并合并到 `main`，供下一练习使用。

> [!CAUTION]
> Copilot 具有非确定性，生成的代码可能不符合所有标准。本练习旨在观察更新指令后行为的一致性变化，不要求逐字匹配示例输出。

## 指令文件

### 场景

Tailspin Toys 的开发实践包括以下要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

通过指令文件，让 Copilot 根据这些实践完成工作。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库以及注释规范，并为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个文件，内容应包括适用于大多数聊天或 CLI 请求的项目级信息，如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 生成代码时，会使用存储库级指令和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊范围。项目示例展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 用于项目级指导，如项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持说明清楚，并提供正反示例。

指令文件没有唯一正确的写法，就像使用 AI 也没有唯一正确的方法。通过实践找到适合项目的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应有完善的指令文件。项目中包含适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同开发环境的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；相关练习会介绍具体用法。
>
> 如需模板或起点，可查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库随附的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指引；这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件适用这些说明。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以拆分并复用较大的指令，也可以为 Copilot 提供示例。路径相对于指令文件本身，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头规则。

## 为更改创建分支

先为本练习的更改创建分支。

1. 返回上一练习中的 codespace。
2. 打开新终端：按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

为了观察自定义指令的影响，先在当前指令生效时发送提示。更新指令后，再次发送相同提示并对比结果。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开启干净的对话。模式和模型会根据练习而变化；相关步骤会指出需要的设置。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，使 Copilot 可以修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，可能会修改 helper 文件及其测试。
6. 注意生成的 helper 是带类型的函数，第一个参数接收 `db` 客户端，返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，即使没有要求也可能添加文档注释。如果出现这种情况也没关系；本练习的重点是更新指令后行为的一致性变化。

## 添加新的存储库标准

现在在 `.github/copilot-instructions.md` 中添加存储库编码标准，改善 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。确认其中记录了项目编码标准，但尚未说明代码内文档要求。
3. 在现有标准下方添加以下 Markdown 行：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级的 `.github/copilot-instructions.md` 适合放置项目通用标准；`*.instructions.md` 可用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准。

## 重新运行提示并观察变化

指令中已加入文档注释规则。现在让 Copilot 再次生成 publishers 文件。

1. 返回 Copilot Chat 并选择 **New Chat** 清除当前对话上下文。
2. 再次单击 `src/lib/publishers.ts`，让 Copilot 聚焦该文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看生成的文件，确认开头出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 确认生成的函数包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot 已遵循新的项目标准，并将其应用于下一练习将继续使用的代码。

## 提交指令并推送分支

使用与其他资源相同的源代码管理流程提交指令和 helper 更改。

1. 打开新终端：按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
2. 运行 `git status`，确认指令和 helper 更改已存在。
3. 暂存并提交指令更新和 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以使用项目中已连接的 GitHub 工具完成此操作。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub；按照提示授权即可。后续练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，先审查命令，再选择 **Allow** 授权。
6. 创建 PR 后，使用以下提示要求 Copilot 合并 PR，并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求执行 GitHub 操作和 shell 命令时，先审查命令，再选择 **Allow** 授权。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从指令文件获取上下文，并在 VS Code Copilot Chat 中完成以下工作：

- 发送代码生成提示，观察现有指令的影响。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。
- 再次发送相同提示，观察代码如何采用新标准。
- 提交指令更新和 helper 基础，并合并到 `main`，让下一练习可以继续使用。

下一步，使用[代理模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
