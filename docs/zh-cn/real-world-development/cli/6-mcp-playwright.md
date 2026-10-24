---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索过滤功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码还涉及数据、外部服务，以及为 Copilot 提供自动化能力。MCP 服务器可以让 Copilot CLI 使用更多工具和服务。

在本课中，你将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它；
- 如果尚未安装 Playwright MCP 服务器，则进行添加；
- 请智能体驱动浏览器并探索过滤功能。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供与外部工具和服务通信的方式。借助 MCP，智能体可以实时访问最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。服务器充当 AI 智能体与外部工具、服务之间的桥梁；每个服务器都提供一组不同的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

GitHub 还托管了 [MCP registry][mcp-registry]，帮助社区发现和贡献 MCP 服务器。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 确认服务器类型为 **STDIO**，按 <kbd>Enter</kbd>，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

`--headless` 标志会让 Playwright 在无可见窗口的模式下运行，这对于没有桌面界面的 codespace 是必需的。

8. 列出 MCP 服务器，确认服务器已注册并处于活动状态：

   ```plaintext
   /mcp show
   ```

9. 确认列表中同时显示 `playwright` 和内置的 `github` 服务器。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 请 Copilot 使用 Playwright 探索功能

之前你手动确认了功能是否正常。现在使用 Copilot 中的 Playwright MCP 服务器执行相同检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断应使用 MCP 服务器，不一定需要明确指定。但指出应使用的工具有助于确保行为一致，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行检查的步骤。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并报告结果。

## 总结和后续步骤

恭喜，你已经通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中探索功能。本课中，你：

- 了解了 MCP 及 Copilot CLI 如何使用它；
- 添加了 Playwright MCP 服务器（如果之前尚未配置）；
- 请智能体驱动浏览器并探索过滤功能。

下一步将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具结合起来。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
