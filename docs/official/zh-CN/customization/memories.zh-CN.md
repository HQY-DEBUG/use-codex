> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/customization/memories.md)。

<a id="memories"></a>

# 记忆

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

记忆让 ChatGPT 和 Codex 将早期工作中的有用上下文带入未来的工作中。 ChatGPT Web 使用 ChatGPT 内存，而本地 Codex 客户端使用单独的本地内存存储和控件。

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

在 `AGENTS.md` 或签入文档中保留所需的团队指导。将记忆视为有用的回忆层，而不是必须始终适用的规则的唯一来源。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

在 ChatGPT 桌面应用程序中，使用 `/memories` 选择聊天是否可以使用本地记忆或贡献未来记忆。当您需要打开或关闭该功能时，可以通过 **设置 > 个性化** 管理该功能。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

从 **设置 > 个性化** 管理 ChatGPT 内存。 ChatGPT Work 使用您的帐户和工作区可用的内存设置；它不使用本地 Codex 内存存储或本地内存控件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在 Codex CLI 中，在交互式会话中使用 `/memories` 来控制当前聊天是否可以使用现有的本地记忆或成为未来记忆的输入。如果该命令不可用，请参阅 [配置本地内存](#configure-local-memories)。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

IDE 扩展使用连接的 Codex 主机的本地内存存储。当为该主机启用内存时，请使用与 Codex CLI 相同的聊天级别控件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

[电脑历史记录](computer-history.zh-CN.md) 是一项 macOS 桌面功能，可将允许的应用程序和网站之间的活动转化为 ChatGPT 和 Codex 可以参考的记忆和时间线。

</ContentModeSwitch>

<a id="how-memories-work"></a>
<a id="memory-storage"></a>
<a id="control-memories-per-thread"></a>
<a id="control-memories-per-chat"></a>
<a id="control-memories-per-task"></a>
<a id="review-memories"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="how-local-codex-memories-work"></a>

## 本地 Codex 存储器的工作原理

启用内存后，Codex 可以将符合条件的先前聊天中的有用上下文转换为本地内存文件。 Codex 跳过活动或短暂的会话，从生成的内存字段中编辑秘密，并在后台更新内存，而不是在每次聊天结束时立即更新。

聊天结束后，记忆可能不会立即更新。 Codex 会等到聊天空闲足够长的时间以避免总结仍在进行中的工作。

当您的 Codex 速率限制剩余百分比低于配置的阈值时，内存生成还可以跳过后台传递，因此当您接近限制时，Codex 不会花费配额。

<a id="local-memory-storage"></a>

## 本地内存存储

Codex 将内存存储在 Codex 主目录下。默认情况下，该值为 `~/.codex`。请参阅 [配置和状态位置](../config-file/config-advanced.zh-CN.md#config-and-state-locations) 了解 Codex 如何使用 `CODEX_HOME`。

主内存文件位于 `~/.codex/memories/` 下，包括摘要、持久条目、最近的输入以及之前聊天的支持证据。

将这些文件视为生成状态。您可以在排除故障时或共享 Codex 主目录之前检查它们，但不要依赖手动编辑它们作为主要控制界面。

<a id="control-local-memories-per-task"></a>

<a id="control-local-memories-per-chat"></a>

## 控制每个聊天的本地内存

在 ChatGPT 桌面应用程序和 Codex TUI 中，使用 `/memories` 控制当前聊天的内存行为。聊天级别的选择让您决定当前聊天是否可以使用现有的记忆以及Codex是否可以使用聊天来生成未来的记忆。

聊天级别的选择不会更改您的全局内存设置。

<a id="review-local-memories"></a>

## 回顾当地记忆

不要将秘密储存在记忆中。 Codex 会编辑生成的内存字段中的秘密，但您仍然应该在共享 Codex 主目录或生成的内存工件之前查看内存文件。

<a id="enable-memories"></a>
<a id="configuration"></a>

<a id="configure-local-memories"></a>

## 配置本地内存

默认情况下，本地 Codex 存储器处于关闭状态。在 ChatGPT 桌面应用程序中，打开 **设置 > 个性化** 并打开 **启用记忆**。

对于基于配置的设置，请将功能标志添加到 `config.toml`：

```toml
[features]
memories = true
```

有关配置文件位置和内存相关设置的完整列表，请参阅 [配置基础知识](../config-file/config-basic.zh-CN.md) 和 [配置参考](../config-file/config-reference.zh-CN.md)。

常见的内存特定设置包括：

- `memories.generate_memories`：控制新创建的聊天是否可以存储为内存生成输入。
- `memories.use_memories`：控制Codex是否将现有内存注入到未来的会话中。
- `memories.disable_on_external_context`：当 `true` 时，将使用外部上下文（例如 MCP 工具调用、Web 搜索或工具搜索）的聊天保留在内存生成之外。较旧的 `memories.no_memories_if_mcp_or_web_search` 密钥仍被接受作为别名。
- `memories.min_rate_limit_remaining_percent`：控制内存生成开始之前所需的最小剩余 Codex 速率限制百分比。
- `memories.extract_model`：覆盖用于每个聊天内存提取的模型。
- `memories.consolidation_model`：覆盖用于全局内存整合的模型。

</ContentModeSwitch>