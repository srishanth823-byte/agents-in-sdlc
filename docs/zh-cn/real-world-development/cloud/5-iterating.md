---
title: "练习 5：迭代完善 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

本实验中，你与 GitHub Copilot 一起处理了多项改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换。现在审查代码更改，并在需要时向 Copilot 提供反馈。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 Copilot 提出的文档、相关游戏和无障碍功能更改，然后再开放给团队审查。

## GitHub Copilot cloud agent 的安全性

Copilot cloud agent 会异步且自主地执行任务，因此有一些安全约束：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并使用独立的临时环境。
- GitHub Actions 工作流需要先经人工批准才能运行。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先审查 GitHub Copilot cloud agent 创建的第一个 PR：为代码添加文档。你将使用 GitHub.com 上常见的 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[配置或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改会有所不同。留意 TSDoc、TypeScript 约定和文件注释头；这些来自练习 2 中查看的自定义说明文件。
6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查部分。如果一切正常，单个 Astro 应用的项目检查应当通过。完成可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并不是单向的。你可以像联系团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。它会看到评论并启动新会话来处理请求。由于生成结果具有非确定性，无法提供唯一的提示文本。可以要求 Copilot：

- 为每个代码文件添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建介绍 Astro 应用结构的 README。

1. 在文档 PR 中添加评论并标记 **@copilot**，请求它修改内容。可以使用上述建议，或提出其他文档要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更新。

可以继续迭代，直到满意为止。准备好后，将 PR 从草稿转换为可审查状态，再合并到 main 分支。

![将 PR 转换为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 的检查部分。单个 Astro 应用的项目检查应当通过，可能需要几分钟。
9. **可选：**可以在 codespace 中切换到 Copilot 创建的分支，手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为分支名称（例如 **copilot/fix-8**），然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。和之前一样，可以在 PR 的 **Conversation** 选项卡中评论，请求它继续修改，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 文档注释（此任务是在更新自定义说明之前分配的）。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 检查实现，特别留意：
   - 用于切换显示模式的 UI；
   - 如何通过 local storage 保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性，如 ARIA 标签和键盘导航；
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查部分。单个 Astro 应用的项目检查应当通过，可能需要几分钟。
10. **可选：**可以在 codespace 中切换到 Copilot 创建的分支，手动测试无障碍功能。打开终端，将 `<branch-name>` 替换为分支名称，然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然保留。

无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践来实现功能。如果发现问题或有改进建议，可以像之前一样在评论中标记 **@copilot**。

## 可选练习——继续委托任务

Cloud agent 最适合处理真实积压任务，让你可以转而处理其他工作。可以继续为存储库创建 issue 并分配给 Copilot，例如：

- 在游戏详情页添加支持者兴趣表单；
- 在游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 路径。通过这些课程，你：

- **查看了存储库附带的自定义说明文件**，了解它们如何影响 cloud agent 的输出；
- **将 issue 分配给 Copilot cloud agent**，观察它如何设置环境、规划并异步执行任务；
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换；
- **使用 Copilot agents 页面作为 Mission Control**，监控并引导正在运行的会话；
- **审查并迭代了 cloud agent 的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 路径。如果希望继续探索，可以学习其他路径：

- 🖥️ **[VS Code 路径](../../vscode/)**——在 IDE 中使用 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 路径](../../cli/)**——在终端完成两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 委托。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent；
- 为其他领域创建自定义智能体；
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
