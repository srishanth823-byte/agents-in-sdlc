---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 应该了解某些背景信息，就要确保这些上下文可用。[说明文件][instruction-files]是向 Copilot 提供代码生成指导的主要方式。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在当前说明的指导下，生成过滤功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准；
- 再次运行相同提示，观察生成代码如何采用新标准；
- 将说明更新和过滤功能基础代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成代码可能不完全符合设定的标准。Copilot 具有概率性。本练习的目标是观察更新说明后行为变化的*趋势*，而不是让输出逐字符一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试；
- UI 应使用深色模式，并呈现现代风格；
- 通过 TSDoc 文档注释为代码添加文档；
- 每个文件开头都应有一段注释，说明文件用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释类型。说明可以适用于整个存储库，也可以针对特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：此单一说明文件会随该存储库中的**每个**请求发送给 Copilot，应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持 `AGENTS.md` 和 `CLAUDE.md` 等其他标准的说明文件。[查看自定义说明支持参考][custom-instructions-support]。

### 管理说明文件的最佳实践

示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码标准；
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明；
- 使用自然语言并保持清晰，同时给出代码应该和不应该如何编写的示例。

说明文件没有唯一正确的写法，使用 AI 也一样。通过试验可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。本项目中有针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的文件。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。
>
> 需要模板或起点时，可以查看 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

先阅读此存储库附带的说明文件，包括核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。它们适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段会设置相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看其中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这样可以将较大的说明拆分为可复用文件，并给 Copilot 提供代码示例。路径相对于说明文件本身。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 注释和文件注释头规则。

## 为更改创建分支

先为更改创建分支。

1. 返回上一个练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。不同练习的模式和模型要求可能不同，具体步骤会逐一说明。

为了观察自定义说明的影响，先在当前说明生效时发送提示。之后更新说明并再次发送相同提示，比较差异。

1. 关闭之前练习留下的编辑器标签页，以便 Copilot 只获取当前需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，允许 Copilot 编辑文件。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常包括 helper 及其测试。
6. 留意生成的 helper 是带类型的函数，首个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**没有** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使未明确要求，也可能添加文档注释。如果发生这种情况也没关系；本练习要观察的是更新说明后行为的一致性变化。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（约在第 27 行）。该部分已有编码标准，但没有代码内文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以放在存储库级文件 `.github/copilot-instructions.md` 中，用于全局指导，也可以放在 `*.instructions.md` 文件中，用于特定语言、文件类型或任务。项目级文件适合存放文档注释规则。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话上下文。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 再次发送相同提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成的函数现在也会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到下一练习将继续使用的代码中。

## 提交说明并推送分支

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新及 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并将其关联到积压列表中的文档标准 issue。Copilot 可以使用已连接到项目的 GitHub 工具来完成此操作。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub，按提示完成身份验证即可。本工作坊稍后会介绍此连接如何通过 Model Context Protocol 工作。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中了 **Agent**。
3. 请 Copilot 查找更新说明文件的 issue，并从当前分支创建 PR，说明其中包含说明更新和 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找 issue 并创建 PR。
5. 当 Copilot 请求允许它代表你执行 GitHub 操作时，先审查命令，再按需选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求允许执行 GitHub 操作和 shell 命令时，先审查命令，再按需选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有说明的影响；
- 向 `.github/copilot-instructions.md` 添加存储库级标准；
- 再次发送提示，观察代码如何遵循新标准；
- 将说明更新和 helper 基础提交到 `main`，供下一练习使用。

接下来使用 [agent mode][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
