---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件，为 Copilot 指定专用指令、最佳实践、编码模式和领域知识。例如，团队可以创建遵循 Web 内容无障碍指南 (WCAG) 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents`。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并指导 AI 执行工作。简单来说，**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有独立的上下文窗口，可协调技能甚至其他智能体完成任务。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流或测试管理等技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。与其他 AI 用法一样，应在目标环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台。用户反馈指出，当前暗色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。由于无障碍访问至关重要，我们将使用自定义智能体快速生成此功能。

你将探索：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 检查无障碍自定义智能体

项目已提供无障碍自定义智能体。先查看文件，了解它如何指导 Copilot。

1. 返回 codespace，打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 继续查看后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以向 Copilot 云智能体分配任务、监控任务并提供额外指导。现在将创建高对比度模式的任务交给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue**。
3. 选择 **Blank issue**。
4. 将 **Title** 设为 `Add high contrast mode to website`。
5. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot**。
8. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面，其中突出显示了自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

9. 选择 **Assign**。
10. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 的[自定义智能体][custom-agents]，以及如何用专用角色编码团队专业知识和标准。本课中，你：

- 了解了自定义智能体的定义方式。
- 将任务分配给了自定义智能体。

Copilot 正在实现高对比度模式。接下来，前往任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中准备使用自定义智能体][org-custom-agents]
- [在企业中准备使用自定义智能体][enterprise-custom-agents]

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
