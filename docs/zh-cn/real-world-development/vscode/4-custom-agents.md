---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在存储库 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专用指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定工作。团队可将专业经验编码为可复用智能体，例如遵循 Web 内容无障碍指南 (WCAG) 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents`。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并指导 AI 执行工作。简单来说，**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有独立的上下文窗口，可协调技能甚至其他智能体完成任务。本实验中的无障碍智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流或测试管理等技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。应在适用的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台。用户反馈指出，当前暗色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。你将使用自定义智能体生成此功能。

在本课中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已提供无障碍智能体。先查看它的内容，了解它如何指导 Copilot。

1. 返回 codespace，打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 查看后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的自定义智能体。选择它即可让会话使用该智能体的指令和工具。

1. 返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。
2. 如果智能体下拉列表中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

3. 选择聊天视图底部的智能体下拉列表。默认情况下，当前智能体为 **default**。
4. 从列表中选择 **Accessibility agent**。
5. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 这通常需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一课将审查并引导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot [自定义智能体][custom-agents]，以及如何通过专用角色编码团队专业知识和标准。本课中，你：

- 了解了自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用了自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并让同一会话添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

| [← 上一课：使用 Playwright MCP 测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
