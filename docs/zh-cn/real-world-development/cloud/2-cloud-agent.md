---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建草稿 PR，供开发人员审查。这有助于将繁琐任务交给智能体处理，加快开发进程，让开发人员专注于更重要的工作。

本练习将使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保操作安全；
- 了解明确界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初受雇开发网站的承包商没有为项目编写文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页面显示同类别的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要可用于 UX 验收测试的功能。此功能目前会阻塞工作，但优先级低于其他议题。

这些任务容易被搁置，适合交给 Copilot 云智能体异步处理。开发人员可以先处理其他工作，之后再回来审查其成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看其过程。方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法——使用它仍然需要思考。看似简单的操作也可能包含复杂环节，因此要[谨慎界定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成相关练习，就已经看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读浏览存储库现有的指令文件，稍后便能辨认它们的影响。

在 GitHub 网页界面或已启动的 codespace 中查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，观察拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的练习还会再次检查这些内容。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境和初始化脚本，以确保项目状态正常。将任务分配给 Copilot 时也一样，它的工作方式类似软件工程师。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境，在智能体开始工作前安装必要的开发工具和依赖项。本实验已预先配置此工作流，以确保流程顺利并提供学习机会。它会准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予这些步骤所需的最低权限。Copilot 会获得用于自身操作的独立令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在这些步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已采用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，使构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

很多项目的文档已过时或完全缺失，这类技术债务往往无人处理，降低效率，也让维护代码库或引入新开发人员更加困难。Copilot 擅长生成文档，因此很适合将这项工作分配给云智能体。它会在后台生成文档，稍后的练习再审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新页面。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它需要创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 查看待办事项及 Copilot 准备完成的任务。
14. 向下查看 PR 时间线，确认 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解现状、暂停思考并制定方案，然后编写代码。

这可能需要几分钟。云智能体的主要优势之一是异步执行任务，让我们可以专注于其他工作。接下来利用这一点：再分配一项任务给云智能体，同时开始编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优势是可以并行分配工作，让你处理一组任务，而它处理另一组。为设计团队添加相关游戏可能不会耗费太久，但这段时间也可以用于其他任务。现在将其分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择空白议题模板 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需一直监控，完成后再审查即可。接下来创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给它，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体继续在后台工作。接下来创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可读取此前练习介绍的自定义指令。

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
