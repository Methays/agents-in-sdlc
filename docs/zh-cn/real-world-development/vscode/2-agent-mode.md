---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件和测试。开发者需要找到相关文件、修改代码、运行测试、调试并补上遗漏更改。Copilot 智能体模式可在 IDE 中更自主地工作：先探索项目结构，再更新相关文件、运行任务并修复发现的问题。现在使用它为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本课中，你将了解 Copilot 智能体模式如何：

- 探索项目、找到相关文件并协同修改。
- 跨用户界面和数据层实现新功能。
- 在合并前审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断增长，需要允许用户按类别和发行商筛选。上一课已添加发行商辅助函数；现在将使用 Copilot 智能体模式完成剩余的数据层、界面和测试工作。

## 运行 Tailspin Toys 网站

修改前先了解网站当前功能。该网站是面向开发者的桌游众筹平台，可列出游戏并显示详情。它是一个 Astro 应用，在构建时渲染为静态 HTML。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准文档对象模型 (DOM) API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回上一课结束后所在的 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321`，选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，网站正在该环境中运行。

### 浏览网站

网站主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后可查看标题、说明、发行商和类别等详细信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。每一步会说明所需的模式和模型。

项目已能正常运行，现在开始增强功能。模板存储库会自动创建一组议题；让 GitHub Copilot 显示待办事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可自主规划和实现跨文件更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，希望得到相对一致的建议；由于大型语言模型 (LLM) 具有概率性，建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于大型语言模型 (LLM) 具有概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看 GitHub 议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 会按它认为对用户最有用的程度排列议题。

## 查看指令文件

生成代码前，先查看用于提供上下文的指令文件。[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括应用架构、组件设计、可测试性和无障碍原则，以及指向 Astro、Tailwind CSS 和 Drizzle 指令文件的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于当前指令文件，可用于复用内容、拆分大型指令，以及提供代码示例。

## 实现筛选功能

完成筛选至少需要三项应用更改：在 `src/lib/` 中添加或完善筛选逻辑、添加或更新筛选测试，以及更新游戏列表页以显示筛选界面。合并前还必须运行并通过测试。Copilot 智能体模式可以处理这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 在 Chat 视图的智能体下拉列表中确认 **Agent** 已选中。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题提示 Copilot：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、定位数据层辅助函数、界面和测试，并修改文件、运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**；工作完成后，你可以对每项更改选择保留或撤销，无需在执行中途操作。

7. Copilot 提示时选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中的对话框，询问是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停或遇到测试失败。Copilot 会在生成代码和运行测试之间迭代，直到不再发现错误。

   ![Copilot 智能体模式的完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能 (AI) 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能和代码均符合预期后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。本课中，你：

- 使用智能体模式跨用户界面和数据层实现新功能。
- 让 Copilot 探索项目、找到相关文件并协同修改。
- 在合并前审查 Copilot 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索：实现分页

游戏列表增长后可能需要分页。运用本课所学，让 Copilot 为网站添加分页。考虑是否允许用户选择每页项目数、提示中需要哪些实现指导，以及如何通过迭代和补充上下文完善结果。这是使用 Copilot 的正常工作方式。

## 资源

- [Copilot ask、edit 和 agent 模式：功能与适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
