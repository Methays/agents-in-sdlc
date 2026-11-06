---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Visual Studio Code (VS Code) 练习前，请先做好准备。你将创建 Tailspin Toys 存储库副本、启动用于练习的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 可正常使用。

## 设置实验存储库

从[模板存储库][template-repository]创建存储库副本。它将包含实验所需文件。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面选择 **Use this template**，再选择 **Create a new repository**。

    ![存储库页面上的 Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 举办的活动，请遵循导师的说明。否则，可在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![设置模板存储库选项的表单](../../../_images/ex0-repository-settings.png)

4. 记下新建的存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，GitHub 会自动为你创建一组议题。本研讨会将使用这些议题，无需自行创建。

## 创建 codespace

接下来使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是云端开发环境，可让你直接在浏览器中编写、运行和调试代码，并提供支持多种语言、扩展和工具的完整集成开发环境 (IDE)。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 创建新的 Codespace。

    ![创建 Codespace 的界面](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但比手动安装所有服务快得多。等待时可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会继续使用此 codespace。现在请将它保留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都会安装所需的先决条件。若要在本地运行，请在 VS Code 中打开克隆的存储库，并在收到提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

若要使用 Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时通常已安装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 帐户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 中输入 `Hello world` 并按 <kbd>Enter</kbd>，以启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示登录 GitHub 帐户。按提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证后，应能看到 Copilot Chat 窗口。

## 总结

你已创建实验存储库副本，并开始创建后续编写代码时要使用的 codespace。

## 下一步

接下来为 Copilot 提供项目规范。继续阅读[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
