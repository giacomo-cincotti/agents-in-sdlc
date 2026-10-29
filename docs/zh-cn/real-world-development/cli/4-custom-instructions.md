---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，就应确保 Copilot 能获得相关指导。[指令文件][instruction-files]不仅说明希望生成什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径指定的指令文件如何提供给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数也应说明）；
- 可复用 Astro 组件应说明 `Props` 接口，相关代码更改时应同步更新注释；
- 保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释偏好，为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 不同环境支持的其他指令格式可能不同。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目的自定义指令文件

起始项目已包含一组指令文件。修改前先查看现有内容，以便了解更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 浏览 `.github/instructions` 文件夹，其中包含适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并让 Copilot 参考示例。

## 更新指令文件以符合团队指导

现有文件已经提供了良好起点，但仍有缺项。修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应在第 35 行附近）。
2. 在最后一条项目符号下添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更改后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍打开，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查更改的 TypeScript 文件。查看新生成的 TSDoc 注释，确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能。本课中，你：

- 查看了存储库指令和按路径指定的指令如何传递给智能体；
- 更新了指令文件，以确保遵循编码标准；
- 观察了指令文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
