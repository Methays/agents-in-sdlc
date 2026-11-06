---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

本系列练习中，你让 GitHub Copilot 处理了多个任务：为代码添加文档、实现设计团队要继续迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，改进结果。

## 场景

生成式人工智能 (AI) 不会改变软件设计和 DevOps 的基本原则。代码仍需审查并遵循常规 DevOps 流程。接下来审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，然后再邀请团队成员参与审查。

## 安全性和 GitHub Copilot 云智能体

云智能体异步且自主地执行任务，因此受到以下安全限制：

- Copilot 对存储库只有读取权限，对工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准才能运行。
- 默认情况下，[外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先查看 Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。

> [!NOTE]
> PR 中可能会出现 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器）的访问受限。你可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 会显示 **[WIP]**。请等待任务完成；这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能不同；检查 TSDoc 注释、TypeScript 规范和文件头注释。这些要求来自练习 2 中查看的自定义指令文件。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域。等待单个 Astro 应用的项目检查通过；这可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以像在团队成员的 PR 中一样，在评论或代码行内评论中提及 Copilot，让它启动新会话处理反馈。由于生成结果并非确定性结果，无法规定唯一的提示；可以要求 Copilot：

- 在代码文件顶部添加简要说明文件用途的注释块。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 在 PR 中添加评论并提及 **@copilot**，提出一项文档改进要求，也可使用上述示例。
2. 选择 **View Session**，观察 Copilot 如何启动新会话进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新提交。
5. 选择 **Files changed** 审查更新。

你可以继续迭代，直到满意为止。之后可将 PR 从草稿改为准备审查状态，再合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在检查 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库，选择 **Pull Requests** 选项卡。
2. 打开标题类似 **Show related games on the game details page** 的 PR。
3. 选择 **Files changed** 审查生成的代码。
4. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
5. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

6. 等待 PR 检查区域中的单个 Astro 应用检查通过。
7. **可选：**在 Codespace 中手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名（例如 `copilot/fix-8`），然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。你可以继续通过 PR 评论要求它调整显示的相关游戏数量，或补充文件头注释和 TSDoc 文档注释。注意，此任务分配时尚未更新自定义指令。Copilot 会读取 **Conversation** 选项卡中的新评论并启动新会话。

## 审查无障碍功能

最后审查无障碍自定义智能体实现的功能。PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的存储库，选择 **Pull Requests** 选项卡。
2. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在工作，PR 会显示 **[WIP]**。等待任务完成，这可能需要几分钟。

3. 选择 **Files changed** 审查生成的代码，重点检查：
   - 用于切换模式的界面组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性，如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。
4. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
5. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

6. 等待 PR 检查区域中的项目检查通过。
7. **可选：**在 Codespace 中检出 Copilot 创建的分支，手动测试无障碍功能：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

无障碍自定义智能体帮助 Copilot 按无障碍最佳实践实现功能。如有问题或改进建议，可像前面一样在评论中提及 **@copilot**。

## 可选练习 - 继续委派任务

云智能体适合处理待办事项，让你专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页添加支持者意向表单。
- 为游戏列表添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境。在这些练习中，你：

- 查看了存储库中的自定义指令文件，以了解它们对云智能体结果的影响。
- 将议题分配给 Copilot 云智能体，并观察它设置环境、规划和异步执行任务。
- 创建并使用无障碍自定义智能体，添加高对比度和浅色模式切换。
- 使用 Copilot 智能体页面作为任务控制中心，监控并引导无障碍会话。
- 审查并迭代云智能体的拉取请求，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体工作环境已完成。其他工作环境可帮助你从不同角度继续探索：

- **[VS Code 工作环境](../../vscode/)**：直接在集成开发环境 (IDE) 中探索 Copilot Chat 智能体模式和模型上下文协议 (MCP) 集成。
- **[命令行界面 (CLI) 工作环境](../../cli/)**：在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体衔接。

也可以在自己的存储库中尝试：将重构或测试覆盖率议题分配给云智能体、为其他领域创建自定义智能体，或为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
