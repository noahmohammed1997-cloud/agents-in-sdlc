---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

Copilot 在代理模式下会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以查看内联差异、接受或撤销单项更改，也可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体添加浅色模式切换。

## 场景

在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。设计团队在开发过程中又提出新需求：为觉得默认主题太暗的用户添加浅色模式。无需从头开始新对话，可以引导现有会话扩展功能。

## 监控 Copilot 进度

Copilot Chat 会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异显示，并附带 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录，查看工具调用、文件读取、文件编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，可选择 **Keep** 接受更改，或选择 **Undo** 撤销更改并让智能体再次尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看该会话中更改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式，并查看它对更改的总结。

## 引导智能体添加浅色模式

现在高对比度模式已实现，在同一对话中继续请求浅色模式。智能体会保留之前的上下文，包括已修改的组件、样式方法和 local storage 模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查已有更改，并提出浅色模式的实现。
3. 每次新编辑出现时，都像高对比度模式一样选择 **Keep** 或 **Undo** 审查。
4. 如果智能体的实现方向不符合预期（例如尝试重构切换 UI），选择聊天输入框旁的 **Stop** 按钮停止，再发送澄清提示。

> [!TIP]
> 在进行中的会话里追加提示通常比重新开始更快。可以要求调整现有方案，例如“简化 CSS，改用 CSS 变量，避免重复选择器”；也可以纠正范围，例如“不要添加第三个开关，将浅色模式整合到现有设置面板”；还可以扩展任务，例如为两个切换项添加键盘快捷键。

## 审查更改集

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改文件；应包括 Astro 组件、样式和相关测试。
3. 打开几个更改文件并检查差异。确认其中体现了无障碍模式的做法，例如 ARIA 属性、键盘导航、语义化 HTML 和通过 local storage 保存设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导进行中的会话，审查每项文件编辑、接受或拒绝更改，并在保留上下文的同时发送后续提示扩展工作。

本课中，你：

- 实时监控了智能体的工作。
- 接受或撤销了单项文件编辑。
- 使用后续提示引导了进行中的会话。

下一步，在[后续课程][next-lesson]中发布无障碍功能 PR，并审查本地自定义智能体在实验中完成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
