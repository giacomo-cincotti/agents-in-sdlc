---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项改进用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在查看代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在面向团队启用审查前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体会在无人监督的情况下异步执行任务，因此设有安全限制：

- Copilot 对存储库只有读取权限，对代码分支只有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个 PR，即为代码添加文档。通过 GitHub.com 上常规的 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![GitHub 拉取请求页面中用于查看更改文件的 Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中应包含新创建的 TSDoc 文档注释和其他文档。实际更改会有所不同。检查 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自练习 2 开始时查看的自定义指令文件。
6. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![GitHub 拉取请求中的 Approve and run workflows 按钮，用于批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会显示在 PR 检查区域中。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向操作。你可以像提及团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法给出固定提示词，但可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用结构。

1. 添加评论请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上述建议，或提出其他希望添加到代码库的文档改进。
2. 选择 **View Session** 观察 Copilot 工作。它会启动新会话来应用更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Copilot 会话页面中用于返回拉取请求的 Back to pull request 按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。之后，将 PR 从草稿状态改为准备审查，再合并到主分支。

![GitHub 拉取请求中的控件，用于将草稿 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![GitHub 拉取请求中的 Approve and run workflows 按钮，用于批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会显示在 PR 检查区域。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。与之前一样，可以通过评论请求云智能体进一步更改，例如调整相关游戏的数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你尚未更新自定义指令）。在 **Conversation** 选项卡中添加评论后，Copilot 会读取请求并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 查看实现，尤其留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![GitHub 拉取请求中的 Approve and run workflows 按钮，用于批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会显示在 PR 检查区域。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**在 Codespace 中切换到该分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后设置仍然有效。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现问题或有改进建议，可以像处理前面的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体适合处理真实的待办事项，让你可以转而完成其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，以熟悉这一工作方式。例如：

- 在游戏详情页创建支持者意向表单；
- 为游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列中，你：

- **检查了存储库自带的自定义指令文件**，了解它们如何影响云智能体的输出；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代改进云智能体创建的 PR**，通过 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习路径。若想继续探索，其他学习路径会通过不同界面补充本系列内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
