---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能可用。接下来连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

代理模式连接编辑器以外的工具后，能力会大幅增强。模型上下文协议（MCP）是一种让 Copilot 与外部工具和服务通信的标准方式。

![代理模式与上下文、LLM 和工具之间交互关系的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)可让 AI 智能体与外部工具和服务实时通信，获取最新信息（通过资源）并代表你执行操作（通过工具）。

MCP 服务器是 AI 智能体与外部工具和服务之间的桥梁，负责管理双方通信，例如现有 API 或 NPM package 等本地工具。每个 MCP 服务器提供一组可供智能体访问的工具和资源。常见服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化能力，可导航网页、填写表单和选择按钮。

GitHub 还提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 将 MCP 服务器视作项目中的其他依赖项。使用前应审查源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予其敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 配置当前 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 确认项目已预先配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

模板项目提供了此 `github` 配置，因此上一练习中 Copilot 能读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。 [Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁添加 `playwright` 配置：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 表示无可见窗口运行浏览器，这是 codespace 中的必要设置，因为那里没有桌面环境。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，所需浏览器通常已安装。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，选择 `playwright` 配置上方出现的 **Start** 操作，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要相关工具时启动服务器，并询问是否信任它。

## 测试筛选功能

Playwright MCP 服务器让 Copilot 可以操作真实浏览器。它能打开页面、导航、应用筛选并读取结果，而不是让你手动操作应用。该服务器使用页面的[无障碍树][playwright-mcp-server]而非截图，因此智能体会基于带标签的按钮、链接和列表项等结构化元素进行推理，与辅助技术的工作方式类似。

连接服务器并启动应用后，请 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器并执行各项检查。将结果与议题验收标准对照；如果有异常，继续追问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试要求应用运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。首次使用 Playwright MCP 时，Copilot 可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支，以便团队审查。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建 PR。也可以让 Copilot 查找相关议题并关联到 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat** 开始新会话。
2. 在智能体下拉列表中确认已选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示进行 GitHub 身份验证；按照提示授权。
5. 确认 Copilot 找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了 MCP 如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布了分支并创建筛选功能 PR。

MCP 服务器可用于测试功能，也可扩展 Copilot 的其他能力。下一步，创建[自定义智能体][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
