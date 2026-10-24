---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、进行修改、运行测试、调试，再确认是否遗漏文件。**Copilot Agent Mode** 可以帮助处理这些工作。

Agent Mode 能在 IDE 中自主工作，先探索项目结构，确定需要修改的内容，执行测试等任务，并自动修复发现的问题。现在使用它为网站添加新功能。

> [!NOTE]
> Agent mode 和 cloud agent 虽然名称相似，但提供不同体验。Agent mode 在 IDE 中执行工作，便于快速反馈和互动；cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍 Copilot Agent Mode 如何：

- 探索项目、识别相关文件并协调多文件更改；
- 跨 UI 和数据层实现功能；
- 在合并前审查它生成的更改和测试。

## 场景

随着游戏列表增长，你希望访客可以按类别和发行商筛选。上一练习已添加 publishers helper；现在使用 Copilot Agent Mode 完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改代码前，先了解网站当前功能。Tailspin Toys 是一个带有开发者主题的桌游众筹平台，可列出游戏并显示详情。整个网站是单个 Astro 应用，页面在构建时以静态 HTML 渲染。页面 frontmatter 通过 Drizzle ORM 直接查询本地 SQLite 数据库，不存在单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能使用作用域有限的 Astro `<script>` 和标准 DOM API。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回 codespace。上一练习结束后应位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建过滤功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后会显示 URL，例如：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示要打开 `http://localhost:4321` 的对话框，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择终端中的 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道。

### 浏览网站

网站运行后，可以探索以下功能：

- **首页**：显示桌游列表、标题、图像和说明；
- **游戏详情页**：选择游戏后，可以查看标题、说明、发行商和类别等信息。

## 使用 Copilot 探索积压 issue

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。各练习的模式和模型要求可能不同，具体步骤会逐一说明。

项目已有可用的初始实现，现在要添加新功能。模板已自动创建一组 issue，请 Copilot 列出它们。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。它可以自主规划、跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未表示偏好某个模型。编写工作坊时使用了 Claude Sonnet 4.5，因此将其写入说明，希望获得相对一致的代码建议。LLM 具有概率性，结果与示例不同也属正常情况。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，询问 Copilot 存储库中的积压 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，允许命令列出所有 issue。
5. 查看生成的列表。

Copilot 还会根据它判断的用户价值为 issue 排序。

## 查看说明文件

让智能体生成代码前，先查看将要提供给 Copilot 的上下文。使用 [UI 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，了解如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向 Astro、Tailwind CSS 和 Drizzle 等专项说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件的位置。这样可以复用内容、拆分大型说明，并提供示例和模板。

## 实现过滤功能

完成过滤至少需要修改以下内容：

- 在数据层（`src/lib/`）添加或完善过滤逻辑；
- 添加或更新过滤行为测试；
- 更新游戏列表页，增加过滤 UI。

Copilot Agent Mode 可以完成这些任务。

1. 继续当前对话，或选择 **New Chat** 创建新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中的智能体选择器仍然选中 **Agent**。
4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，根据积压 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、定位数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，包括探索项目、修改文件和运行测试，可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**；工作完成后再决定是否接受每项更改。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至暂时出现测试失败。这没关系：Copilot 会在生成代码和运行测试之间迭代，直到它认为任务完成且没有错误。
9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能有效并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜，你已使用 GitHub Copilot Agent Mode 为 Tailspin Toys 添加功能。本课中，你了解了：

- Agent Mode 如何跨 UI 和数据层实现新功能；
- Copilot 如何探索项目、识别相关文件并协调更改；
- 如何在合并前审查 Copilot 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 附加探索：实现分页

游戏列表增长后，需要提供分页。使用本练习学到的方法，请 Copilot 更新网站以实现分页。考虑是否允许用户选择每页大小，或固定每页数量；提供必要上下文，并通过迭代调整实现。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的 Agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
