---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在 Copilot 智能体模式下，它会自主探索代码库、提出更改、编辑文件并运行命令。VS Code 中的 Copilot Chat 会实时显示工具调用和文件编辑。你可以在编辑器中检查每个差异、接受或撤销单项更改，并通过后续提示引导对话，无需离开编辑器。

在本课中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体添加浅色模式切换。

## 场景

上一课中的无障碍自定义智能体正在实现高对比度模式。此时设计团队又提出浅色模式的需求。无需从头开始新会话，你可以引导当前会话扩展工作。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体探索的文件、运行的命令和提出的编辑。每项文件编辑都以内嵌差异显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回上一课使用的 Copilot Chat 视图。
2. 随着智能体工作，滚动聊天记录，查看工具调用、文件读取和编辑，以及请求运行的终端命令。
3. 出现文件差异时：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 选择聊天顶部的 **Files changed** 指示器，查看本次会话中智能体修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式，并查看汇总更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中要求添加浅色模式。由于会话上下文未变，智能体可以沿用先前的组件、样式和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查现有更改，并提出浅色模式的补充内容。
3. 像审查高对比度模式一样，逐项检查新更改，并选择 **Keep** 或 **Undo**。
4. 如果方向不符合预期，例如智能体试图以不希望的方式重构切换界面，可选择聊天输入框旁的 **Stop** 按钮停止任务，再发送澄清提示。

> [!TIP]
> 引导进行中的会话通常比重新开始更快。可以要求调整（例如使用 CSS 变量而不是重复选择器）、纠正方向（将浅色模式整合到现有设置面板，而不是添加第三个切换项），或扩展范围（为两个切换项添加键盘快捷键）。

## 审查工作集

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改的文件，包括 Astro 组件、样式和相关测试。
3. 打开部分文件并检查差异，确认实现体现了无障碍模式，例如可访问的富互联网应用程序 (ARIA) 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导了进行中的会话，审查每项文件编辑、接受或撤销更改，并在保留上下文的情况下通过后续提示扩展工作。本课中，你：

- 实时监控了智能体的工作。
- 接受或撤销了单项文件编辑。
- 使用后续提示在会话进行中引导智能体。

下一课将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
