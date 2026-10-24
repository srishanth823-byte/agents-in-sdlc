---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉基础操作，并找到预先创建的过滤功能 issue。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是功能强大的代理式编码助手，可在终端中运行。它可以通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它能帮助你分担任务、请求更改并专注于工作。第一步是安装工具；你可以使用熟悉的工具完成此操作。

本课中，你将：

- 使用 npm 安装 GitHub Copilot CLI；
- 使用 GitHub 账户完成身份验证；
- 信任工作坊存储库并尝试快速对话；
- 使用内置 GitHub MCP 服务器查找过滤功能 issue。

## 场景

团队开始使用 AI 智能体处理不断增长的积压工作。Copilot CLI 将这种能力带入终端，许多开发者的主要工作环境。本课将完成安装和身份验证，为后续工作坊做好准备。

## 安装 Copilot CLI

你可以通过 [npm][install-cli]、WinGet 或 Homebrew 安装 Copilot CLI。GitHub Codespaces 已预装 Node.js，因此本练习使用 npm。

1. 返回 Codespace 并打开终端。
2. 确认 Node.js 已安装并满足版本要求：

   ```bash
   node --version
   ```

   应显示版本 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应显示版本号。

## 使用 GitHub 进行身份验证

首次启动 Copilot CLI 时，系统会提示你使用 GitHub 账户进行身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果系统提示身份验证，请按设备代码说明完成身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认显示的路径是 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**。

> [!NOTE]
> 在 Codespace 中，你可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有提示身份验证，就可以直接使用。

## 熟悉基本操作

普通 shell 提示符下的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给智能体，斜杠命令用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看当前安装版本提供的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，留意 Copilot 会先探索存储库再回答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 请 Copilot 查找过滤功能 issue：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读 issue。完成第一个小功能后还会再次使用它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建 worktree。每次进行更改前都会先创建功能分支。

## 使用工作坊快捷方式

Copilot CLI 通常会在使用未获既定权限允许的工具前征求同意。为了让你专注于本工作坊，接下来会使用 `--yolo` 重新启动 CLI，在 Codespace 中取消这些审批提示。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或 VM 等隔离环境中使用，不要将它设为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

本工作坊还会使用 `--enable-all-github-mcp-tools` 启用后续课程所需的 GitHub MCP 读写工具，以便操作 issue 和拉取请求。Codespace 限制了对本地计算机的访问，但经过身份验证的 GitHub 资源仍然是真实资源。发布或合并更改前，请先审查。

1. 使用 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再问 Copilot 一个关于项目的简单问题，确认对话正常，然后使用 `/exit` 退出。

Copilot 会自动保存对话。之后更新说明或添加智能体时，可以使用 `copilot --resume` 返回同一个功能对话和分支。

## 总结和后续步骤

恭喜！本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI；
- 使用 GitHub 账户完成身份验证；
- 信任了工作坊存储库并尝试快速对话；
- 使用内置 GitHub MCP 服务器找到了过滤功能 issue。

接下来开始[第一个专注的小改动][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
