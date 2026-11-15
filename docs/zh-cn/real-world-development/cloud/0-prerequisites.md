---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，先准备好所需环境。你将创建 Tailspin Toys 存储库副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查 cloud agent 的工作。

## 设置实验存储库

通过[模板存储库][template-repository]创建自己的存储库副本。该副本包含实验所需文件，后续练习都将在其中进行。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Tailspin Toys 模板存储库页面上的 Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在有权使用 GitHub Copilot 的组织中创建存储库。

   ![填写存储库模板设置的表单](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 codespace

接下来使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是云端开发环境，可在浏览器中编写、运行和调试代码，并提供支持多种编程语言、扩展和工具的完整集成开发环境（IDE）。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![存储库页面上的 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建 codespace。

   ![创建 codespace 的选项](../../../_images/ex0-create-codespace.png)

codespace 创建需要几分钟，但比手动安装所有服务快得多。等待期间，可以先探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会使用此 codespace。现在先不要关闭浏览器标签页。

> [!NOTE]
> 本工作坊可在 codespace 或本地[开发容器][dev-containers]中运行。两者都已安装所需依赖项。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

## 总结

你已创建实验存储库副本，并开始创建后续练习要使用的 codespace。

## 下一步

接下来添加 cloud agent 将遵循的自定义指令。继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [根据模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[next-lesson]: ../1-custom-instructions/
