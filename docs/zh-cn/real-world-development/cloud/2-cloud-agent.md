---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更重要的工作。

本课将了解如何：

- 自定义代码生成环境。
- 安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

设计团队还希望改善游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，方便访客继续浏览。当前只需要用于用户体验（UX）验收测试的初步实现，不要求完善。该工作目前受阻，优先级低于其他议题。

这类容易被推迟的工作适合交给 cloud agent 异步处理。开发者可以继续处理其他任务，稍后再回来审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台处理任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求（PR）跟踪进度、准备环境并开始工作。你可以在任务进行中或完成后查看会话。准备好审查建议方案时，Copilot 会在 PR 中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，使用它仍然需要思考。即使看似简单的任务也可能有复杂细节，因此要[认真界定 cloud agent 的任务范围][cloud-agent-best-practices]。像与人工智能（AI）结对编程一样，分阶段工作、学习、试验并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 查看本存储库中的自定义指令

之前的练习介绍了自定义指令及其作用。分配任务前，以只读方式查看存储库中已有的指令，稍后便能识别它们的影响。

1. 在存储库的 GitHub Web UI 中或已启动的 codespace 中打开 `.github/copilot-instructions.md`，查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，查看 `applyTo` frontmatter，了解这些说明如何应用于 `**/*.test.ts` 文件。
3. 将下一个步骤中的 `Code lacks documentation` 议题分配给 cloud agent 后，留意 PR 中的 TSDoc 注释、TypeScript 约定和文件注释头；它们来自上述指令文件。

## 为 Copilot cloud agent 设置开发环境

生成代码通常需要配置开发环境和运行设置脚本。分配给 Copilot 的任务也需要准备环境。cloud agent 使用 [GitHub Actions][github-actions] 执行任务。可创建专用设置工作流，并将其配置在 `.github/workflows/copilot-setup-steps.yml`，在 Copilot 开始工作前安装必要的工具和依赖项。本实验已预先配置该工作流，确保 Copilot 可以使用 Node.js、项目依赖、端到端测试所需浏览器，以及已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 "Actions" 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低级别。Copilot 会获得用于自身操作的令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有两点值得注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你在存储库的 **Actions** 选项卡中手动运行工作流，以便在 Copilot 使用前检查它是否能正常运行。

## 添加文档

文档常常过时或缺失，成为影响工作效率和维护代码库的技术债务。Copilot 可以在后台生成文档，适合处理这类议题；稍后的练习将审查它的工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot 的界面](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有 PR，等待一两分钟并刷新页面。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> 待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境、运行上文中的工作流、分析项目并确定处理方案。

13. 查看 Copilot 计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 如果状态尚未更新，可能需要刷新页面。

16. 浏览实时会话，观察 Copilot 探索代码、了解状态、思考并制定计划，然后编写代码。

任务可能需要几分钟。利用 cloud agent 在后台工作的同时，可以继续处理其他事情。接下来再分配另一项任务，然后转而编写应用功能。

## 为游戏详情页添加相关游戏

cloud agent 可以并行处理不同工作，让你腾出时间处理其他任务。现在将相关游戏功能分配给它。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue**。
3. 选择 **Blank issue**。
4. 将 **Title** 设置为 `Show related games on the game details page`。
5. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot**，再选择 **Assign**。

   ![将议题分配给 Copilot 的界面](../../../_images/shared-assign-copilot.png)

议题的第一条评论中很快会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。它的工作方式类似于软件工程师，因此无需持续监控，完成后再审查即可。接下来创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 Copilot，让它异步处理技术债务、新功能或框架迁移等任务。

你探索了以下内容：

- 自定义代码生成环境。
- 安全执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

cloud agent 正在后台工作，接下来创建并使用自定义智能体。cloud agent 也可以使用[模型上下文协议（MCP）服务器][cloud-agent-mcp]和此前探索的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
