---
title: "第 8 课 - 创建并合并功能拉取请求"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建拉取请求（PR）并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一并审查这些更改并创建拉取请求（PR）。你已自行合并星级评分 PR；这次将让 **Agent Merge** 管理整个流程。

> [!NOTE]
> 实际工作中，通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR。为简化本工作坊，我们在同一会话和分支中完成筛选和质量工作，并将所有更改纳入此 PR。

本课将：

- 了解 Agent Merge 及其如何自动执行合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理 PR 中剩余的工作，同时由开发者掌控合并授权。

## Agent Merge 简介

**Agent Merge** 可自动完成在 GitHub Copilot app 中合并拉取请求所需的剩余工作。启用后，app 会话会读取 PR 并处理阻塞项，包括修复失败的持续集成（CI）检查、回应审查意见，以及在需要时变基。GitHub 允许合并后，它会立即合并 PR。该功能在后台运行，app 重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理，但它仍需要你的明确授权才能编辑代码和合并。授予合并权限前，先审查其允许的操作和工作内容。

## 使用 Agent Merge 管理 PR

代码都已创建并审查完成，现在让 Agent Merge 管理 PR 流程。先选择智能体并启用 Agent Merge，然后检查差异和验证证据，再授权合并。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁边的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅选择下拉菜单中的选项并不会启动流程。

Agent Merge 流程随即启动。它将：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题启动，则在 PR 正文中引用相关议题。
- 对目标分支执行变基，或处理潜在的合并冲突。
- 监视 CI 流程，确保所有检查通过。
- 监视其他开发者或 Copilot 代码审查在 PR 中提供的反馈，并进行更新以解决这些意见。
- 可以选择在所有操作成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 在授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁边的下拉菜单。
7. 确认 **Merge pull request** 旁边有勾选标记。
8. 明确要求智能体合并：

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 跟踪进度并解决报告的阻塞项。继续前确认 PR 状态为 **Merged**；已启用合并权限或正在运行的智能体并不能证明 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护，也无法弥补缺失的权限。继续前先解决这些阻塞项。

## 总结和后续步骤

你已自动执行开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理 PR。本课中，你：

- 了解了 Agent Merge 及其如何自动执行合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

下一步，[使用现有画布并创建一个分流画布][next-lesson]，体验与智能体共同检查、规划和可视化工作的更多方式。

## 资源

- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
