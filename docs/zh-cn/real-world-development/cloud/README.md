---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你在 GitHub 上分配任务后，cloud agent 会在后台处理：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

本系列练习将介绍如何添加 cloud agent 会遵循的自定义说明，然后分配 GitHub issue 并让它实现需求。你将审查并使用自定义智能体来引导它的工作方式，在智能体仪表板上监控并指导会话，最后审查它创建的拉取请求并迭代完善结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 将遵循的自定义说明 |
| [2. Cloud agent][ex2] | 异步智能体 | 将 issue 分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保你具备以下条件：

- [ ] 拥有已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为你的账户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可用于完成每条路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用 cloud agent。使用 Copilot Business 或 Enterprise 时，组织管理员必须先启用该功能。

> [!NOTE]
> 本路径不介绍 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 路径](../cli/)或 [VS Code 路径](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
