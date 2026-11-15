---
title: "第 2 课 - 添加星级评分：快速上手"
description: "在 GitHub Copilot app 中启动第一个智能体会话，为游戏卡片添加小型更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

你已熟悉工作区并尝试快速聊天，现在可以启动**智能体会话**，为项目做第一个更改。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示。你将让智能体显示该评分、审查更改并将其作为第一个拉取请求合并。

本课将：

- 启动智能体会话并了解会话的组成。
- 让智能体对项目进行小而明确的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，该评分已显示在游戏详情页中。但主页的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让智能体在每张卡片上显示现有评分。这项小而独立的更改很适合第一个会话。

## 会话的组成

**会话**是智能体在独立工作区中运行的一段对话。每个会话都有专用的 **Git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复错误，而不会让更改相互冲突。侧边栏会按存储库分组显示会话；选择任一会话即可切换到该会话。

会话中包含三个部分：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及显示已更改文件和差异的**文件列表**。

## 启动会话并请求更改

现在为项目启动新会话，让智能体探索并实现此功能。你已在[设置 app 时][prior-lesson]通过 GitHub 存储库添加项目，接下来为该存储库创建会话并提出更改请求。

1. 返回或打开 GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 选择 `tailspin-toys` 存储库。
4. 选择**新工作树**，并在提示框下方选择 **Interactive** 模式。输入以下提示：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树（项目的隔离副本），然后探索项目、找到需要修改的文件并生成代码。现在你已使用 Copilot app 添加了一项功能。

## 审查差异

即使是小更改，也应在合并前审查所有人工智能（AI）生成的更改。你可以直接在 Copilot app 中检查。

1. 在 app 右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

   ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 检查核心游戏详情显示文件 `GameCard.astro` 中是否添加了代码。实现应与以下示例类似：有评分时显示评分，`starRating` 为 `null` 时显示“No rating yet”：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，并非确定性，因此具体代码可能与示例不同，但整体实现应相近。

## 检查更改

打开浏览器前，先查看智能体自动检查的结果。确认测试涵盖数值型 `starRating` 和 `null` 回退情况。缺少前置条件或跳过的检查不算通过；批准任何安装请求前请先审查。

不要只看代码就假定功能正常。接下来让 Copilot 启动网站，并在浏览器画布中查看更新后的界面。

> [!TIP]
> 画布是 GitHub Copilot app 中可交互的小组件。后续你将探索自定义画布并创建自己的画布；本课先使用内置浏览器画布。

1. 使用以下提示请求 Copilot 启动应用并在浏览器画布中打开页面：

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. 稍等片刻，应用会启动，浏览器窗口会在 Copilot app 内打开。
3. 确认有评分的游戏卡片显示了五分制评分。
4. 检查完毕后，使用以下提示请求 Copilot 停止本会话启动的开发服务器：

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 创建并合并第一个拉取请求

功能已完成，接下来创建拉取请求（PR），将新代码合并到现有代码库。

1. 在右上角选择 **Create PR**。
2. 如果出现提示，选择 **Sign in with your browser** 并按说明完成身份验证。
3. 如果 Copilot 说明存储库策略要求创建 PR 前必须关联匹配的议题，请确认它应遵循该流程。
4. Copilot 将开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR 并按需审查。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

你已使用 GitHub Copilot app 发布第一个更改。本课中，你：

- 启动了智能体会话并了解了会话的组成。
- 指示智能体对游戏卡片进行小而明确的更改。
- 在工作区差异视图中审查了更改。
- 在本地运行应用并在浏览器中确认了星级评分。
- 创建了第一个 PR、审查其检查结果并明确合并。

下一步，[从筛选议题启动会话并使用 Plan 和 Autopilot 模式][next-lesson]，构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
