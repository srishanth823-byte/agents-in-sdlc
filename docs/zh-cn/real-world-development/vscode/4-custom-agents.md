---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可创建适用于开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以为 Copilot 提供专用说明、最佳实践、编码模式和领域知识。团队可以将经验固化为可复用智能体，例如确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维持一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都有包含 `name` 和 `description` 的 YAML frontmatter，后面跟着定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的对比

自定义智能体和[智能体技能][agent-skills-concept]都通过 Markdown 文件定义，并指导 AI 执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，可以在工作中编排技能，甚至其他智能体。本实验中的无障碍自定义智能体会按无障碍指南审查并更新网站，也可以调用拉取请求工作流或运行测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的写法。与 AI 的其他用法一样，需要通过测试和迭代找到适合当前环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户开放，无论其视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文本和背景对比度不足，导致部分用户难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相关功能。

本练习将：

- 查看现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已预先创建无障碍自定义智能体。先查看它如何引导 Copilot。

返回 codespace，在终端中从 `main` 创建新的无障碍功能分支，以便与练习 3 的过滤 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，查看其中的核心职责、无障碍最佳实践、HTML/CSS/JavaScript 示例，以及常见陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的自定义智能体。你可以选择一个智能体，让它按自己的说明和工具处理对话。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。各练习的模式和模型要求可能不同，具体步骤会逐一说明。

1. 如果 Chat 视图中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认显示 **default**）。
3. 从列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每次编辑都会在 Chat 中显示文件路径和差异，供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一练习中，你将审查并引导此任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即适用于特定任务和领域的专用 AI 助手。团队可以将经验和标准固化为可复用智能体，引导 Copilot 更有效地完成工作。

本课介绍了以下内容：

- 如何定义自定义智能体；
- 如何在 Copilot Chat agent mode 中使用自定义智能体。

接下来将[监控并引导智能体的工作][next-lesson]，审查更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
