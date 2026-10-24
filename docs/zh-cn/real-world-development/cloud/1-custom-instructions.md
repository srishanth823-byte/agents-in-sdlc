---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 应该了解某些背景信息，就要确保这些上下文可用。[说明文件][instruction-files]可以提供这类指导，帮助 Copilot 理解的不只是要做*什么*，还有应该*如何*完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 路径不同，本路径不会运行更新前后的提示进行对比。Copilot cloud agent 会在 GitHub issue 上异步工作，因此难以实时并排展示效果。稍后审查 cloud agent 创建的拉取请求时，可以看到说明文件对结果的影响。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并呈现现代风格。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定偏好的编码约定、库，以及希望添加的注释类型。既可以为整个存储库编写说明，也可以为特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：此单一说明文件会随该存储库中的**每个**请求发送给 Copilot。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 `AGENTS.md` 和 `CLAUDE.md`。[查看自定义说明支持参考][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊范围。示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并给出代码应该和不应该如何编写的示例。

说明文件没有唯一的正确写法，使用 AI 也一样。通过实验，可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。查看本项目时，可以看到针对多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应路径会在适用的课程中介绍。
>
> 需要模板或起点时，可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要留意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹浏览。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定适用文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意其中针对本项目单元测试的说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目文件的链接。将较大的说明拆分为更小、可复用的文件，可以让 Copilot 在生成代码时参考示例。这里的路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需编辑一次：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后的练习中，cloud agent 处理 issue 时会读取这项规则。

编辑前先创建工作分支（cloud agent 会读取 issue 目标分支中的说明，但仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在通过记录存储库编码标准来改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（约在第 27 行）。这里记录了项目编码标准，但还没有代码内文档规则，因此生成的 helper 不会包含文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以放在存储库级文件（`.github/copilot-instructions.md`）中，用于全局指导；也可以放在 `*.instructions.md` 文件中，用于特定语言、文件类型或任务。项目级文件适合存放刚添加的文档注释规则。

## 提交、推送并合并说明更新

Cloud agent 会读取 issue 目标分支中的说明。下一练习中，将 issue 分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后分配 issue 时，cloud agent 就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本工作坊，也可以跳过创建分支并直接提交。此处使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从本项目的说明文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。之后的练习中，可以在 cloud agent 生成的拉取请求中看到这项标准的效果。

接下来，将[第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
