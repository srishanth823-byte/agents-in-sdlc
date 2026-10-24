---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可帮助你创建适用于开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以提供聚焦的说明、最佳实践、编码模式和领域知识，引导 Copilot 更有效地完成特定类型的工作。团队可以将经验固化为可复用的智能体，例如确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维持一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都有包含 `name` 和 `description` 的 YAML frontmatter，后面跟着定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的对比

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者通常都通过 Markdown 文件定义，并告诉 AI 如何执行任务。最简单的区分方式是：**自定义智能体**负责执行工作，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，可以在工作中编排技能，甚至调用其他智能体。本实验中的无障碍自定义智能体会按照无障碍指南审查并更新网站；它还可以调用拉取请求工作流技能或运行测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的写法。与 AI 的其他用法一样，需要通过测试和迭代找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将了解自定义智能体的定义方式，以及如何将任务分配给它。

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户开放，无论其视觉能力或偏好如何。近期用户反馈指出，深色主题中文字和背景的对比度不足，导致部分用户难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相关功能。

## 查看无障碍自定义智能体

项目已预先创建无障碍自定义智能体。先查看其内容，了解它会如何引导 Copilot。

返回 codespace，然后查看无障碍智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，了解其中强调的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建并分配 issue

Mission Control 是管理环境中所有智能体的中心位置。你可以在这里分配任务、监控任务，并引导正在运行的会话。现在将创建高对比度模式的任务并分配给 Copilot。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云端智能体分配界面的截图，其中突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的专用 AI 助手。它们可以将团队经验和标准固化为可复用智能体，引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下内容：

- 自定义智能体的定义方式；
- 如何将任务分配给自定义智能体。

Copilot 正在处理高对比度模式。接下来，你将学习如何从 Mission Control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
