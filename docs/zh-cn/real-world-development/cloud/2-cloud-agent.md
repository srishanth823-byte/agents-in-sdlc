---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主执行代码更新和功能开发等任务。完成后，它会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交给 Copilot，加快开发流程，让开发者专注于更重要的工作。

本课将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围清晰的 issue；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初负责网站的承包商没有为代码编写文档，因此团队希望先为应用中导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每个游戏的详情页显示同一类别的其他游戏，方便访客继续浏览。暂时不需要完善的实现，只需能用于 UX 验收测试。此需求目前受到阻碍，但优先级高于它的其他 issue 还在排队。

这类任务很容易被推迟，很适合分配给 Copilot cloud agent。它可以异步处理，让开发者继续其他工作，之后再审查其结果。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。你可以通过多种方式交办工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话运行期间或结束后查看其过程。解决方案准备好审查时，它会在拉取请求中通知你。

## 明确任务范围的重要性

GitHub Copilot 并没有魔法——思考工作仍然不可或缺。即使看似简单的操作，深入分析后也可能很复杂，因此[为 Copilot cloud agent 明确任务范围][cloud-agent-best-practices]。将它视为 AI 配对程序员：分阶段工作、学习、试验并调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其引导 Copilot 的方式。如果你已经完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；否则现在可以快速查看。分配任务前，先只读检查存储库中的说明文件，以便之后识别它们对结果的影响。

在 GitHub Web UI 或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给 cloud agent。审查 PR 时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自说明文件。之后的练习会再次介绍审查过程。

## 设置 Copilot cloud agent 开发环境

无论由谁编写代码，通常都需要特定环境和安装步骤。分配给 Copilot 的任务也一样，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境。你可以在 `.github/workflows/copilot-setup-steps.yml` 中配置专用的[设置工作流][setup-workflow]，在开始工作前安装所需的开发工具和依赖项。本实验已预先配置该工作流，为 Copilot 提供 Node.js、项目依赖项、端到端测试所需的浏览器，以及单个 Astro 应用所用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果不在设置步骤中克隆，Copilot 会在步骤完成后自动执行。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来和其他 GitHub Actions 工作流相同，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的 job。Copilot 开始处理拉取请求前，GitHub Actions 会先运行该 job。
- 工作流使用 `workflow_dispatch` 触发器，因此可在存储库的 **Actions** 选项卡中手动运行。可以先测试它是否成功，而不必等 Copilot 启动工作。

## 添加文档

虽然大家都知道文档很重要，但许多项目中的信息已经过时，或完全缺失。这类技术债务常常无人处理，会降低效率，也让维护代码库或引入新开发人员变得更困难。Copilot 擅长生成文档，因此可以把这个任务分配给 cloud agent。它会在后台编写文档，之后的练习中再审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中创建待办列表。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方法。

13. 查看待办列表和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理 issue 的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到状态更新。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、分析现状、暂停思考并确定计划，以及创建代码。

这可能需要几分钟。Cloud agent 的主要用途之一，就是异步执行任务，让我们可以处理其他工作。接下来可以利用这一点：把另一个任务也分配给 cloud agent，同时自己继续开发应用功能。

## 为游戏详情页添加相关游戏

GitHub Copilot cloud agent 的一项优势是可以拆分工作：你处理一组任务，Copilot 同时处理另一组。为设计团队添加相关游戏区域可能不需要太久，但这段时间仍可用于其他工作。现在将任务交给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你应该会在 issue 的首条评论中看到 👀，表示 Copilot 已开始工作。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在继续了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，让它异步处理技术债务、实现功能，或协助迁移代码框架。

本课介绍了以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 明确任务范围的重要性；
- 将 issue 分配给 Copilot。

Cloud agent 正在后台工作，现在可以继续学习如何创建和使用自定义智能体。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可读取之前课程中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
