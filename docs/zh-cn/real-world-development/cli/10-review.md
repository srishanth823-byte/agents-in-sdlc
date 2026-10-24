---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的完整工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的过滤功能 issue；
- 添加星级评分，在转发浏览器中审查，并手动合并第一个拉取请求（PR）；
- 从过滤功能 issue 开始，在 Plan 模式确定方案、用 Autopilot 构建并在 Interactive 模式审查；
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能并运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中探索过滤功能；
- 创建并选择 QA 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整过滤功能，并授权 Agent Merge 处理该 PR；
- 探索用于上下文、模型、共享和可选 cloud agent 委托的斜杠命令。

## 已交付的成果

本工作坊有两个 PR 阶段，各自从已更新的 `main` 分支创建：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **过滤功能和质量工作流：**实现过滤、更新说明并应用到功能、定制 `quality-checks` 报告、创建 QA 配置并添加相关测试。

从规划过滤功能到创建 PR，你一直使用同一个对话和分支。为简化工作坊，我们将这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动测试、自己在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并使用新的格式报告结果。QA 智能体在 PR 创建前，汇总了这些结果以及需求和测试覆盖率审查。

新增测试应当弥补真实缺口；如果 QA 运行后认为无需添加测试，也可能是合理结果。合并前请审查代码和证据；代码修改后要更新受影响的验证证据。

## 最佳实践

为 Copilot 提供的上下文和工具会影响其工作结果。本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只看智能体总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先确定方案，使用 **Interactive** 持续参与重点更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可选择较快的模型，复杂任务则选择能力更强的模型。

上下文仍和基础设施同样重要。清楚说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出结果。

## 更多探索

核心工作流已完成。还可以尝试以下 CLI 功能：

- `/review`：请代码审查智能体分析更改；
- `/rubber-duck`：讨论问题并获取不同角度；
- `/fleet`：并行协调独立子任务；
- `/worktree`：隔离单独任务；
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方式就是持续使用它。可以将它用于生产代码、个人项目，或一直想开发的小应用。与团队分享经验，也向团队学习；同时继续阅读文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。也可以比较其他环境，探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

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
