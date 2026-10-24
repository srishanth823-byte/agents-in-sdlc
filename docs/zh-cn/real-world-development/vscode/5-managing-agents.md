---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

使用 agent mode 时，Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在本地实时发生，VS Code 中的 Copilot Chat 会持续显示工具调用和文件编辑。你可以在行内审查差异、接受或撤销单项更改，并通过后续提示引导对话，不必离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话中途引导智能体，在高对比度切换之外添加浅色模式切换。

## 场景

[练习 4][previous-lesson] 中使用的无障碍自定义智能体正在实现高对比度模式。工作过程中，设计团队又提出增加浅色模式的需求，以便偏好较亮主题的用户使用。与其从头开始新对话，不如引导当前会话扩展任务。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每一步：探索文件、运行命令和提出编辑。每项编辑都会以内嵌差异和 **Keep**/**Undo** 控件显示。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时滚动查看工具调用，包括文件读取、编辑和请求运行的终端命令。
3. 文件编辑出现后：
   - 选择 **Keep** 接受更改；
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以选择 Chat 顶部的 **Files changed** 指示器，查看智能体在本会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式，并查看其总结。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式切换。同一会话保留此前的组件、样式实现和 local storage 模式等上下文。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查已有更改并提出浅色模式的实现。
3. 像之前一样逐项审查新更改，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如尝试以不需要的方式重构切换 UI），可以使用输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 在当前会话中引导智能体通常比重新开始更快。可以用后续提示要求调整，例如“简化 CSS，使用 CSS 变量而不是重复选择器”、“不要添加第三个切换控件，将浅色模式并入现有设置面板”，或“为两个切换控件添加键盘快捷键”。

## 审查工作集

提交前，快速检查智能体修改的所有文件。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件列表，其中应包含 Astro 组件、样式和相关测试。
3. 打开部分更改文件并查看差异。确认自定义智能体建议的无障碍做法已应用，包括 ARIA 属性、键盘导航、语义化 HTML 和 local storage 持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监控并引导正在运行的会话：审查每项文件编辑、接受或撤销更改，并通过后续提示扩展工作，同时保留上下文。

本课介绍了：

- 实时监控智能体工作；
- 接受和撤销单个文件编辑；
- 通过后续提示在会话中途引导智能体。

下一课将把无障碍功能作为拉取请求发布，并审查此实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
