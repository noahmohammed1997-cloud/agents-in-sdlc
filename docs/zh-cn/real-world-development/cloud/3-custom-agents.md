---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以向 Copilot 提供专用指令、最佳实践、编码模式和领域知识。团队可以将专业经验整理为可复用智能体，例如遵循《网页内容无障碍指南》（WCAG）的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents`。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体与[智能体技能][agent-skills-concept]都主要通过 Markdown 文件定义，并指导 AI 执行工作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍准则审查并更新网站，也可以调用拉取请求（PR）工作流或测试管理等技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。应在自己的开发环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将探索：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，文本与背景颜色对比度不足，导致部分用户难以阅读当前深色主题。设计团队因此要求添加可切换的高对比度模式。

无障碍改进十分重要，团队希望尽快实现此功能，因此将使用自定义智能体生成实现。

## 查看无障碍自定义智能体

项目已提供无障碍自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

1. 返回 codespace，打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续内容，查看无障碍网站代码生成时的核心职责、最佳实践、超文本标记语言（HTML）/层叠样式表（CSS）/JavaScript 示例，以及常见问题。

## 创建并分配议题

智能体控制中心是管理环境中所有智能体的中心位置。可以在其中分配任务、监控进度，并为正在运行的会话提供进一步指导。现在将高对比度模式任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot 议题分配界面中突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是面向特定任务和领域的专用助手。团队可将专业经验和标准固化为可复用智能体，引导 Copilot 更有效地完成工作。

你探索了以下内容：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，在智能体控制中心[监控并引导会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
