---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用 agent mode 构建了过滤功能。创建拉取请求前，应先确认功能在浏览器中正常运行。无需手动操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 增加工具；
- 在工作区中添加 Playwright MCP 服务器；
- 请 Copilot 使用它在浏览器中手动测试过滤功能；
- 发布分支并为过滤功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

Agent mode 连接编辑器以外的工具后会更加强大。Model Context Protocol (MCP) 是 Copilot 连接外部工具的标准方式。

![Agent mode 的工作原理示意图，显示其如何与上下文、LLM 和工具交互，包括 MCP 服务器及 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问工具和资源。服务器负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组不同的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化功能。

GitHub 还托管了 [MCP registry](https://github.com/mcp)，帮助社区发现和贡献服务器。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查源代码、验证发布者并考虑安全影响。只使用可信服务器，并谨慎授予对敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该能看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含此 `github` 条目，因此上一练习中 Copilot 能读取积压 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，不需要在本机安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时进行 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]提供 Copilot 可以控制的浏览器。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，使文件如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志使浏览器在无可见窗口的模式下运行，这适用于没有桌面环境的 codespace。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器：Copilot 首次请求服务器工具时，VS Code 会启动服务器，并请你确认是否信任。

1. `.vscode/mcp.json` 中的 `playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请审查配置并选择信任。

手动启动是可选的。若跳过，VS Code 会在下一步 Copilot 首次请求工具时启动服务器并询问是否信任。

## 测试过滤功能

[Playwright MCP 服务器][playwright-mcp-server]提供可由 Copilot 驱动的真实浏览器。智能体可以打开页面、应用筛选并读取结果，无需你离开对话手动操作。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是截图。智能体会根据结构化、有标签的元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式类似。因此，快速功能测试也能兼顾基础无障碍检查。

连接服务器并启动应用后，请 Copilot 运行以下检查：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器、执行各步骤并报告结果。将报告与 issue 的验收标准对照；若发现问题，可以追问或请它在创建 PR 前修复代码。

> [!NOTE]
> 测试要求应用运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；若提示缺少浏览器，请运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支并创建 PR，供团队审查。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建 PR。现在请 GitHub Copilot 查找关联 issue 并创建 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**。
2. 确认智能体选择器中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按提示选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能需要登录 GitHub。
5. 查看 Copilot 如何搜索 issue、找到正确项并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时**不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了 MCP 以及 MCP 服务器如何为 Copilot 添加工具；
- 在工作区中添加 Playwright MCP 服务器；
- 使用它在发布前通过浏览器手动测试过滤功能；
- 发布分支并创建过滤功能 PR。

MCP 服务器可以用于测试功能，但 MCP 只是扩展 Copilot 的一种方式。下一步将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
