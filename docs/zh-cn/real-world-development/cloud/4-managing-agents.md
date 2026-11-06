---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体执行了三项改善体验和添加功能的任务。云智能体虽然异步自主工作，但仍需监控任务进展。

可通过多种工具管理分配给云智能体的任务，例如 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看有未关闭拉取请求的智能体任务、查看它执行的操作，并在会话进行中提供指导。

在本课中，你将：

- 使用智能体页面监控云智能体任务。
- 在会话进行中提出额外功能要求。

## 场景

分配高对比度模式任务后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，可以趁此加入浅色模式。你将引导智能体完成这项扩展。

## 查看 Copilot 云智能体任务

查看已分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务，包括代码文档更新、游戏 API 生成以及网站高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 执行的操作，例如检出存储库、创建工作环境、设置 MCP 服务器和完成任务的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的进度和处理方式而变化。

4. 查看右侧的拉取请求 (PR) 面板，监控 PR 和更改的文件。

## 引导云智能体

查看当前任务后，让 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择负责添加高对比度模式的会话。会话标题取决于 Copilot 的命名和任务进度。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，其中突出显示了 Copilot 工作期间引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 收到提示，并将其纳入任务流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务，并在完成发送提示时正在执行的步骤后纳入新要求。此过程需要几分钟，可趁机回顾已学内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导云智能体任务的中心位置。你可以跟踪进行中和已完成的任务、审查工作，并调整会话范围。本课中，你：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 在会话进行中提出了额外功能要求。

Copilot 正在完成无障碍功能。接下来审查并[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
