---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实现方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才我们为项目添加了一项小功能；较大的更改则需要更完整的流程。GitHub Copilot 命令行界面 (CLI) 可以融入组织现有的开发流程，帮助团队以正确方式构建正确功能。本课开始一组实践课程：从议题出发生成新功能，验证代码和行为，最后成功合并。

> [!NOTE]
> 后续功能开发会继续使用同一对话和分支。实际工作中，不同文件类型的更改通常会放在不同分支或拉取请求中；本研讨会为聚焦核心概念而简化了流程。

本课将带你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在 Codespaces 转发端口打开的浏览器中手动验证功能。

继续开发时，你还将更新存储库指令、自定义现有 `quality-checks` 技能、添加模型上下文协议 (MCP) 验证、创建质量保证 (QA) 智能体，并创建功能拉取请求。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办议题描述了该功能，但筛选条件如何组合等细节需要在编码前确定。你将使用 Plan 模式商定这些决定，再授权 Autopilot 在明确范围内实现功能。

## 背景

引入人工智能 (AI) 编码智能体不会改变软件开发的基本流程。开发者通常会：

1. 从描述需求的议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查和持续集成流程成功后合并更改。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程相似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过验证。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直到认为任务完成。

先使用 Plan 模式审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分的拉取请求已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 连按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，然后返回 **Session** 选项卡。

提示现在会以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求带入上下文。

## 规划筛选功能

规划能让你在交由 Copilot 执行前，先确定功能实现方式。对于复杂任务，花时间规划通常很有帮助。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定实现方案。过程中，它可能会询问筛选行为的细节；请根据自己的偏好回答，没有唯一正确答案。

3. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 浏览计划，并要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现。Autopilot 会持续处理任务，直到认为工作完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与议题及规划时作出的决定进行比较。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查结果；如有失败，请让 Copilot 修复。

## 体验新功能

代码看起来没问题后，启动应用并通过 Codespaces 转发端口打开网站，确认功能确实运行。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及计划中商定的组合条件。
4. 确认重置行为和无结果时的行为符合议题及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在 Codespaces 转发端口打开的浏览器中手动验证功能。

接下来，使用[自定义指令][next-lesson]进一步了解如何按照已记录的实践生成代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
