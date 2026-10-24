---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明，添加文档标准，并将其用于过滤功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，还会说明代码应如何组织。过滤功能已经完成，接下来查看 Copilot 使用的说明、添加一项文档标准，并将其用于代码。

在本课中，你将：

- 了解存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件以确保遵循编码标准；
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指导。

通过说明文件，可以让 Copilot 按这些实践要求完成任务。

## 说明文件

自定义说明可以提供上下文和偏好，帮助 Copilot 理解你的编码风格和要求。你可以指定编码约定、库，以及注释的编写方式。说明可以适用于整个存储库，也可以仅适用于特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：此单一说明文件会随该存储库中的**每个**请求发送给 Copilot，应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：可为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持有所不同。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

项目已附带一组说明文件。先查看它们，再进行更改，以便观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些说明适用于你与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看文件，其中包含针对 Astro 文件、Drizzle 数据层、测试等的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置 glob 以确定适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和项目文件的链接。这样可以将较大的说明拆分为可复用文件，并提供示例。路径相对于说明文件本身。

## 更新说明以匹配团队指南

现在更新 `.github/copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（约第 35 行）。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更改后，退出并恢复过滤功能对话，以便 Copilot 使用新指导，同时保留功能上下文。

1. 如果过滤功能对话仍处于打开状态，输入 `/exit`。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如有提示，选择过滤功能对话，并确认已检出 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查变更的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取说明文件，并更新了过滤功能代码以符合新的文档标准。具体来说，你：

- 探索了存储库说明和路径范围说明文件；
- 更新了说明文件，要求遵循代码标准；
- 查看说明文件对代码的影响。

接下来使用可复用的 [quality-checks 技能][next-lesson]运行测试，确保检查流程一致。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持参考][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源][awesome-copilot]

[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
