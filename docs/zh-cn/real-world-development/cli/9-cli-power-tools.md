---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委托给 cloud agent 的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

像其他 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。接下来探索其他有用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索并行工作、worktree 和委托给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已经完成。现在查看其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令手动查看或压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未运行，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间中的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次运行 `/context` 并比较结果。如果对话较短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近满载时，Copilot CLI 会自动压缩。想自行选择时间时可运行 `/compact`。切换到不相关任务并需要全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也可能有不同偏好。Copilot CLI 可以列出并选择模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前版本中 `/share` 的选项。
2. 如需共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先审查它。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算公开的存储库或对话内容。

## 可选：扩展或委托工作

核心工作坊已完成。Copilot CLI 还提供了适用于大型任务的命令：

- `/fleet` 可以将独立子任务分配给多个子智能体并行处理；
- `/worktree` 可以为单独任务创建隔离的 Git worktree；
- `/delegate` 可以将任务发送给 Copilot cloud agent，由它异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建额外的 worktree 或远程任务。尝试前应创建范围清晰的新任务，并通过常规工作流审查结果。若想深入了解异步智能体工作，可继续学习 [Cloud agent 路径][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置 CLI、共享会话并查看幕后状态。本课中，你：

- 使用 `/context` 和 `/compact` 探索上下文管理；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索并行工作、worktree 和可选的 cloud agent 委托。

Copilot CLI 还有更多命令和功能值得探索。接下来通过[回顾已学内容][next-lesson]并了解后续资源，为本次工作流收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
