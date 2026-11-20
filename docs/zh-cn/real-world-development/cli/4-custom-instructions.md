---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看仓库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，接下来将查看 Copilot 使用的说明、添加文档标准，并将它应用于代码。

本课将学习如何：

- 了解仓库说明和按路径限定的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应记录。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式设置和 lint 指南。

通过使用说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 提供更相关的建议和代码片段。你可以指定编码规范、库，甚至希望添加的注释类型。说明既可以应用于整个仓库，也可以应用于特定类型的文件，为具体任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会将这一个说明文件发送给 Copilot。文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：可用于为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 浏览项目中的自定义说明文件

入门项目已包含一系列说明文件。在更改之前，先查看现有文件，了解它们的作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意对项目的简要介绍和编码指导。此仓库中与 Copilot 的所有交互都适用这些说明。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意 Astro 文件、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定哪些文件适用这些说明。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意它指向的其他说明文件和项目现有文件。通过这种方式，可以将较大的说明集拆分成较小、可复用的文件，并提供可遵循的示例。

## 更新说明文件以符合团队指导

现有文件是不错的起点，但仍有缺失。现在修改核心 `copilot-instructions.md` 文件，确保 Copilot 为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
2. 在该章节末尾添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在每次对话开始时加载仓库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时使用新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看更改过的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

你了解了 Copilot 如何从说明文件获取上下文，并将新标准应用到了功能中。具体来说，你：

- 了解仓库说明和按路径限定的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot - 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
