---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文很重要。若任务有特定做法，应让 Copilot 能获取相应指导。[指令文件][instruction-files]不仅能说明要编写什么代码，还能说明代码应如何组织。完成筛选功能后，现在可以了解 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

在本课中，你将：

- 了解存储库指令和按路径应用的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发指南：注释应说明意图和不明显的决策，而不是复述代码；`db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数也应说明）；可复用 Astro 组件应说明其 `Props` 契约，代码更改时应同步更新注释；现有格式和 lint 指南也应保留。使用指令文件可为 Copilot 提供这些要求。

## 指令文件

自定义指令向 Copilot 提供上下文和偏好，帮助它理解代码风格与需求。可以指定编码规范、库以及注释要求，并为整个存储库或特定文件类型创建指令。

- `.github/copilot-instructions.md` 是发送给 Copilot 的存储库级指令，适用于该存储库中的每个请求，应包含对大多数请求都相关的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

先查看入门项目中已有的指令文件，了解当前指导方式。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 阅读项目简介和编码指导。此文件适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段；它定义指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目文件的链接。通过这些链接，可以将较大的指令拆分为可复用的小文件，并提供示例。

## 更新指令以符合团队标准

现有文件只是一个起点。接下来修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot 命令行界面 (CLI) 会在对话启动时加载存储库指令。保存指令后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍打开，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。确认新生成的 TSDoc 注释准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取指令文件，并将新标准应用到功能中。本课中，你：

- 探索了存储库指令和按路径应用的指令文件如何提供给智能体。
- 更新了指令文件，要求遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，定制并运行可复用的 [quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件等资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
