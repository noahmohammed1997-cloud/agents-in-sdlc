---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "先用 Plan 确定方案，再用 Autopilot 根据议题构建筛选功能，最后在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚刚为项目添加了一项小功能。较大的更改则需要更完整的流程。GitHub Copilot CLI 可以融入团队现有的开发流程，帮助从议题出发生成新功能、验证代码和行为，并最终合并到项目中。本课将开始体验这一典型的智能体开发流程。

> [!NOTE]
> 后续功能开发将继续使用同一对话和分支。实际工作中，通常会为不同类型的改动使用不同分支或 PR；本工作坊为了聚焦核心概念，将合并处理。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库指令、自定义现有的 `quality-checks` 技能、使用 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩充，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了功能，但组合筛选等细节需要在编码前确定。你将使用 Plan 模式讨论这些决定，再授权 Autopilot 完成范围明确的实现。

## 背景

引入人工智能（AI）编码智能体不会改变软件开发的基本流程。常见流程包括：

1. 从描述需求的议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多与上述流程类似。

遵循标准流程，可以确保 AI 生成的代码符合需求，并经过与手写代码相同的验证。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作；智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式审查计划，再用 Autopilot 实现。

## 从议题开始

开始筛选功能前，先确认 Codespace、存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，然后返回 **Session** 选项卡。

提示现在会以 `#7` 或类似编号开头。`#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

在委托 Copilot 实施前，先明确功能方案。复杂任务尤其值得花时间规划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的方案。它可能会询问筛选行为的细节，请根据自己的预期回答。

3. 计划生成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 浏览计划，并要求 Copilot 修改任何不符合决定的部分。

## 批准 Autopilot

计划审查完成后，使用 Autopilot 实施功能。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此要先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，先解决环境问题，再判断检查是否完成。

## 审查并验证实现

像其他代码一样，生成的代码也必须在合并前经过审查。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认提示下方不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 对照议题和规划时做出的决定审查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查结果，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来没问题后，再启动应用并通过 Codespaces 转发端口打开网站，验证实际行为。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题和计划。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

本课使用不同的对话模式构建并审查了功能。你：

- 从 GitHub 议题启动了 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

下一步，使用[自定义指令][next-lesson]，确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
