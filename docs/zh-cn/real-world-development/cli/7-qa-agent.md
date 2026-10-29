---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建结合议题需求、quality-checks 技能和 Playwright MCP 的 QA 自定义智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将介绍如何：

- 了解自定义智能体如何结合指令、技能和 MCP 工具；
- 创建并查看可复用的质量保证配置文件；
- 选择 QA 智能体，根据筛选议题审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告结果。它不会取代其他功能，也不需要启动新对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件，明确 QA 要执行的检查和必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 按预期记录了 QA 工作流程和边界，避免在只需要验证功能时生成不完整的配置或擅自修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必填项；`name` 是可选的，但可以让智能体显示清晰名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请先要求默认智能体修改。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选功能对话并选择 QA，以便使用已有的议题和计划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍可使用上述命令启用。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专业角色并审查其结果。本课中，你：

- 了解了自定义智能体如何结合指令、技能和 MCP 工具；
- 创建并检查了可复用的质量保证配置文件；
- 选择 QA 智能体，根据筛选议题审查结果。

现在，实现、技能更新、QA 配置文件、测试和验证报告均已准备好供审查。下一课将[汇总这些内容并创建功能 PR，使用 Agent Merge 合并][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
