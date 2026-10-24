---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证过滤功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已经手动验证了代码，并通过说明文件确保它符合标准。接下来还要考虑测试、lint 和持续集成（CI）中的其他工作。

**智能体技能**很适合完成这类任务。技能可以帮助 Copilot 正确执行这些操作。

在本课中，你将：

- 探索现有的 `quality-checks` 技能；
- 自定义结果报告格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 说明、脚本和资源

智能体技能会打包可复用的任务说明、可执行脚本和辅助资源，供智能体按需加载。技能的核心是一个名为 `SKILL.md` 的 Markdown 文件，其中包含用于定义技能名称和说明的 frontmatter、技能概述，以及何时调用技能的指导。技能文件夹还可以包含脚本和其他资源。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目中已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

先查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace，在编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。`description` 可以帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何引导 Copilot 运行测试和 lint。
4. 注意该技能尚无 **Results output formatting** 部分。

## 修改前先运行技能

通过 Copilot CLI 或自然语言都可以调用技能。先请 Copilot 运行其中的三项检查：

1. 返回 Interactive 模式下的过滤功能对话。
2. 发送以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

接下来改进报告，使其说明运行了什么、是否成功，以及工具实际报告了什么。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 可在对话不重启的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次发送与之前相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最终报告，并与第一次报告进行比较。

## 总结和后续步骤

你自定义并使用了现有的智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了报告格式；
- 重新加载并运行技能。

这项更改会与过滤功能一起进入功能 PR。下一步将通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
