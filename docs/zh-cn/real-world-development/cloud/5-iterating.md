---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查智能体的工作

在本实验中，你让 GitHub Copilot 执行了多项任务，以改善用户体验并增加功能：为代码添加文档、实现相关游戏功能，以及添加高对比度和浅色模式切换。现在审查代码更改，必要时向 Copilot 提供反馈。

### 场景

生成式人工智能（AI）不会改变软件设计和开发运维（DevOps）的基本原则。代码仍需审查，并遵循常规 DevOps 流程。接下来审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，然后再允许团队审查。

## GitHub Copilot cloud agent 的安全性

Copilot cloud agent 在无人监督的情况下异步执行任务，因此受到以下安全限制：

- Copilot 对存储库只有读取权限，对用于提交代码的分支才有写入权限。
- cloud agent 在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- 默认情况下，外部资源（包括 MCP 服务器）的[访问权限受限][agent-firewall]。

## 审查生成的文档

先通过 GitHub.com 的常规拉取请求（PR）界面，查看 Copilot cloud agent 创建的第一个文档 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为 Copilot 默认只能有限访问外部资源，包括外部模型上下文协议（MCP）服务器。如有需要，可以[自定义或禁用 cloud agent 防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会包含 **[WIP]**。等待任务完成，可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![PR 中的 Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释及其他文档。具体更改可能不同；留意 Exercise 2 中查看过的 TSDoc、TypeScript 约定和文件注释头。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 确认工作流已加入 PR 的检查列表。单个 Astro 应用的项目检查应通过；等待可能需要几分钟。

## 请求 GitHub Copilot 修改

可以像在 PR 或代码行内评论中提及团队成员一样，使用评论中的 **@copilot** 请求 Copilot 修改。Copilot 会读取评论并启动新会话。由于结果具有非确定性，不能预先规定唯一的提示。可以要求它：

- 在代码文件开头添加简短的文件用途注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 添加评论并提及 **@copilot**，请求修改生成的文档。可以选择上述建议或提出其他代码文档要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并进行修改。
3. 需要返回 PR 时，选择 **Back to pull request**。

   ![返回拉取请求的按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新提交。
5. 选择 **Files changed** 选项卡审查更改。

满意后，可以将 PR 从草稿状态改为可审查状态，再合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流；如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

7. 确认工作流已加入检查列表，并等待单个 Astro 应用的项目检查通过。此过程可能需要几分钟。
8. **可选：**在 Codespace 中手动测试相关游戏功能。打开终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 `copilot/fix-8`：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。你可以通过在 PR 的 **Conversation** 选项卡中添加评论，要求 cloud agent 继续修改，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 注释。注意：分配该议题时，之前添加的自定义指令尚未合并。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。PR 应包含 Exercise 3 中分配的高对比度模式，以及 Exercise 4 中追加的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 还在处理任务，PR 标题会带有 **[WIP]**。等待它完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 重点检查模式切换用户界面（UI）、使用浏览器本地存储保存用户偏好、两种模式对应的层叠样式表（CSS）样式，以及无障碍富互联网应用（ARIA）标签、键盘导航和负责切换模式的 JavaScript/TypeScript 代码。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流；如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

8. 确认工作流已加入检查列表，并等待单个 Astro 应用的项目检查通过。
9. **可选：**如需手动测试无障碍功能，可切换到 Copilot 创建的分支：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在重新加载页面后仍然保留。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现问题，可以像之前一样在评论中提及 **@copilot**，请求进一步修改。

## 可选练习：继续委派工作

cloud agent 适合处理真实待办事项，让开发者有时间专注其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 开发环境的练习。在这些课程中，你：

- 查看了存储库提供的自定义指令文件，以便之后观察它们对 cloud agent 输出的影响。
- 将议题分配给 Copilot cloud agent，观察它准备环境、规划并异步执行任务。
- 创建并使用无障碍自定义智能体，添加高对比度和浅色模式切换。
- 使用 Copilot agents 页面监控并引导无障碍会话。
- 审查并迭代 cloud agent 的 PR，通过 **@copilot** 请求修改并批准工作流。

## 回顾和后续步骤

Cloud agent 开发环境的练习已完成。其他开发环境可以补充本工作坊的实践：

- [VS Code 开发环境](../../vscode/)：在集成开发环境（IDE）中体验 Copilot Chat 代理模式和 MCP 集成。
- [CLI 开发环境](../../cli/)：在终端完成并审查两项更改，使用命令行界面（CLI）的 Plan 和 Autopilot 模式、技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

还可以在自己的存储库中尝试：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
