---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。还需要处理数据、外部服务，并为 Copilot 提供其他自动化能力。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 请智能体操作浏览器，检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面（UI）更新还需要实际与界面交互。让 Copilot 像用户一样操作网站，可以进一步自动化验证过程，并提高对功能正确性的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为人工智能（AI）智能体提供了与外部工具和服务通信的方式，使其能够实时访问最新信息并代表你执行操作。

MCP 服务器是 AI 智能体与外部工具和服务之间的桥梁。每个服务器提供一组可供智能体访问的工具和资源。常见服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的应用编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 将 MCP 服务器视作项目中的其他依赖项。使用前审查源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 通过 Playwright 请 Copilot 探索功能

之前已手动确认功能，现在让 Copilot 使用 Playwright MCP 服务器完成相同检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断应使用 MCP 服务器，不一定需要明确指定。但指出 Playwright MCP 有助于它稳定地使用合适工具。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认行为符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，并报告结果。

## 总结和后续步骤

你已通过 Copilot CLI 和 Playwright MCP 在真实浏览器中探索功能。本课中，你：

- 了解了 MCP 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 请智能体操作浏览器，检查筛选功能。

下一步，创建[质量保证（QA）自定义智能体][next-lesson]，将技能和浏览器工具结合起来。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
