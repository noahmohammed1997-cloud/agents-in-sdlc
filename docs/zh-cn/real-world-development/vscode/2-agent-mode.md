---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、修改代码、运行测试、调试，再检查是否遗漏文件。Copilot 代理模式可在 IDE 中更自主地工作：它会探索项目结构、执行必要更改、运行测试，并尝试自动修复发现的问题。现在使用它为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于不同场景。代理模式在 IDE 中执行任务，可快速反馈并实时交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将了解 Copilot 代理模式如何：

- 探索项目并查找相关文件，协调多文件更改。
- 跨 UI 和数据层实现新功能。
- 在合并到代码库前审查 Copilot 生成的更改和测试。

## 场景

随着游戏列表增加，访客需要按类别和发行商筛选。上一练习已添加 publishers helper，现在使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先了解 Tailspin Toys 网站当前的功能。它是一个面向开发者的桌游众筹平台，可浏览游戏列表和详情。网站是单个 Astro 应用，构建时渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能使用范围明确的 Astro `<script>` 和标准 DOM API。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回 codespace。上一练习结束后应位于 `main`。
2. 打开新终端：按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示询问是否打开 `http://localhost:4321`，选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 中网站的私有隧道。

### 浏览网站

网站启动后，可以浏览现有功能：

- **主页**：显示游戏名称、图片和描述。
- **游戏详情页**：选择游戏后可查看标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

在项目模板中，GitHub 议题待办事项已自动创建。先请 GitHub Copilot 显示这些项目。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开启干净的对话。模式和模型会根据练习而变化；相关步骤会指出需要的设置。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并不偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此示例使用该模型，以便尽量保持建议一致。大型语言模型（LLM）具有概率性，收到不同建议完全正常。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看 Copilot 生成的议题列表和优先级建议。

## 查看指令文件

让智能体生成代码前，先查看用于提供项目上下文的指令文件。项目中的 [用户界面（UI）指令](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括应用架构、组件设计原则、可测试性、无障碍要求，以及指向 Astro、Tailwind CSS 和 Drizzle 指令文件的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要三类更改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还必须运行并通过测试。Copilot 代理模式可以协助完成这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表仍选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、查找数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo**。Copilot 完成后，每项更改都会显示相应选项；工作进行时无需提前选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，或暂时出现测试失败。Copilot 会在代码生成和测试之间迭代，直至没有发现错误。
9. 检查生成的代码是否有潜在问题。

   ![Copilot 代理模式会话完成时的 Chat 窗口](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，在浏览器中体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了功能。你了解了它如何跨 UI 和数据层实现功能、探索项目并协调更改，以及如何在合并前审查更改和测试。

下一步，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索：实现分页

随着游戏列表变长，网站可能需要分页。可以使用本练习学到的方法，让 Copilot 添加分页：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定是否允许用户选择每页条目数，还是使用固定数量。
- 编写提示时，提供 Copilot 实现预期功能所需的指导。
- 根据需要与 Copilot 迭代、请求更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
