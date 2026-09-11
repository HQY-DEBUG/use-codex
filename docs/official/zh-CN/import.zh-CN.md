> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/import.md)。

<a id="import-from-another-agent"></a>

# 从其他智能体导入

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用导入流程将指令、设置、技能、插件、项目和最近的工作从另一个智能体导入到 ChatGPT 桌面应用程序或 Codex CLI 中。桌面应用程序可以从 **克劳德·科德**、**克劳德·科沃克** 或 **光标** 导入。 Codex CLI 可以从 **克劳德·科德** 或 **光标** 导入。

桌面应用程序直接导入支持的项目，并让您完成导入的插件或需要授权的连接的设置。您还可以使导入的工作与自动更新保持同步。

导入不会更改或删除您现有的智能体设置。



> 插图：ChatGPT 导入屏幕，用于选择要从中导入的其他 AI 应用程序。



<a id="start-an-import"></a>

## 开始导入

<a id="import-in-the-desktop-app"></a>

### 在桌面应用程序中导入

<WorkflowSteps>

1. 在 ChatGPT 桌面应用程序中，打开 **设置 > 导入**。如果 **进口** 尚不能用作设置部分，请打开 **一般** 并找到 **导入其他智能体设置**。
2. 选择**进口**。
3. 选择要从中导入的智能体，然后选择 **继续**。
4. 在 **选择要导入的项目** 上，选择要带过来的内容，然后选择 **继续**。
5. 导入完成后，打开导入的项目或聊天以继续工作。

</WorkflowSteps>

<a id="keep-imported-work-in-sync"></a>

### 保持导入的工作同步

在 ChatGPT 桌面应用程序中，打开 **设置 > 导入** 并打开自动更新，以使导入的工作与原始智能体保持同步。您还可以从同一设置部分查看导入历史记录。

<a id="import-in-codex-cli"></a>

### 在 Codex CLI 中导入

1. 启动本地 Codex CLI 会话并键入 `/import`。
2. 选择**克劳德·科德**或**光标**。
3. 选择要导入的受支持的设置、项目文件和最近的聊天记录。
4. 检查导入的配置并继续在 Codex 中工作。

Codex CLI 最多可导入过去 30 天内的 50 条聊天记录。 `/import` 命令在运行任务期间、远程会话中或连接到本地应用程序服务器守护程序时不可用。参见 [CLI 斜线命令](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-import-claude-code-or-cursor-setup-with-import)。



> 插图：ChatGPT 导入屏幕，用于选择要导入的设置、项目和最近的聊天记录。



<a id="how-importing-works"></a>

## 导入如何进行

导入流程会检查您的用户级设置和现有项目。用户级设置来自计算机上的文件。项目级设置来自您选择的仓库和文件夹中的文件。

导入时，ChatGPT：

1. 检测支持的设置和最近的工作。
2. 导入您选择的项目。
3. 保持您现有的智能体设置不变。
4. 检查导入的插件或连接是否仍需要设置。
5. 当您需要完成设置时显示状态卡。

<a id="what-chatgpt-can-import"></a>

## ChatGPT可以导入什么

| 进口商品 | 目的地 |
| --------------------------------- | ------------------------------------------------------- |
| 指令文件 | [`AGENTS.md`](agent-configuration/agents-md.zh-CN.md) |
| `settings.json` | [`config.toml`](config-file/config-basic.zh-CN.md) |
| 技能 | [技能](build-skills.zh-CN.md) |
| 插件 | 插件 |
| 现有项目文件夹 | 使用相同文件夹 | 的项目
| Claude Code 的项目记忆 | [回忆](customization/memories.zh-CN.md) |
| 过去 30 天的聊天记录 | ChatGPT 聊天记录 |
| MCP 服务器配置 | [Codex MCP 配置](extend/mcp.zh-CN.md) |
| 挂钩 | [Codex 挂钩](hooks.zh-CN.md) |
| 斜杠命令 | [技能](build-skills.zh-CN.md) |
| 子智能体 | [Codex 分智能体](agent-configuration/subagents.zh-CN.md) |

<a id="finish-setup-after-importing"></a>

## 导入后完成设置

导入完成后，应用程序会在左下角显示状态卡。如果导入的插件或连接仍需要设置，该卡会调出它。

当应用程序标记需要注意的项目时，选择 **完成** 并按照提示完成设置。

<a id="what-to-review-after-importing"></a>

## 导入后要检查什么

在依赖之前检查导入的设置，尤其是：

- 导入技能和智能体中的工具限制或权限。
- 使用自定义身份验证、标头、环境变量或传输的 MCP 服务器设置。您可能需要重新登录。
- 导入后其行为可能不同的挂钩。
- 插件、市场或其他需要手动跟进的设置。
- 依赖于参数、shell 插值或文件路径占位符的提示模板或命令样式提示。

<a id="after-you-import"></a>

## 导入后

导入完成后，打开导入的项目之一并从那里继续。请参阅 [使用ChatGPT](use-chatgpt.zh-CN.md) 以获取开始下一个任务的指导。