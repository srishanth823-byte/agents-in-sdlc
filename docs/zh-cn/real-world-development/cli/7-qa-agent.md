---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合 issue 需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察过滤功能。现在创建一个自定义智能体，将这些能力整合到明确的 QA 流程中。

在本课中，你将：

- 了解自定义智能体如何配合说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，根据过滤功能 issue 审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，统一审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的专用 Copilot。配置文件描述智能体的用途、说明和可用工具。本课将创建 `.github/agents/qa.agent.md`，定义 QA 职责并在 Copilot CLI 中选择它。

不同自定义项承担不同职责：存储库说明描述团队标准，`quality-checks` 技能打包可重复检查，Playwright MCP 提供浏览器工具，而 QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告结果。它们各有用途，不需要额外对话。

## 创建 QA 配置

创建 PR 前，请 Copilot 创建可复用的 QA 配置，明确它要执行的检查和必须遵守的边界。

1. 返回 Codespace，确认过滤功能对话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认它捕获了预期的 QA 工作流和边界，避免它不完整或超范围地修改功能。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必填项；`name` 可选，但能提供清晰的显示名称。
3. 阅读说明，确认 QA 会从需求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供证据、修改实现代码前会询问，并且不会提交更改或创建 PR。
5. 如果配置遗漏了任何职责或边界，请让默认智能体先修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复过滤功能对话并选择 QA，以便它使用对话中已有的 issue 和规划决策。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会出现在可用列表中。仍可使用上述命令启用它。

2. 使用以下提示请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它的报告。

## 总结和后续步骤

你添加了可复用的专用角色，并审查了它的工作。本课中，你：

- 探索了自定义智能体如何配合说明、技能和 MCP 工具；
- 创建并检查 QA 配置；
- 选择 QA 智能体，根据 issue 审查功能。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好。下一步将[整合这些更改到功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
