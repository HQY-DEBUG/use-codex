> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/developer-settings.md)。

<a id="developer-settings"></a>

# 开发者设置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT 将产品和工作区首选项存储在 ChatGPT 设置中。 ChatGPT Work 聊天在托管环境中运行，不会读取本地 Codex 配置文件。使用您的工作区在 ChatGPT 设置中公开的控件；工作区管理员可以为您管理一些设置。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

[设置](reference/settings.zh-CN.md) 常规页面涵盖应用程序首选项，包括个人资料、键盘快捷键、通知、外观、个性化、记忆和存档聊天。

<a id="project-and-terminal-behavior"></a>

## 项目和终端行为

选择文件打开位置、聊天中显示多少命令输出以及默认打开终端选项卡的位置。

<a id="code-review"></a>

## 代码审查

在 **设置 > Git** 下，使用 **审核交付** 选择 **内联** 以在可能的情况下在当前聊天中运行 `/review`，或使用 **独立的** 启动单独的审阅聊天。

<a id="ide-extension-sync"></a>

## IDE扩展同步

当 ChatGPT 桌面应用程序和 IDE 扩展在同一项目中打开时，它们共享活动聊天和编辑器上下文。从应用程序编辑器中打开 **IDE环境**，让 Codex 使用当前在编辑器中打开的文件。当您希望应用程序提示排除该编辑器上下文时，请将其关闭。

您可以从 IDE 扩展打开应用程序聊天，并在应用程序中继续 IDE 聊天。两个使用界面使用相同的项目来确定要共享哪些聊天和上下文。

<a id="agent-configuration"></a>

## 智能体配置

应用程序中的 Codex 智能体继承与 IDE 扩展和 CLI 相同的配置。使用应用程序内控件进行常用设置，或编辑 `config.toml` 进行高级选项。详细信息请参见 [智能体审批和安全](agent-approvals-security.zh-CN.md) 和 [配置基础知识](config-file/config-basic.zh-CN.md)。

<a id="git"></a>

## git

使用Git设置标准化分支命名并选择Codex是否使用强制推送。您还可以设置 Codex 用于生成提交消息和拉取请求描述的提示。

<a id="integrations-and-mcp"></a>

## 集成和 MCP

通过模型上下文协议（MCP）连接外部工具。启用推荐的服务器或添加您自己的服务器。如果服务器需要 OAuth，应用程序将启动身份验证流程。这些设置也适用于 Codex CLI 和 IDE 扩展，因为 MCP 配置位于 `config.toml` 中。详细信息请参见 [模型上下文协议](extend/mcp.zh-CN.md)。

<a id="browser-developer-mode"></a>

## 浏览器开发者模式

