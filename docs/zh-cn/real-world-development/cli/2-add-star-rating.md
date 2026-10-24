---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过一次对话后，现在可以开始对项目进行第一次更改了。先从小功能入手：游戏数据中已有星级评分，但首页的游戏卡片还没有显示它。你将请智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支上启动专注的 Copilot 对话；
- 请智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 启动应用，并在转发的浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可能有星级评分，而且游戏详情页已经显示该评分。但首页的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将请智能体在每张卡片上显示已有评分。这项小而独立的更改非常适合首次使用。

## 对话的组成

**对话**是你使用 Copilot CLI 执行任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用 worktree。已保存的对话可供之后继续，但文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示三类内容：你的提示和智能体回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

开始新的对话以实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，运行：

   ```bash
   copilot --yolo
   ```

4. 使用 `/new` 斜杠命令启动新会话，然后按 <kbd>Enter</kbd>。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到显示游戏详情的文件并创建所需代码。现在已使用 Copilot CLI 添加了新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使改动很小也不例外。现在直接在 Copilot CLI 中检查。

1. 输入 `/diff`，检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 审查 Copilot 运行的检查结果；如有失败，请让它修复。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样具有概率性，因此生成的代码可能有所不同。请审查实际行为，而不要要求实现逐字一致。

## 检查更改

不要只读代码就假定它有效。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的 UI。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片以五分制显示评分。
4. 返回 Copilot，请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已经完成，现在创建拉取请求（PR），将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布第一个更改。你：

- 在功能分支上启动了专注的 Copilot 对话；
- 引导智能体修改游戏卡片；
- 使用 `/diff` 审查更改；
- 在转发的浏览器中运行应用并确认星级评分；
- 创建并合并了第一个拉取请求。

接下来，从过滤功能 issue 开始，使用 [Plan 和 Autopilot 模式][next-lesson]实现更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
