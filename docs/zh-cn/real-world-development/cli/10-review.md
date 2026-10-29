---
title: "课程 10 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。本系列中，你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查；
- 使用自定义指令引导智能体，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议 (MCP) 服务器，通过真实浏览器检查筛选行为；
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据；
- 审查完整的筛选功能，并授权 Agent Merge 合并筛选 PR；
- 探索管理上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本研讨会包含两个 PR 里程碑，各自基于更新后的 `main` 分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选与质量工作流：**实现筛选功能、更新指令并应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相应测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为了简化研讨会流程，将相关工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能以新格式运行单元测试、lint 和类型检查并报告结果。QA 智能体在 PR 创建前汇总这些结果，并审查需求和测试覆盖范围。

添加的测试应弥补真实缺口；如果 QA 检查后确认无需添加测试，也可能是合理结果。授权合并前请审查代码和证据；代码更改后，应重新验证受影响的内容。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置 MCP 服务器。可以在多个会话中复用这些设置，并根据团队需求调整。指令规定标准，技能描述可重复任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改过程，使用 **Autopilot** 处理定义清楚的任务。常规编辑可选择较快的模型，复杂任务则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响输出。

## 更多可探索内容

核心工作流已完成，还可以了解其他 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获得另一种观点；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提高任何工具使用能力的最佳方法就是持续使用它。可以在生产代码、个人项目或一直想构建的小应用中使用 Copilot。与团队分享经验，也向团队成员学习。最后，继续浏览相关文档。

若要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可查看 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
