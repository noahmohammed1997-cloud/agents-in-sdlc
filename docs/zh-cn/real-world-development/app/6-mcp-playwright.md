---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "通过 Customize 配置 Playwright MCP，并在现有功能工作树中通过浏览器观察筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-07-09
---

如前所述，编写代码不只是编写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用更多自动化功能。这正是模型上下文协议（MCP）服务器的作用。MCP 服务器可以让 Copilot 使用 app 内置功能以外的工具和服务。

本课将：

- 了解模型上下文协议（MCP）以及 GitHub Copilot app 如何使用它。
- 添加 Playwright MCP 服务器。
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证用户界面（UI）的更新，需要实际与界面交互。你希望 Copilot 像用户一样使用正在开发的网站，以进一步自动化更改，并提高对更新符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为人工智能（AI）智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方的通信。外部工具可以是现有应用编程接口（API），也可以是 npm 包等本地工具。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

以下是两种常用的 MCP 服务器：

- [**GitHub MCP Server**](https://github.com/github/github-mcp-server)：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以创建存储库、更新现有存储库，以及管理议题和拉取请求。
- [**Playwright MCP Server**][playwright-mcp-server]：使用 Playwright 提供浏览器自动化功能。AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可用于访问不同的工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，以提高生态系统的可发现性并促进贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 添加 Playwright MCP 服务器

通过侧边栏中的 **Customize** 管理 MCP 服务器。为存储库或 Copilot CLI 配置的服务器可能已在 app 中可用，因此添加前先检查，避免重复。[app 自定义文档][customize-app]介绍了可用选项。

1. 在侧边栏中选择 **Customize**。
2. 打开 **All types** 下拉菜单并选择 **MCP**，然后检查 **Installed** 列表中是否已有 Playwright 服务器。
3. 如果尚未安装 Playwright，请在可用服务器中找到 **Playwright**，然后选择 **Add server**。
4. 批准前审查发布者、配置和安装提示。按照提示添加服务器；组织策略或缺少先决条件可能会阻止设置。
5. 返回处于 **Interactive** 模式的筛选会话，确认 Playwright MCP 工具可用。
6. 如果设置失败，先解决配置或权限问题再继续。

## 让 Copilot 通过 Playwright 探索功能

议题和规划决策已在上下文中。让 Copilot 启动开发服务器前，先停止之前启动的服务器。

1. 使用以下提示要求 Copilot 验证新功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. 观察 Copilot 的操作。

> [!NOTE]
> 不必明确要求 Copilot 使用特定的 MCP 服务器；它通常会根据当前上下文选择合适的服务器。不过，明确指出你认为重要的信息也很有帮助。

Copilot 会启动服务器、打开浏览器并与网站交互。完成后，它会停止服务器并提供报告。

## 总结和后续步骤

你已使用 Playwright MCP 服务器，从 GitHub Copilot app 在真实浏览器中探索功能。本课中，你：

- 了解了模型上下文协议（MCP）以及 GitHub Copilot app 如何使用它。
- 添加了 Playwright MCP 服务器。
- 让智能体操作浏览器并探索筛选功能。

下一步，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 是什么？为什么每个人都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [在 GitHub Copilot app 中配置 MCP 服务器][customize-app]

[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[customize-app]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
