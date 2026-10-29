---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

本课将介绍如何：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果 Playwright MCP 服务器尚未配置，则添加它；
- 让智能体操控浏览器并探索筛选功能。

## 场景

单元测试和端到端测试都很重要，但验证 UI 更新还需要实际与界面交互。希望 Copilot 能像用户一样使用网站，进一步自动化开发流程，并增强对功能行为的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器代表一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，方便发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用之前，审查其源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，使 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能行为正常。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的测试。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会自行判断。但指出预期使用的服务器有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 如何在浏览器中逐步执行操作，验证功能是否正常。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器并汇报结果。

## 总结和后续步骤

恭喜，你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中探索功能。本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 在需要时添加了 Playwright MCP 服务器；
- 让智能体操控浏览器并探索筛选功能。

接下来[创建 QA 自定义智能体][next-lesson]，结合技能和浏览器工具执行专门的质量保证工作。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
