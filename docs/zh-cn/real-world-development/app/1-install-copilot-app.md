---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于代理式开发的桌面应用。它基于 GitHub Copilot CLI（命令行界面）构建，并与 GitHub 原生集成，因此存储库、分支和持续集成（CI）管道都可直接使用。它适用于由你指挥多个智能体并行工作的流程，每个智能体都在独立工作区中运行；app 还能自动执行重复任务，让你不必亲自完成所有工作。你已安装 Node.js 并准备好项目副本，接下来安装 app 并连接该存储库。

本课将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到 app。
- 熟悉工作区，包括模板为你预置的待办事项。
- 尝试快速聊天，了解 app 本身。

## 场景

团队正采用人工智能（AI）智能体处理不断增长的待办事项。Copilot app 将工作集中在一处：选择议题、运行智能体、审查更改并合并拉取请求。本课将完成安装和连接，并熟悉如何围绕项目开始对话。

> [!NOTE]
> 你需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装和配置 GitHub Copilot app

首先安装 GitHub Copilot app。它提供适用于 Windows、macOS 和 Linux 的版本。接下来安装 app、完成身份验证并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 下载页面][download-app]。
2. 下载适用于当前平台的 app，并按照下载页面上的说明安装。
3. 安装完成后打开 app。
4. 选择 **Sign in to GitHub**，并按照提示使用 GitHub.com 或 GHE.com 账户登录。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **GitHub Enterprise Cloud**，并在提示时输入 `*.ghe.com` 主机名。

   如果使用 GitHub Enterprise Server，请先使用云账户完成引导设置。然后打开 **Settings**，选择 **Accounts**、**Add account** 和 **GitHub Enterprise Server**。按照提示在浏览器中授权 app，详情请参阅 [Enterprise Server 登录指南][ghes-sign-in]。

5. 如果引导设置提供存储库选项，请选择通过模板创建的 Tailspin Toys 存储库。也可以跳过此项，在引导设置完成后再添加项目。
6. 按提示选择主题，然后完成引导设置。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需再次添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁边的 **+** 以创建新会话，然后选择 **Add GitHub repository**。使用先决条件课程中记录的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。此布局会在 **Sessions** 下按存储库分组，例如 `tailspin-toys` 文件夹及其下的各个会话。

## 熟悉工作区

项目连接后，先熟悉工作区。侧边栏中的区域包括：

- **New**：开始与 Copilot 的新聊天会话。
- **Pull requests**：通过 app 的 GitHub 原生集成查看拉取请求。在此可浏览和筛选拉取请求、查看 CI 状态并进行审查，无需离开 app。
- **Issues**：同样通过集成查看议题。在此可浏览和筛选议题，并从议题启动会话。
- **Automations**：保存按计划或按需运行的智能体任务。可用于管理待办列表、定期项目维护或其他重复工作。本工作坊收尾时会将其作为后续探索内容，不是额外练习。
- **Customize**：添加模型上下文协议（MCP）服务器、插件、技能和其他组件以扩展 Copilot app。后续会在此配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量级对话，无需单独创建分支或工作区。首次聊天后此区域才会显示。如果未显示，请选择 **Sessions** 旁边的 **+** 开始第一次聊天。
- **Sessions**：按存储库文件夹分组显示智能体会话。选择存储库名称（如 `tailspin-toys`）可打开其存储库视图；选择存储库下的会话可返回对应对话。每个会话都在独立工作区运行，因此可并行开展多个任务而不会相互影响。添加星级评分时，你将启动第一个会话。

随着工作坊继续，你会逐步了解这些区域。

> [!TIP]
> 如有疑问，可以直接询问 Copilot。如果不确定某项操作如何完成，或是否可行，可以请 Copilot 指引你。

### 查找预置待办事项

大多数项目都有待办事项，Tailspin Toys 也不例外。现在查看创建模板时预置的待办事项。

1. 在侧边栏的 **Sessions** 下选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设它们的议题编号固定：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个议题阅读详情。每个议题也可用于启动智能体会话。完成一个快速的首次更改后，你将从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。若要查看其他存储库的工作项，请将相应存储库添加到 app。

## 尝试快速聊天

熟悉 app 的好方法是向它询问 app 本身的问题。**快速聊天**可用于提问或头脑风暴，不会创建分支或工作树，适合快速、临时的提问，无需启动会话。

1. 在侧边栏中选择 **Chats** 旁边的 **+**，开始新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁边的 **+** 开始第一次聊天；之后 **Chats** 会出现。
2. 询问 app 会话的工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 阅读对话视图中的回复。你会了解到，每个会话都在独立的 Git 工作树中运行，因此可并行启动多个智能体而不让更改相互冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

你已安装 GitHub Copilot app、连接项目并熟悉工作区。本课中，你：

- 安装了 app 并登录 GitHub。
- 从 GitHub 存储库添加了项目。
- 熟悉了工作区，并在 **Issues** 中找到了预置待办事项。
- 使用快速聊天提出了一个简短的临时问题。

下一步，[启动第一个智能体会话][next-lesson]，在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [开始使用 GitHub Copilot app][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
