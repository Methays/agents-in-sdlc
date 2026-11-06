---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文很重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就应让它能获取这些信息。[指令文件][instruction-files]可提供指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本课中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面 (CLI) 工作环境不同，本练习不演示更新指令前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，难以实时并排展示效果；稍后审查它创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发指南：数据层始终需要单元测试；界面应采用暗色模式并具有现代风格；代码应包含 TSDoc 文档注释；每个文件顶部应有说明文件用途的注释块。使用指令文件可让 Copilot 按这些实践工作。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解代码风格与需求。可以指定编码规范、库和注释要求，并为整个存储库或特定文件类型创建指令。

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个存储库级指令，适用于该存储库的每个请求，应包含适用于大多数请求的项目背景、技术栈和实践。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会读取存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 还支持 AGENTS.md 和 CLAUDE.md 等其他标准。

### 管理指令文件的最佳实践

示例项目展示了一种可行做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、结构和全局编码标准。
- 用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，清楚表达要求，并提供正反示例。

指令文件没有唯一的正确写法；可以通过试验找到适合项目的方式。

> [!TIP]
> Copilot 项目通常需要一组完善的指令文件。项目中有适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的示例。
>
> Copilot 也能帮助生成指令文件。不同工作环境的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。若需要相关步骤，本工作环境的课程会说明。
>
> 可在 [awesome-copilot][awesome-copilot] 中查找指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先查看存储库附带的指令文件。打开 GitHub 网页界面或 codespace 中的文件：

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。嵌套在 **Code standards** 下的 **GitHub Actions Workflows** 指南也适用于 Copilot 的交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob（相对于存储库根目录）决定文件适用范围，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看此文件中有关单元测试的具体指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。它会链接其他指令文件（如 `unit-tests.instructions.md`）和项目文件，帮助拆分指令并引用示例。路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件头注释的规则。

## 添加存储库级标准

本课唯一的编辑任务是添加项目级规则，要求以 TSDoc 注释和文件头注释记录代码文档。云智能体将在后续练习中读取此规则。

先创建分支。云智能体会读取议题目标分支中的指令；提交更改可使其清楚地进入目标分支。

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分（约在第 27 行）。确认其中还没有代码内文档规则。
4. 在现有标准下方添加以下 Markdown：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级文件 `.github/copilot-instructions.md` 适合放置项目范围的标准；`*.instructions.md` 则适合语言、文件类型或任务专属指导。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一课中，Copilot 会从 `main` 分支创建分支，因此必须先把指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体便会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上操作，可以跳过分支步骤并直接提交。本练习保留分支流程，以模拟真实项目。

## 总结和后续步骤

你了解了 Copilot 如何读取项目指令，并向 `.github/copilot-instructions.md` 添加了存储库级标准。后续练习中，可以在云智能体创建的拉取请求中观察该标准的效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的五个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
