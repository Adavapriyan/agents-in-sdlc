---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot cloud agent 承担了三项不同任务，旨在改善用户体验并添加功能。Cloud agent 虽然可以异步、自主地工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有带有未关闭拉取请求（PR）的智能体任务。你可以查看执行过的操作，也可以为正在进行的会话提供指导。

本课将学习如何：

- 浏览智能体页面并监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以借此添加浅色模式。由于已经在更新网站样式并添加切换功能，因此加入此功能合情合理。你要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表中应能看到你分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从仓库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和执行方式而异。

4. 还要注意右侧显示的拉取请求（PR）区域，它可以帮助你查看 PR 和已更改的文件，以便进一步监控。

## 引导 cloud agent

现在已经了解当前正在执行的任务，可以请求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切的标题会因 Copilot 使用的名称和当前工作状态而异。

    ![mission control 中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。此时会看到类似下面的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents 页面中的 cloud agent 任务，突出显示了引导活动会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成任务

和之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求纳入工作流程。

和之前一样，这需要几分钟。此时可以暂停一下，回顾至今所学和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。通过此 mission control，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，甚至重定向进行中的任务来调整范围或提供更多指导。

本课学习了以下概念：

- 使用 mission control 和智能体页面监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来转到[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
