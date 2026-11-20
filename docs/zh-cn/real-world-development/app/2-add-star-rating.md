---
title: "第 2 课 - 添加星级评分：快速上手"
description: "在 GitHub Copilot app 中开始第一个智能体会话，小幅修改游戏卡片，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

熟悉工作区并尝试快速聊天后，现在开始**智能体会话**，对项目进行第一次更改。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。你将让智能体显示该评分、审查更改，并将其作为第一个拉取请求合并。

本课将学习如何：

- 开始智能体会话并了解会话结构。
- 让智能体对项目进行小幅、专注的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示该评分。但主页上的游戏卡片只显示标题、类别、发行方和说明。作为热身练习，你将让智能体在每张卡片上显示现有评分。这项更改小而独立，非常适合作为第一次会话的任务。

## 会话的组成

**会话**是与智能体进行的对话，每个会话都在独立工作区中运行。每个会话都有专用的 **git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而更改不会互相冲突。会话按仓库归类显示在侧边栏中，选择任一会话即可切换。

会话中有三项内容：与智能体的**对话**、智能体探索和编辑文件时执行的**工具操作**，以及显示差异的**已更改文件**列表。

## 开始会话并请求更改

现在开始一个新会话，探索项目并实现功能。在[设置 app][prior-lesson]时，已通过 GitHub 仓库添加了项目。接下来为该仓库创建新会话并请求更改。

1. 返回或打开 GitHub Copilot app。
2. 选择侧边栏顶部的 **+** 开始会话。
3. 选择 `tailspin-toys` 仓库。
4. 在提示框下方选择**新工作树**和 **Interactive** 模式。使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。然后探索项目，查找需要更新以添加功能的文件，并创建所需代码。你已使用 Copilot app 添加了新功能！

## 审查差异

即使是小改动，所有 AI 生成的更改也都应在合并前审查。现在直接在 Copilot app 中查看更改。

1. 选择 app 右上角的 **Toggle review panel**。在打开的面板中选择 **Changes**，查看更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 你应该会看到在用于显示游戏详情的核心文件 `GameCard.astro` 中添加了代码。代码应类似以下内容：在评分存在时显示评分，在 `starRating` 为 `null` 时显示 “No rating yet”：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能与上面的示例不同，但大体上应类似。

## 检查更改

打开浏览器前，先审查智能体自动检查的结果。确认测试覆盖了数值 `starRating` 和 `null` 回退状态。缺少先决条件或被跳过的检查不能算通过；批准任何安装请求前，请先检查请求内容。

当然，不能只阅读代码就假定一切正常。现在让 Copilot 打开网站，以便在浏览器画布中查看更新后的 UI。可以让它启动网站，然后在浏览器画布中打开。

> [!TIP]
> 画布是可直接在 Copilot app 中使用的交互式小组件。稍后会探索自定义画布并创建自己的画布，现在先使用内置的浏览器画布。

1. 使用以下提示，让 Copilot 启动 app 并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 片刻之后，app 会启动，Copilot app 中会打开浏览器窗口。
3. 确认带评分的游戏卡片显示了满分为五分的评分。
4. 完成后，使用以下提示让 Copilot 停止此会话启动的开发服务器：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

功能已创建完成！接下来创建拉取请求（PR），将新代码合并到现有代码库中。

1. 选择右上角的 **Create PR**。
2. 如果出现提示，选择 **Sign in with your browser** 并按照提示完成身份验证。
3. 如果 Copilot 说明仓库策略要求先关联相应议题才能创建 PR，请确认它应遵循该流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。你可以在此审查 PR。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot app 发布了第一个更改！具体来说，你：

- 开始智能体会话并了解会话结构。
- 让智能体对游戏卡片进行小幅、专注的更改。
- 在工作区差异视图中审查更改。
- 在本地运行 app，并在浏览器中确认星级评分。
- 打开 PR 1、审查检查结果并明确合并。

接下来，从筛选议题出发，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
