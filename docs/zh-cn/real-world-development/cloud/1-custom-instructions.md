---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。[指令文件][instruction-files]可提供此类指导，帮助 Copilot 理解你希望它做什么以及如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 系列不同，本练习不会比较提示前后的结果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查云智能体创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释偏好，为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围，但示例项目展示了常见做法：

- 在 `copilot-instructions.md` 中集中提供项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

指令文件没有唯一的写法，AI 的用法也没有标准答案。可以通过试验找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。浏览本项目中的文件时，你会看到针对[更新 UI][ui-instructions]、[Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍具体方法。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义指令文件

花些时间查看存储库自带的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹浏览，其中包含适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，决定指令适用的文件。此处匹配 TypeScript 测试文件，例如 `**/*.test.ts`。
5. 查看为本项目编写单元测试的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部，其中链接到其他指令文件（如 `unit-tests.instructions.md`）和项目文件。这种方式可将较大的指令集拆分成可复用的小文件，并让 Copilot 参考示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

本练习唯一需要编辑的内容，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后将议题分配给云智能体时，它会采用这条规则。

编辑前先创建分支（云智能体会读取议题目标分支中的指令，但仍建议单独提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准添加文档要求，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。它记录了项目编码标准，但尚未规定代码文档要求，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 指导特定语言、文件类型或任务。你刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将指令更新合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。之后云智能体处理下一练习中分配的议题时，就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上操作，可以跳过创建分支并直接提交。本练习采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，云智能体创建的拉取请求将展示这条标准的作用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
