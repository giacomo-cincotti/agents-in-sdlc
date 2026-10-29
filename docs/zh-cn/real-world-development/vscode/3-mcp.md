---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。无需亲自逐项操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

智能体模式可以访问编辑器之外的工具，从而发挥更大作用。Copilot 通过模型上下文协议 (MCP) 实现这一点；MCP 是智能体与外部工具和服务通信的标准方式。

![示意图展示智能体模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，智能体可以实时访问最新信息（使用资源），也可以代表你执行操作（使用工具）。

MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信，例如连接已有 API 或 npm 包等本地工具。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 智能体能够创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体能够访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可用于访问不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，方便发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用之前，仔细审查其源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

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

项目模板中已包含 `github` 配置，因此 Copilot 能在前一练习中读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 配置：

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

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标记让 Playwright 在后台运行浏览器，不显示窗口。这适用于没有桌面环境的 codespace。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供真实浏览器。智能体可以打开页面、导航、应用筛选器并读取结果，然后向你总结观察结果，无需离开对话即可快速验证功能行为。

Playwright MCP 服务器实际使用页面的[无障碍树][playwright-mcp-server]而非屏幕截图。智能体会像辅助技术一样，根据结构化且带标签的元素（按钮、链接、列表项）进行推理，因此简单的功能检查也能粗略验证无障碍体验。

服务器已连接且应用正在运行时，发送以下提示，让 Copilot 检查刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐项执行测试并汇报结果。将报告与议题验收标准进行对照；如果有异常，先提出后续问题或要求它修复代码，再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重启。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR) 供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板显示所做更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让正在使用的 GitHub Copilot 帮你创建 PR，并关联到相关议题。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示你登录 GitHub，请按提示授权。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在发布前通过浏览器手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
