---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，先准备好所需资源。你将创建 Tailspin Toys 存储库副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查云智能体的工作。

## 设置实验存储库

从[模板存储库][template-repository]创建副本。新存储库包含实验所需文件。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面选择 **Use this template**，再选择 **Create a new repository**。

    ![“Use this template”按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在你有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 codespace

接下来使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是云端开发环境，可在浏览器中编写、运行和调试代码，并提供支持多种编程语言、扩展和工具的完整集成开发环境 (IDE)。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但比手动安装所有服务快得多。等待时可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会继续使用此 codespace。现在请将其保留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都会安装所需的先决条件。若要在本地运行，请在 VS Code 中打开克隆的存储库，并在收到提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建实验存储库副本，并开始创建后续与 Copilot 云智能体协作时要使用的 codespace。

## 下一步

接下来添加云智能体要遵循的自定义指令。继续阅读[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
