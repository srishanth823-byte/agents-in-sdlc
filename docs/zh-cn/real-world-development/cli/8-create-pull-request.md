---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查过滤功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

过滤功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支上。现在一起审查这些更改并创建拉取请求。之前你手动合并了星级评分 PR；这次让 **Agent Merge** 管理流程。

> [!NOTE]
> 实际工作中，功能、说明更新、技能更新和 QA 智能体通常会拆分到多个 PR。为简化工作坊，我们将整个过滤和质量工作流保留在一个对话和分支中，并合并为一个 PR。

在本课中，你将：

- 了解 Agent Merge 如何自动化合并生命周期；
- 检查完整功能更改和验证证据；
- 创建过滤功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个过滤功能流程中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 希望自动化剩余 PR 工作，同时由开发者掌握合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动完成 PR 合并前的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及必要时进行 rebase；然后在存储库允许时启用 GitHub 自动合并。

之前你都是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前，请审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码完成后，一起审查更改、创建 PR，并让 Agent Merge 处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 已成功进行 squash 合并”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或缺少权限的情况。如果流程被阻止，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已将代码生成、测试和验证以及 PR 流程中的多个步骤自动化。本课中，你：

- 了解了 Agent Merge 如何自动化合并生命周期；
- 检查了完整功能更改和验证证据；
- 创建了过滤功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来将[探索更多 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的 cloud agent 委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
