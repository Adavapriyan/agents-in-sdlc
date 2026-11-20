---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题出发，在 Plan 模式中确定方案、在 Autopilot 模式中构建功能，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并使用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器验证证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选 cloud agent 委派的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为了简化研讨会流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你使用多种方式检查代码：自动测试、自己在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，为 PR 提供完整验证。

新增的测试应弥补真实缺口；QA 运行后不需要添加测试也可能是正确结果。授权合并前，请审查代码和证据；代码更改后，重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作成果。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置文件并设置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求进行调整。说明定义标准，技能描述可重复的任务，自定义智能体定义专用角色，而 MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是阅读智能体摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 参与专注的小型更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂任务则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响生成结果。

## 进一步探索

你已经完成核心工作流。还有一些值得探索的 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

提升任何工具使用能力的最好方式，就是持续使用它！可以将它用于生产代码、个人项目，或者那个想了多年却一直没开始实现的小应用。把经验分享给团队，也从团队中学习。并且，一如既往地，多查阅文档。

如果想继续从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
