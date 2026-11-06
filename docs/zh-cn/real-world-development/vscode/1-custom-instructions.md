---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文很重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就应让它能获取这些信息。[指令文件][instruction-files]可为代码生成提供项目指导。

在本课中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件提供给 Copilot。
- 在现有指令下生成首个筛选数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 重复同一提示，观察生成代码如何遵循新标准。
- 将指令更新和筛选数据切片提交到 `main`，供下一课使用。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 的结果具有概率性。本练习旨在观察更新指令前后行为变化的趋势，而不是要求输出与示例完全相同。

## 指令文件

### 场景

Tailspin Toys 有一套开发指南：数据层始终需要单元测试；界面采用现代暗色模式；代码应使用 TSDoc 文档注释；每个文件顶部应有说明文件用途的注释块。通过指令文件，让 Copilot 遵循这些实践。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解代码风格与需求。可以指定编码规范、库和注释要求，并为整个存储库或特定文件类型创建指令。

- `.github/copilot-instructions.md` 是发送给 Copilot 的存储库级指令，适用于该存储库中的每个请求，应包含与大多数请求相关的项目背景、技术栈和实践。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot 命令行界面 (CLI) 和 Copilot 云智能体在生成代码时会读取存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 还支持 AGENTS.md 和 CLAUDE.md 等其他标准。

### 管理指令文件的最佳实践

示例项目展示了一种可行做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、结构和全局编码标准。
- 用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，清楚表达要求，并提供正反示例。

指令文件没有唯一的正确写法；可以通过试验找到适合项目的方式。

> [!TIP]
> Copilot 项目通常需要一组完善的指令文件。项目中有适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的示例。
>
> Copilot 也能帮助生成指令文件。不同工作环境的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作环境的课程会说明相关入口。
>
> 可在 [awesome-copilot][awesome-copilot] 中查找指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先查看此存储库附带的指令文件，了解当前指令的效果。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。嵌套在 **Code standards** 下的 **GitHub Actions Workflows** 指南同样适用。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob（相对于存储库根目录）决定文件适用范围，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看此文件中有关单元测试的具体指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。它会链接其他指令文件（如 `unit-tests.instructions.md`）和项目文件，帮助拆分指令并引用示例。路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件头注释规则。

## 创建更改分支

先为更改创建分支。

1. 返回上一课使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。模式和模型因练习而异；每一步会说明相关选择。

先使用当前指令文件发送提示，观察结果；更新指令后再发送同一提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用更改，通常会同时修改辅助函数和测试。
6. 查看建议的辅助函数：它应为类型化函数，以 `db` 客户端为第一个参数，并返回类型化发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md`。
7. 注意建议的代码缺少 TSDoc 文档注释和文件头注释。

> [!CAUTION]
> Copilot 具有概率性，可能会在未明确要求时添加文档注释。如果发生这种情况也没关系；本练习的重点是观察更新指令后行为的一致性变化。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（约在第 27 行）。确认该部分尚未要求代码内文档，这也解释了为什么辅助函数没有注释。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件头注释和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级文件适合放置项目范围的标准；`*.instructions.md` 则适合语言、文件类型或任务专属指导。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意新文件应以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 确认函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新项目标准，并将其应用到后续练习会用到的真实代码中。

## 提交并推送分支

指令文件与其他项目文件一样由版本控制管理。提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
2. 确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令文件、辅助函数及相关更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办议题。让 Copilot 查找议题、创建 PR 并合并。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认 **Agent** 已选中。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和发行商辅助函数：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 开始查找议题并创建 PR 时，按提示审查命令，并在适当时选择 **Allow**。
5. PR 创建后，使用以下提示请求合并并更新本地 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. 按提示审查 GitHub 操作和 shell 命令，并在适当时选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取项目指令，并在 VS Code 中：

- 发送代码生成提示，观察现有指令下的结果。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送同一提示，观察建议的代码如何遵循新标准。
- 将指令和辅助函数提交到 `main`，供下一课继续使用。

接下来，让 Copilot 在[智能体模式][next-lesson]下使用这些指令，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的五个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
