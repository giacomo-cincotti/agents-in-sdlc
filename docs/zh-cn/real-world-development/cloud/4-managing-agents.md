---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然异步自主工作，但监控任务仍然很重要。

有许多工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有有未关闭 PR 的智能体任务，检查已执行的操作，并引导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面并监控云智能体任务；
- 在会话运行过程中引导它添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式很合适。现在需要引导智能体同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表，其中应包括：
   - 更新代码文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
   - 从存储库检出代码；
   - 创建工作环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 同时留意右侧出现的 PR 面板，可在其中查看 PR 和更改的文件，以便继续监控。

## 引导云智能体

查看活动任务后，在智能体处理高对比度模式时，请求 Copilot 同时加入浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，已突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新请求纳入后续工作流程。

这同样需要几分钟，可以借此回顾已经学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以跟踪所有活动和已完成的任务、审查工作过程，并重定向正在运行的任务以调整范围或补充指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在运行的会话以请求添加功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
