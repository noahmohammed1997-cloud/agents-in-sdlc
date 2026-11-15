---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能获取这些信息。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做*什么*，也了解应该*如何*完成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 开发环境不同，本练习不会运行前后对比提示。Copilot cloud agent 会在 GitHub 上异步处理议题，因此难以实时并排展示指令的影响。后续审查它创建的拉取请求时，可以看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

通过指令文件，可以确保 Copilot 按照这些实践完成工作。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它遵循编码风格和需求。可以指定编码约定、库以及注释规范，并为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个文件，内容应包括适用于大多数请求的项目级信息，如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot 命令行界面（CLI）和 Copilot cloud agent 生成代码时，会使用存储库级指令和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
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
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以拆分并复用较大的指令，也可以为 Copilot 提供代码示例。路径相对于指令文件本身，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头规则。

## 添加新的存储库标准

现在为 `.github/copilot-instructions.md` 添加项目级规则，要求代码使用 TSDoc 注释和文件注释头。后续将分配议题，cloud agent 会读取这些规则。

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分（大约在第 27 行）。确认其中记录了编码标准，但还没有代码文档规则。
4. 在现有标准下方添加以下 Markdown 行：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级的 `.github/copilot-instructions.md` 适合放置所有任务都应遵循的标准；`*.instructions.md` 则适合特定语言、文件类型或任务。

## 提交、推送并合并指令更新

cloud agent 会从议题目标分支创建分支。下一练习中它将从 `main` 开始，因此必须先把指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，cloud agent 处理下一练习分配的议题时就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上完成本工作坊，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤是为了模拟真实项目的处理方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项全库标准。后续练习中，可以在 cloud agent 创建的拉取请求中看到该标准的作用。

下一步，将第一个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
