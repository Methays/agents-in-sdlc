---
title: "练习 3 - 使用 Playwright 模型上下文协议 (MCP) 测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认功能在浏览器中正常运行。本课将连接 **Playwright 模型上下文协议 (MCP) 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建拉取请求。

在本课中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

智能体模式可连接编辑器以外的工具。[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)是一种让 Copilot 与外部工具和服务通信的标准。

![智能体模式与上下文、大型语言模型和工具的交互示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

MCP 让人工智能 (AI) 智能体能够与外部工具和服务实时通信，获取最新信息，并代表你执行操作。MCP 服务器充当智能体与外部工具（例如现有应用程序编程接口 (API) 或本地 npm 包）之间的桥梁，每个服务器都会提供一组工具和资源。

常见服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的 API，可创建存储库、更新内容、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，可导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，便于发现服务器并参与生态建设。

> [!CAUTION]
> 请像对待其他项目依赖项一样审慎对待 MCP 服务器。使用前应审查源代码、验证发布者并评估安全影响。只使用信任的服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 确认其中已配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板预先配置了 `github` 项，因此上一课中 Copilot 才能读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次使用工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。Copilot 可借助 [Playwright MCP 服务器][playwright-mcp-server]操作浏览器，测试刚构建的功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 项，使文件如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数让 Playwright 在后台运行浏览器；codespace 没有可显示浏览器的桌面，因此需要此参数。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，通常已安装浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动它并询问你是否信任。

1. 在 `.vscode/mcp.json` 中找到 `playwright` 项上方的 **Start** 操作，选择它以立即启动服务器并确认配置有效。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要服务器工具时启动它并询问你是否信任。

## 测试筛选功能

Playwright MCP 服务器让 Copilot 操作真实浏览器：打开页面、应用筛选并读取结果，然后汇总观察结果。它能快速验证功能行为，无需离开对话。

Playwright MCP 服务器依据页面的[无障碍树][playwright-mcp-server]而非截图工作。智能体会分析带标签的结构化元素（按钮、链接和列表项），与辅助技术类似，因此功能检查也能顺便验证基本无障碍状况。

确认服务器已连接且应用正在运行后，使用以下提示：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器、执行测试并汇报结果。对照议题的验收标准审查报告；如发现问题，可继续提问或让 Copilot 修复后再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重启。首次使用 Playwright MCP 时可能需要下载浏览器；若报告缺少浏览器，请让 Copilot 运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## 发布分支并创建拉取请求

功能已验证，可以创建拉取请求 (PR)，供团队审查。先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 的更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可通过 github.com 或 GitHub 命令行界面创建 PR。本课让 Copilot 查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据提示选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你进行 GitHub 身份验证。
5. 确认 Copilot 搜索议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时不要合并。

## 总结和后续步骤

本课中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布分支并创建筛选功能 PR。

你使用 MCP 服务器测试了功能；MCP 也是扩展 Copilot 的方式之一。接下来，创建一个[自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
