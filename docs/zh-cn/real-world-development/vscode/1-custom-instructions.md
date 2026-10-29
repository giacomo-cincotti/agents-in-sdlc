---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导代码生成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在现有指令下生成筛选功能的第一个数据切片（发行商 helper）；
- 在 `.github/copilot-instructions.md` 中添加适用于整个存储库的新标准；
- 再次发送相同提示，观察生成的代码如何采用新标准；
- 将指令更新和筛选功能切片提交到 `main`，使 Copilot 能在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有编码标准。Copilot 的输出具有概率性。本练习的重点是观察更新指令前后行为的变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试；
- UI 应采用暗色模式并具有现代感；
- 应通过 TSDoc 文档注释为代码添加文档；
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

- 在 `copilot-instructions.md` 中集中提供项目级指导，例如项目说明、结构和全局编码标准；
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导；
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

## 创建更改分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 在 Codespaces 编辑器（而非终端）中，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所需的模式和模型可能不同，相关步骤会在适用时说明。

先在当前指令文件下发送提示，观察自定义指令的效果。稍后更新指令后，再发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取本次需要的上下文。
2. 打开 `src/lib/publishers.ts`，告诉 Copilot helper 应位于何处。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，使 Copilot 可以编辑文件。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 留意生成的 helper 是类型化函数：第一个参数为 `db` 客户端，返回类型化发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有指令，它也可能添加文档注释。即使如此也没有问题；重点是观察更新指令后的行为是否更加一致。

## 添加新的存储库标准

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

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 再次发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在文件开头应出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在应包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码，下一练习会在此基础上继续。

## 提交指令并推送分支

指令文件和存储库中的其他资产一样，需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中确认指令和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新及 Copilot 为 helper 基础工作所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已在工作区中配置 GitHub 工具，因此无需设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。稍后的练习将介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 请求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新建的发行商 helper 基础工作：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 请求 **Allow** 权限以代表你执行 GitHub 操作，请先检查操作，再按需批准。
6. PR 创建后，要求 Copilot 合并 PR 并将分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 请求 **Allow** 权限以执行 GitHub 操作和 shell 命令，请先检查操作，再按需批准。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从指令文件获取上下文，并通过 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据*现有*指令生成内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次发送相同提示，观察代码如何采用新标准；
- 将指令更新和 helper 基础工作提交到 `main`，以便下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
