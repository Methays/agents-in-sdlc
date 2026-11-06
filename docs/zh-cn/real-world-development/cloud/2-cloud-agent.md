---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

许多组织都要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求，供开发者审查。这样可以委派繁琐任务，让开发者专注于更重要的工作。

本课将探索：

- 自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务。最初开发网站的外包团队没有留下足够的文档。团队希望先为应用中所有导出函数添加 TSDoc 文档注释。

设计团队还希望改善游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，帮助用户继续浏览。暂时不需要完善的实现，只需要能用于用户体验验收测试的版本。此需求目前被更高优先级的工作阻塞。

这些任务可能被不断推迟，适合交给云智能体异步处理。开发者可继续处理其他工作，之后再审查智能体的结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可像开发者一样在后台执行任务。你可以通过[分配 GitHub 议题][assign-issue]等方式启动会话。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看过程；方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确指令范围的重要性

GitHub Copilot 并非魔法，开发者仍需思考任务要求。即使看似简单的工作也可能包含复杂情况。请参考[云智能体任务范围最佳实践][cloud-agent-best-practices]，像与人工智能 (AI) 结对编程一样分阶段开展工作、学习并调整流程。生成式 AI 不会改变软件开发的基本原则。

## 查看此存储库中的自定义指令

之前的练习介绍了自定义指令及其作用。分配任务前，先只读查看存储库中的指令文件，稍后可据此判断它们对云智能体工作的影响。

在 GitHub 网页界面或 codespace 中查看：

- `.github/copilot-instructions.md`：检查 **Code standards**，尤其是 TypeScript 规范和 TSDoc 注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体。之后检查拉取请求中的 TSDoc 注释、TypeScript 规范和文件头注释；这些内容由上述指令提供。本系列后续练习会再次检查这些效果。

## 设置云智能体的开发环境

编写代码前通常需要配置环境并运行设置脚本。将任务分配给 Copilot 时也一样，云智能体会以类似软件工程师 (SWE) 的方式工作。

云智能体使用 [GitHub Actions][github-actions]环境。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在智能体开始工作前安装必要工具和依赖项。本实验已预先配置此工作流，以安装 Node.js、项目依赖项、端到端测试所需浏览器，以及迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流相似，但有几点需要注意：

- 它只有一个名为 `copilot-setup-steps` 的作业，会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流，以验证它是否成功，而不必等待 Copilot 启动。

## 添加文档

许多项目的文档已过时或缺失，这类技术债务会降低效率，也让维护代码和新成员上手更困难。Copilot 可协助生成文档，因此适合处理相关议题。稍后的练习会审查它生成的文档。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新建的拉取请求。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新页面。
12. 几分钟后，Copilot 应会创建待办列表。它需要先运行前述工作流来创建环境、分析项目并确定处理方案，因此列表可能稍后才会出现。
13. 查看 Copilot 准备完成的任务。
14. 在 PR 时间线中向下滚动，确认 Copilot 已开始处理议题。
15. 选择 **View session**。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，查看 Copilot 如何探索代码、了解当前状态、制定方案并编写代码。

任务可能需要几分钟。云智能体的优势之一是能异步处理任务。现在将另一项工作分配给云智能体，然后你可以转而为应用添加功能。

## 在游戏详情页添加相关游戏

云智能体可以在后台处理任务，让你同时开展其他工作。现在将设计团队提出的相关游戏功能分配给它。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，再选择空白议题模板。
4. 将 **Title** 设为 `Show related games on the game details page`。
5. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot**，然后选择 **Assign**。

稍后会看到议题的第一条评论上出现 👀，表示 Copilot 已开始处理。云智能体会异步工作，你无需持续监控；完成后再审查即可。现在可以转而学习如何创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题交给它异步处理技术债务、开发功能，或协助框架迁移。本课中，你：

- 自定义了代码生成环境。
- 了解了如何安全地执行操作。
- 了解了清楚界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，接下来学习创建和使用自定义智能体。[云智能体也能使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并可读取之前练习中探索过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
