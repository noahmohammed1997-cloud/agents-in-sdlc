---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明想要*什么*代码，还说明代码应当*如何*组织。筛选功能已经构建完成，现在来了解 Copilot 使用的指令、添加文档标准，并将它应用于代码。

本课将：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 的开发实践包括以下要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数接受可注入的 `db` 参数，也要记录。
- 可复用 Astro 组件应说明 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过指令文件，让 Copilot 根据这些实践完成工作。

## 指令文件

自定义指令可以提供上下文和偏好，帮助 Copilot 理解编码风格和需求。可以指定编码约定、库以及注释规范，并为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个存储库级文件，内容应与大多数请求相关。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因开发环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令

先查看入门项目中已有的指令文件，再进行修改。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段，了解它匹配哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目文件的链接。通过这些链接，可以拆分并复用较大的指令，也可以为 Copilot 提供示例。

## 更新指令以符合团队标准

现有文件还缺少一项要求。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 输入以下提示，让 Copilot 应用新指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。确认新增的 TSDoc 注释准确说明了代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何从指令文件获取上下文，并将新标准应用到筛选功能。具体来说，你：

- 了解了存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新了指令文件以确保遵循编码标准。
- 观察了指令文件对代码的影响。

下一步，自定义并运行可复用的 [`quality-checks` 技能][next-lesson]，确保 lint 和测试以一致方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
