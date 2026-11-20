---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app、连接从模板创建的仓库、熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款面向代理式开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和 CI 管道开箱即用。它专为同时指挥多个智能体的工作流而设计——每个智能体都在独立工作区中运行——而不是由你亲自完成所有工作，并可自动执行重复任务。安装好 Node.js 并准备好项目副本后，接下来安装 app 并连接仓库。

本课将学习如何：

- 安装 GitHub Copilot app 并登录。
- 通过 GitHub 仓库将项目添加到 app。
- 熟悉工作区，包括模板自动创建的待办事项。
- 尝试快速聊天，了解 app 本身。

## 场景

团队正开始使用 AI 智能体处理不断增长的待办事项。Copilot app 在一个位置集中管理这些工作：领取议题、运行智能体、审查更改和合并拉取请求。本课会引导你完成安装和连接，并学习如何针对项目开始对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装。Windows、macOS 和 Linux 都提供对应版本。现在安装 app、完成身份验证并将 Tailspin Toys 仓库添加到 app。

1. 在浏览器中打开 [GitHub Copilot app 登录页面][download-app]。
2. 下载适用于当前平台的 app，并按照页面上的说明安装。
3. 安装完成后打开 app。
4. 选择 **Sign in to GitHub**，然后按照提示使用 GitHub.com 或 GHE.com 账户登录。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **GitHub Enterprise Cloud**，并在提示时输入 `*.ghe.com` 主机名。

   如果使用 GitHub Enterprise Server，请先使用云端账户完成引导。然后打开 **Settings**，选择 **Accounts**、**Add account** 和 **GitHub Enterprise Server**。按照 [Enterprise Server 登录指南][ghes-sign-in]中的说明，在浏览器中授权 app。

5. 如果引导流程要求选择仓库，请选择从模板创建的 Tailspin Toys 仓库。也可以跳过仓库选择，在完成引导后再添加项目。
6. 按提示选择主题，然后完成引导。
7. 在侧边栏 **Sessions** 下查找仓库。如果它已经列出，无需重复添加。
8. 如果仓库尚未连接，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和仓库名称。
9. 确认仓库显示在侧边栏中。此处所用布局会将 `tailspin-toys` 等仓库文件夹归在 **Sessions** 下，每个仓库下方再列出各个会话。

## 熟悉工作区

项目连接后，花些时间熟悉界面。app 在侧边栏中划分了几个区域：

- **New** - 用于开始与 Copilot 的新聊天会话。
- **Pull requests** - 通过 app 的原生 GitHub 集成显示拉取请求。可以浏览和筛选 PR、查看 CI 状态并审查 PR，全程无需离开 app。
- **Issues** - 通过相同方式显示议题。可以浏览和筛选议题，并从议题开始会话。
- **Automations** — 保存的智能体任务，可按计划或按需运行。适合管理待办列表、定期项目维护或其他希望委派的繁琐任务。总结课程会将其作为后续探索内容，而非额外的研讨会练习。
- **Customize** - 通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。你将使用此区域配置 Playwright MCP。
- **Chats** — 适用于提问和头脑风暴的轻量对话，无需单独创建分支或工作区。开始第一个聊天后，此区域才会显示。如果未显示，请选择 **Sessions** 旁的 **+** 开始第一个聊天。
- **Sessions** — 按仓库文件夹和智能体会话分组。选择 `tailspin-toys` 等仓库名称可打开仓库视图；选择其下的会话可返回对应对话。每个会话都在独立的隔离工作区中运行，因此可以同时运行多个会话而不会发生更改冲突。添加星级评分时将开始第一个会话。

随着研讨会逐步进行，你会进一步探索此工作区！

> [!TIP]
> 遇到疑问时，可以询问 Copilot！如果不确定如何操作或某项功能是否可用，都可以询问 Copilot，它会帮助你了解。

### 查找预置的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在查看通过模板创建项目时自动生成的待办事项。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 仓库文件夹，而不是其下的某个会话。然后在仓库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假定它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题并阅读详情。每个议题也都可以作为启动智能体会话的入口。完成快速的首次更改后，你将从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示添加到 Copilot app 的仓库中的项目。想查看其他仓库的工作项？将这些仓库也添加到 app！

## 尝试快速聊天

熟悉 app 的一个好方法是询问 *app 本身*，而**快速聊天**正适合此用途。快速聊天无需创建分支或工作树，就可以提问或头脑风暴，非常适合快速、一次性的问题，无需创建会话。

1. 在侧边栏中选择 **Chats** 旁的 **+** 开始新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁的 **+** 开始第一个聊天；之后就会显示 **Chats**。
2. 询问 app 自身会话的工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 阅读对话视图中的回复。你会了解到，每个会话都在独立的 Git 工作树中运行，因此可以同时运行多个智能体，而不会发生更改冲突。你可以继续对话，也可以随时开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。你学会了如何：

- 安装 app 并登录 GitHub。
- 通过 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中查找预置的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[开始第一个智能体会话][next-lesson]，让它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
