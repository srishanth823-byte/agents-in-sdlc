---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你让 Copilot cloud agent 处理了三个与改善用户体验和添加功能有关的任务。Cloud agent 虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 Mission Control 中，可以查看所有带有未关闭拉取请求（PR）的智能体任务、了解执行过程，甚至引导正在运行的会话。

在本课中，你将：

- 使用智能体页面监控 cloud agent 任务；
- 引导正在运行的会话，要求它增加功能。

## 场景

分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。既然已经在调整网站样式并添加切换功能，将其一起实现会更合理。你将引导智能体，让它在高对比度模式之外也添加浅色模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应该能看到已分配给 Copilot 的任务，例如：
   - 更新代码文档；
   - 生成修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 检出存储库代码；
   - 创建 Copilot 工作环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的其他操作。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和实现方式而异。

4. 另请查看右侧的 PR 面板，以便监控 PR 和更改的文件。

## 引导 cloud agent

现在已了解当前任务，可以要求 Copilot 在处理高对比度模式时加入浅色模式切换。

1. 选择负责添加高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示设置已完成并开始处理任务。出现类似下方的消息时，就表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会开始处理更新后的任务。它会先完成发送消息时正在执行的步骤，然后将新要求纳入工作流程。

整个过程可能需要几分钟。等待期间，可以回顾目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪正在进行和已完成的任务、审查执行内容，还可以调整正在运行任务的范围或补充指导。

本课介绍了以下内容：

- 使用 Mission Control 和智能体页面监控 cloud agent 任务；
- 引导正在运行的会话，以要求添加功能。

Copilot 正在完成无障碍功能。接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
