---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 能融入组织现有的工作方式，帮助我们以正确方式构建正确功能。本课是多个课程中的第一课，将介绍典型的智能体开发流程：根据议题开发新功能、验证代码、检查行为，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常不同类型的文件可能会使用不同分支或 PR，但本研讨会会采用简化方式，以便专注于核心概念。

本课将介绍如何：

- 根据 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将先用 Plan 模式确定决策，再授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则，甚至会让这些原则更加重要。大多数开发人员遵循类似的流程：

1. 从描述需求的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查完成且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，期间不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先使用 Plan 模式并审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
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

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示全部议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

现在提示词开头会出现 `#7` 或类似编号。`#` 可将 GitHub 议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以先确定功能实现方式或任务处理方案，再交给 Copilot 执行。对于复杂任务，花些时间规划总是值得的。现在切换到计划模式并让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定最佳方案，以此制定计划。

3. Copilot 可能会询问筛选功能的行为细节。根据偏好回答，没有标准答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完成后，就可以开始实现。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 迭代处理问题，直至认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

生成代码后，和其他代码一样，需要先审查再合并。接下来审查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题及规划时作出的决定进行比较。
4. 审查完毕后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复失败项。

## 探索新功能

代码看起来没问题，但实际运行情况如何？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题要求及你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查功能。本课中，你：

- 根据 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查了代码；
- 在转发的浏览器中手动验证功能。

接下来[使用自定义指令][next-lesson]，深入了解代码生成方式并确保其符合文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