在**开发者模式**下，打开**启用完全 CDP 访问**，让ChatGPT使用Chrome DevTools协议进行性能分析和更深入的浏览器调试。如果您的组织已禁用完全 CDP 访问权限，则您无法在本地启用它。有关设置、风险、批准和管理员要求，请参阅 [开发者模式](https://learn.chatgpt.com/docs/browser?surface=app#app-developer-mode)。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

Codex CLI 从 `~/.codex/config.toml` 读取您的个人默认值。当您需要特定于项目的覆盖时，将 `.codex/config.toml` 文件添加到受信任的项目或子文件夹中。 CLI 和 IDE 扩展共享这些配置层。

<a id="inspect-your-settings"></a>

## 检查您的设置

使用以下命令了解当前会话的有效设置：

- 运行 `/status` 以查看活动模型、审批策略、可写根和令牌使用情况。
- 运行 `/debug-config` 以按优先顺序查看配置层以及托管策略要求的来源。
- 使用 `--strict-config` 启动 Codex，将无法识别的 `config.toml` 密钥视为错误，而不是忽略它们。

有关完整的交互式命令参考，请参阅 [开发者命令](developer-commands.zh-CN.md)。

<a id="change-settings-for-one-run"></a>

## 更改一次运行的设置

如果存在专用标志，请使用该标志。常见示例包括 `--model`、`--sandbox`、`--ask-for-approval`、`--profile` 和 `--search`。使用 `-c` 或 `--config` 覆盖一次运行的任何支持的配置密钥：

```shell
codex --model gpt-5.5
codex --profile deep-review
codex --config model_reasoning_effort='"high"'
```

命令行标志和 `--config` 值具有最高优先级。有关完整的标志列表，请参阅 [开发者命令](developer-commands.zh-CN.md)。

<a id="configuration-layers"></a>

## 配置层

CLI 在项目、配置文件、用户、系统和内置设置之前应用命令行标志和 `--config` 覆盖。使用该优先级来保留配置文件中的共享默认值以及命令行上的一次性更改。

有关完整的订单和常用选项，请参阅 [配置基础知识](config-file/config-basic.zh-CN.md)。

<a id="change-settings-in-the-tui"></a>

## 更改 TUI 中的设置

交互式终端 UI 提供了用于公共会话和显示设置的选择器：

| 目标 | 命令 | 相关配置 |
| ------------------------- | -------------------------------------------- | --------------------------------------------- |
| 选择模型 | `/model` | `model`、`model_reasoning_effort` |
| 更改权限 | `/permissions` | 批准和沙箱设置 |
| 选择响应样式 | `/personality` | `personality` |
| 配置可选工具 | `/experimental`、`/memories` | 功能特定 `config.toml` 按键 |
| 自定义终端 UI | `/keymap`、`/statusline`、`/title`、`/theme` | `[tui]` | 下的按键
| 设置编辑默认值 | `/vim`、`/raw` | `tui.vim_mode_default`、`tui.raw_output_mode` |

有些命令仅适用于当前会话，而有些选择器则提供保存选择的功能。当您希望将来的会话使用默认值时，请设置相关的配置键。有关终端主题、编辑器、补全和快捷方式工作流程，请参阅 [CLI定制](cli-customization.zh-CN.md)。

<a id="settings-references"></a>

## 设置参考

- [高级配置](config-file/config-advanced.zh-CN.md) 涵盖配置文件、一次性覆盖和其他高级工作流程。
- [配置参考](config-file/config-reference.zh-CN.md) 列出了可用的键和值。
- [配置示例](config-file/config-sample.zh-CN.md) 提供了完整的示例文件。
- [环境变量](config-file/environment-variables.zh-CN.md) 记录 CLI 和安装程序使用的变量。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

Codex IDE 扩展有两个设置层：

- **Codex设置** 控制智能体行为与 Codex CLI 共享，包括模型、推理工作、权限、沙箱、MCP 服务器和个性化。 Codex 从 `config.toml` 读取这些设置。
- **编辑器设置** 控制扩展在 VS Code 和兼容编辑器中的行为方式。这些设置使用编辑器设置系统中的 `chatgpt.*` 键。

<a id="open-codex-settings"></a>

## 打开Codex设置

选择 Codex 侧栏中的齿轮图标，然后选择 **Codex 设置**。使用常见智能体控件的设置面板，或选择 **打开config.toml** 直接编辑活动配置层。

配置层顺序和常用密钥请参见[配置基础知识](config-file/config-basic.zh-CN.md)。对于每个受支持的 `config.toml` 密钥，请参阅 [配置参考](config-file/config-reference.zh-CN.md)。

<a id="change-an-editor-setting"></a>

## 更改编辑器设置

要更改设置，请按照下列步骤操作：

1. 打开您的编辑器设置。
2. 搜索 `@ext:openai.chatgpt`、`Codex` 或设置名称。
3. 更新值。

该扩展还支持 VS Code 针对 Codex 聊天界面的内置聊天字体设置。

<a id="editor-settings-reference"></a>

## 编辑器设置参考

| 设置 | 默认值 | 说明 |
| -------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `chatgpt.commentCodeLensEnabled` | `true` | 在 `TODO` 注释上方显示 CodeLens，以便 Codex 可以解决它们。                                                                                                                                                                                                                             |
| `chatgpt.openOnStartup` | `false` | 当扩展完成启动时，聚焦 Codex 侧边栏。                                                                                                                                                                                                                              |
| `chatgpt.followUpQueueMode` | `queue` | 选择运行期间发送的消息是等待下一次运行 (`queue`) 还是引导当前运行 (`steer`)。该扩展将旧的 `interrupt` 值视为 `steer`。新闻<kbd>Cmd</kbd>/<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Enter</kbd>反转一条消息的行为。 |
| `chatgpt.composerEnterBehavior` | `enter` | 选择是否<kbd>Enter</kbd>总是发送（`enter`），<kbd>Cmd</kbd>/<kbd>Ctrl</kbd>+<kbd>Enter</kbd>发送多行提示 (`cmdIfMultiline`)，或者始终需要修饰符 (`cmdAlways`)。                                                                                      |
| `chatgpt.reviewDelivery` | `inline` | 尽可能在当前聊天中运行 `/review` (`inline`) 或启动单独的审核聊天 (`detached`)。                                                                                                                                                                                   |
| `chatgpt.localeOverride` | 自动 | 设置 Codex UI 的首选语言。留空以自动检测。                                                                                                                                                                                                       |
| `chatgpt.runCodexInWindowsSubsystemForLinux` | `false` | 仅 Windows：当 WSL 可用时，在 WSL 中运行 Codex。当您的仓库和工具位于 WSL2 中或需要 Linux 原生工具时，请使用此选项。更改此设置会重新加载 VS Code。                                                                                               |
| `chatgpt.cliExecutable` | 取消设置 | 仅开发：设置 Codex CLI 可执行文件的路径。除非您正在开发 Codex CLI，否则不需要此设置；手动覆盖捆绑的可执行文件可能会导致部分扩展无法工作。                                                                |
| `chat.fontSize` | 编辑器默认 | 控制Codex侧边栏中的聊天文本，包括聊天内容和编写者。                                                                                                                                                                                                           |
| `chat.editor.fontSize` | 编辑器默认 | 控制 Codex 聊天中代码渲染的内容，包括代码片段和差异。                                                                                                                                                                                                           |

上面的 `chatgpt.*` 密钥属于 IDE 扩展，不在 `config.toml` 中。对于共享智能体设置，请使用 [配置基础知识](config-file/config-basic.zh-CN.md)、[高级配置](config-file/config-advanced.zh-CN.md) 和 [配置参考](config-file/config-reference.zh-CN.md)。

</ContentModeSwitch>