<a id="skills--plugins"></a>

# 技能与插件基础

> 本文根据本地英文原文 [skills-and-plugins.md](../en/skills-and-plugins.md) 翻译，为非官方简体中文译文。

> 完整文档索引请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。在文档页面的网址末尾添加 `.md`，即可获取该页面的 Markdown 版本。

技能和插件为 ChatGPT 与 Codex 提供合适的指令、资源和工具，帮助它们完成重复性工作。这样，你就不必在每次对话中都粘贴相同的提示词、模板、要求或流程。

[观看视频：ChatGPT 中的插件](https://www.youtube.com/watch?v=pKwRNdDtai0)

- **技能（skill）**将特定任务或工作流程所需的指令和辅助资源打包在一起。
- **插件（plugin）**是一个可安装的软件包，可以包含技能和模型上下文协议（Model Context Protocol，MCP）服务器。MCP 服务器提供工具，也可以选择包含自定义的 ChatGPT 用户界面。插件还可以为 Codex 运行时（包括 ChatGPT Work 和 Codex）提供[生命周期钩子](hooks.zh-CN.md)。

<a id="use-skills-for-repeatable-work"></a>

## 使用技能处理重复性工作

技能是一种可复用的工作流程，为 ChatGPT 或 Codex 提供针对特定任务的指导。它可以记录你现有的重复性工作方式，让任一产品在遇到该任务时都遵循同样的流程。

技能可以组合以下内容：

- 名称和说明，帮助 ChatGPT 与 Codex 判断何时适合使用该技能。
- 工作流程指令，定义具体过程和预期结果。
- 辅助资源，例如模板、示例、品牌规范、数据结构定义或已连接的工具。

当高质量结果依赖一套可重复执行的方法时，技能最有用。例如，技能可以用于准备每日简报、审阅文档、制作演示文稿、应用团队写作规范，或每周从相同的已连接工具中收集信息。

使用技能可以提高结果的一致性，将团队最佳实践融入工作流程，并通过共享标准流程来减少对未成文经验的依赖。

当你的请求与某项技能的用途匹配时，ChatGPT 和 Codex 可以自行选择该技能。你也可以明确指定：ChatGPT 支持用 `@` 提及技能，Codex 则支持用 `$` 提及技能。

<a id="build-skills"></a>

## 创建技能

你可以先将一项已有的重复性任务，整理成供 ChatGPT 和 Codex 使用的专门操作指南。适合入门的技能包括每周进展更新、营销活动简报、会后跟进，或任何需要保持步骤和格式一致的任务。

要创建实用的技能：

1. **选择一项明确、聚焦的任务。**记录你通常从哪些材料开始，例如文件、链接或笔记，以及完成后的结果应该是什么样。
2. **描述工作流程。**在 ChatGPT 中，以 `@skill-creator` 开始；在 Codex 中，使用 `$skill-creator`。说明目标、应遵循的步骤、预期格式，以及技能应始终包含或避免的内容。如果有模板或优秀示例，也一并提供。
3. **审阅并试用草稿。**检查指令，用一个真实场景中的请求测试技能。如果结果遗漏了步骤或偏离了所需格式，就继续调整。
4. **安装并重复使用。**启用后，ChatGPT 或 Codex 可以在相关请求中使用该技能，你也可以明确选择它。如果工作区设置允许，还可以与团队成员共享。

有关创建技能的更多细节，请参阅下面的专门指南。

[创建技能

      <Tools />

    使用 ChatGPT 和 Codex 创建、测试和共享可复用的技能。](https://learn.chatgpt.com/docs/build-skills)

<a id="use-plugins-for-tools-and-shared-workflows"></a>

## 使用插件获取工具和共享工作流程

插件让可复用能力更容易安装和共享。插件可以将技能与 MCP 服务器组合起来，获取 GitHub、Google Drive 或 Slack 等服务提供的工具和上下文。

ChatGPT 和 Codex 共用一个通用插件目录。当你想添加现成的工作流程时，可以浏览该目录。安装插件后，直接描述任务，或使用当前使用界面支持的调用语法，明确选择某个插件或其附带的技能。

[了解如何安装和使用插件](plugins.zh-CN.md)。

<a id="choose-between-a-skill-and-a-plugin"></a>

## 在技能与插件之间做出选择

如果你需要针对某项明确任务的可复用指令，请使用技能。如果你需要一个可安装的软件包，将指令与已连接的服务或其他工具组合起来，请使用插件。

你也可以通过[录制与回放（Record & Replay）](extend/record-and-replay.zh-CN.md)演示工作流程，该功能会将录制内容转化为可复用技能。要打包和分发你自己的插件包，请参阅[创建插件](https://developers.openai.com/plugins/build/plugins)。

如果你的插件需要连接服务或提供 MCP 工具，请参阅[构建 MCP 服务器](https://developers.openai.com/plugins/build/mcp-server)。当插件准备好接受公开审核时，请参阅[提交插件](https://developers.openai.com/plugins/deploy/submission)。

有关可复用工作流程的更多示例，请参阅 [OpenAI Academy：使用技能](https://openai.com/academy/skills/)。
