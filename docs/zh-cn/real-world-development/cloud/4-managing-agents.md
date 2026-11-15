---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你已让 Copilot cloud agent 承担三项改进用户体验和增加功能的任务。cloud agent 虽然异步自主工作，但仍需要监控。

可以使用 GitHub 上的[智能体页面][agents-page]管理任务。在这个控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，检查已执行的操作，也可以在会话进行时引导智能体。

本课将：

- 查看智能体页面并监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

团队在委派高对比度模式后，决定也添加浅色模式。由于智能体已经在修改网站样式并添加切换功能，可以在会话进行中引导它同时实现浅色模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表，其中应包括更新代码文档、生成游戏 API，以及为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如检出存储库、准备环境、设置模型上下文协议（MCP）服务器和完成任务的其他步骤。

> [!NOTE]
> 显示的具体步骤会因任务状态和 Copilot 采用的方法而异。

4. 查看右侧显示的 PR 面板，其中列出 PR 和更改文件，可用于进一步监控。

## 引导 cloud agent

现在在智能体实现高对比度模式的同时，要求它增加浅色模式切换开关。

1. 选择标题涉及添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和任务状态。

   ![智能体控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话完成设置并开始工作。此时会出现类似下文所示的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的任务界面](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已收到提示，并将其纳入任务。

## 让 Copilot 继续工作

Copilot 会在完成当前步骤后，将新请求加入工作流程。等待期间，可以回顾此前完成的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪正在进行和已完成的任务、审查工作，也可以调整正在运行的任务范围或补充指导。

你探索了以下内容：

- 使用智能体控制中心和智能体页面监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
