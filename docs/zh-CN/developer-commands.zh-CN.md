> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/developer-commands.md)。

<a id="developer-commands"></a>

# 开发者命令

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT web 有自己的输入框命令菜单。输入 `/` 查看当前聊天中可用的操作。它不会公开 ChatGPT 桌面应用程序或 CLI 命令集；本参考中的 Codex 斜线命令、CLI 子命令和标志不适用于 ChatGPT Web。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="chatgpt-desktop-app-commands"></a>

## ChatGPT 桌面应用程序命令

常规 [命令](reference/commands.zh-CN.md) 页面涵盖应用程序导航、聊天快捷方式、键盘自定义以及聊天、设置、技能、计划任务、插件和宠物的深层链接。

[斜杠命令](https://learn.chatgpt.com/docs/reference/slash-commands) 页面涵盖了应用程序编辑器中可用的命令，包括 `/feedback`、`/goal`、`/init`、`/mcp`、`/plan`、`/review` 和 `/status`。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="how-to-read-this-reference"></a>

## 如何阅读本参考资料

此页面列出了每个记录的 Codex CLI 命令和标志。使用交互式表格按关键字或描述进行搜索。每个部分都显示选项的成熟度并标记已弃用的选项和有风险的组合。

CLI 继承了 `~/.codex/config.toml` 的大部分默认值。您在命令行中传递的任何 `-c key=value` 覆盖都优先于该调用。有关详细信息，请参阅 [配置基础知识](config-file/config-basic.zh-CN.md#configuration-precedence)。

<a id="global-flags"></a>

## 全球旗帜

<ConfigTable client:load options={globalFlagOptions} />

这些选项适用于基本 `codex` 命令。大多数传播为命令；有关例外情况，请参阅上面的注释或相关命令帮助。对于传播的标志，请遵循相关命令帮助。例如，`codex exec --oss ...` 将 `--oss` 应用于 `exec`。

<a id="command-overview"></a>

## 命令概述

成熟度列使用功能成熟度标签，例如实验性、测试版、稳定和已弃用。请参阅 [功能成熟度](feature-maturity.zh-CN.md) 了解如何解释这些标签。

<ConfigTable
  client:load
  options={commandOverview}
  secondColumnTitle="Maturity"
  secondColumnVariant="maturity"
/>

<a id="command-details"></a>

## 命令详细信息

<a id="codex-interactive"></a>

### `codex`（交互式）

运行不带子命令的 `codex` 会启动交互式终端 UI (TUI)。智能体接受上面的全局标志以及图像附件。 Web搜索默认为缓存模式；使用 `--search` 切换到实时浏览。对于低摩擦局部工作，请使用 `--sandbox workspace-write --ask-for-approval on-request`。

使用 `--remote ws://host:port` 或 `--remote wss://host:port` 将 TUI 连接到以 `codex app-server --listen ws://IP:PORT` 启动的应用程序服务器。对于本地 Unix 套接字，请使用 `--remote unix://` 作为默认套接字，或使用 `--remote unix://PATH` 作为显式路径。添加 `--remote-auth-token-env<ENV_VAR>` 当服务器需要持有者令牌进行 WebSocket 身份验证时。

<a id="codex-app-server"></a>

### `codex app-server`

在本地启动 Codex 应用服务器。这主要用于开发和调试，可能会更改，恕不另行通知。

<ConfigTable client:load options={appServerOptions} />

`codex app-server --listen stdio://` 保留默认的 JSONL-over-stdio 行为，`codex app-server --stdio` 是该传输的别名。 `--listen ws://IP:PORT` 为应用程序服务器客户端启用 WebSocket 传输。服务器接受 `ws://` 监听 URL；当客户端与 `wss://` 连接时，使用 TLS 终止或安全代理。使用 `--listen unix://` 在 Codex 的默认 Unix 套接字上接受 WebSocket 握手，或使用 `--listen unix:///absolute/path.sock` 选择套接字路径。如果您为客户端绑定生成架构，请添加 `--experimental` 以包含门控字段和方法。

添加 `--code-mode-host wss://code-mode.example.com/host` 将应用程序服务器连接到远程代码模式主机，而不是启动本地主机。此出站连接与 `--listen` 分开，并由应用程序服务器进程中的每个线程共享。仅将 `ws://` 用于本地主机或 SSH 转发的主机。

<a id="codex-remote-control"></a>

### `codex remote-control`

运行`codex remote-control`，在前台启动远程控制。使用 `codex remote-control start` 启动启用远程控制的本地应用程序服务器守护进程，并使用 `codex remote-control stop` 停止它。托管远程控制客户端和 SSH 远程工作流程使用这些命令；当您构建本地协议客户端时，它们不能替代 `codex app-server --listen`。

守护进程运行后，使用 `codex remote-control pair` 创建并打印短暂的手动配对代码。将 `--json` 添加到任何远程控制命令中以获得机器可读的输出。对于 `pair`，JSON 响应包括 `pairingCode`、`manualPairingCode`、`environmentId` 和 `expiresAt`。

<a id="codex-app"></a>

### `codex app`

从 macOS 或 Windows 上的终端启动 ChatGPT 桌面应用程序。在macOS上，Codex可以打开特定的工作区路径；在 Windows 上，Codex 打印要打开的路径。

<ConfigTable client:load options={appOptions} />

`codex app` 打开已安装的 ChatGPT 桌面应用程序，或在应用程序丢失时启动安装程序。在 macOS 上，Codex 打开提供的工作区路径；在 Windows 上，它会打印安装后打开的路径。

<a id="codex-debug-app-server-send-message-v2"></a>

### `codex debug app-server send-message-v2`

使用内置的应用程序服务器测试客户端通过应用程序服务器的 V2 线程/回合流发送一条消息。

<ConfigTable client:load options={debugAppServerSendMessageV2Options} />

此调试流程使用 `experimentalApi: true` 进行初始化、启动线程、发送轮次并流式传输服务器通知。使用它在本地重现和检查应用程序服务器协议行为。

<a id="codex-debug-models"></a>

### `codex debug models`

将原始模型目录 Codex 打印为 JSON。

<ConfigTable client:load options={debugModelsOptions} />

当您只想检查与当前二进制文件捆绑的目录而不从远程模型端点刷新时，请使用 `--bundled`。

<a id="codex-debug-prompt-input"></a>

### `codex debug prompt-input`

将精确的模型可见提示输入列表渲染为 JSON。在调试指令发现、会话上下文或提示构造时使用此选项。

<ConfigTable client:load options={debugPromptInputOptions} />

<a id="codex-apply"></a>

### `codex apply`

将 Codex 云聊天中的最新差异应用到本地仓库。您必须进行身份验证并有权访问聊天。

<ConfigTable client:load options={applyOptions} />

如果 `git apply` 失败（例如，由于冲突），Codex 打印修补的文件并以非零值退出。

<a id="codex-review"></a>

### `codex review`

以非交互方式运行代码审查。仅选择一个审阅目标，或将自定义审阅说明作为提示传递。

<ConfigTable client:load options={reviewOptions} />

`--uncommitted`、`--base`、`--commit` 和自定义 `PROMPT` 相互冲突。 `--title` 只能与 `--commit` 一起使用。

<a id="codex-archive-and-codex-unarchive"></a>

### `codex archive` 和 `codex unarchive`

通过会话 ID 或会话名称存档或恢复已保存的交互式会话。当您想要清理会话选择器而不删除记录时，请使用这些命令。会话 ID 优先于会话名称。

```bash
codex archive <SESSION>
codex unarchive <SESSION>
```

<ConfigTable client:load options={archiveOptions} />

<a id="codex-delete"></a>

### `codex delete`

通过会话 ID 或会话名称永久删除已保存的交互式会话。仅当您想要删除脚本而不是将其从活动会话列表中隐藏时才使用此选项。

```bash
codex delete <SESSION>
codex delete <SESSION_UUID> --force
```

<ConfigTable client:load options={deleteOptions} />

仅将 `--force` 与会话 UUID 结合使用。命名会话仍需要确认，因此 Codex 不会在没有提示的情况下删除重复或不明确的名称。

<a id="codex-cloud"></a>

### `codex cloud`

从终端与 Codex 云聊天进行交互。默认命令打开一个交互式选择器； `codex cloud exec`直接提交任务，`codex cloud list`返回最近的聊天记录以供脚本编写或快速检查。

<ConfigTable client:load options={cloudExecOptions} />

身份验证遵循与主 CLI 相同的凭据。如果任务提交失败，Codex 将以非零值退出。

<a id="codex-cloud-list"></a>

#### `codex cloud list`

列出最近的云聊天，并提供可选的过滤和分页功能。

<ConfigTable client:load options={cloudListOptions} />

纯文本输出打印任务 URL，后跟状态详细信息。使用 `--json` 进行自动化。 JSON 负载包含一个 `tasks` 数组以及一个可选的 `cursor` 值。每个任务包括 `id`、`url`、`title`、`status`、`updated_at`、`environment_id`、`environment_label`、`summary`、`is_review` 和 `attempt_total`。

<a id="codex-completion"></a>

### `codex completion`

生成 shell 完成脚本并将输出重定向到适当的位置，例如 `codex completion zsh > "${fpath[1]}/_codex"`。

<ConfigTable client:load options={completionOptions} />

<a id="codex-doctor"></a>

### `codex doctor`

在提交支持问题之前或在调查损坏的 Codex 安装时生成本地诊断报告。该报告检查安装、配置、身份验证、运行时、Git、终端、应用程序服务器和线程库存运行状况。

<ConfigTable client:load options={doctorOptions} />

<a id="codex-features"></a>

### `codex features`

管理存储在 `$CODEX_HOME/config.toml` 中的功能标志。 `enable` 和 `disable` 命令保留更改，以便它们应用于将来的会话。 `features` 子命令不接受 `--profile`。

<ConfigTable client:load options={featuresOptions} />

<a id="codex-exec"></a>

### `codex exec`

使用 `codex exec`（或简称 `codex e`）进行脚本化或 CI 风格的运行，无需人工交互即可完成。

<ConfigTable client:load options={execOptions} />

Codex 默认写入格式化输出。添加 `--json` 以接收换行符分隔的 JSON 事件（每个状态更改一个）。可选的 `resume` 子命令允许您继续非交互式任务。使用 `--last` 从当前工作目录中选择最近的会话，或添加 `--all` 以搜索所有会话：

<ConfigTable client:load options={execResumeOptions} />

<a id="codex-execpolicy"></a>

### `codex execpolicy`

在保存 `execpolicy` 规则文件之前检查它们。 `codex execpolicy check` 接受一个或多个 `--rules` 标志（例如，`~/.codex/rules` 下的文件）并发出 JSON，显示最严格的决策和任何匹配规则。添加 `--pretty` 以格式化输出。 `execpolicy` 命令当前处于预览状态。

<ConfigTable client:load options={execpolicyOptions} />

<a id="codex-login"></a>

### `codex login`

使用 ChatGPT 帐户、API 密钥或访问令牌对 CLI 进行身份验证。如果没有标志，Codex 将打开 ChatGPT OAuth 流程的浏览器。

<ConfigTable client:load options={loginOptions} />

当凭证存在时，`codex login status` 会以 `0` 退出，这在自动化脚本中很有帮助。

<a id="codex-logout"></a>

### `codex logout`

删除 API 密钥和 ChatGPT 身份验证的已保存凭据。该命令没有标志。

<a id="codex-mcp"></a>

### `codex mcp`

管理存储在 `~/.codex/config.toml` 中的模型上下文协议服务器条目。

<ConfigTable client:load options={mcpCommands} />

`add` 子命令支持 stdio 和可流式 HTTP 传输：

<ConfigTable client:load options={mcpAddOptions} />

OAuth 操作（`login`、`logout`）仅适用于可流式 HTTP 服务器（并且仅当服务器支持 OAuth 时）。

<a id="codex-plugin"></a>

### `codex plugin`

从配置的市场安装、列出和删除插件。

<ConfigTable client:load options={pluginCommands} />

`codex plugin add --json` 打印 `pluginId`、`name`、`marketplaceName`、`version`、`installedPath` 和 `authPolicy`。 `codex plugin list --json` 打印 `installed` 和 `available` 数组。条目包括 `pluginId`、`name`、`marketplaceName`、`version`、`installed`、`enabled`、`source`、`installPolicy`、`authPolicy`，以及（如果有） `marketplaceSource` 具有配置的市场源类型和值。 `codex plugin remove --json` 打印 `pluginId`、`name` 和 `marketplaceName`。

<a id="codex-plugin-marketplace"></a>

### `codex plugin marketplace`

管理 Codex 可以浏览和安装的插件市场源。

<ConfigTable client:load options={marketplaceCommands} />

`codex plugin marketplace add` 接受 GitHub 简写，例如 `owner/repo` 或 `owner/repo@ref`、HTTP 或 HTTPS Git URL、SSH Git URL 和本地市场根目录。使用 `--ref` 固定 Git 引用，并重复 `--sparse PATH` 对 Git 支持的市场仓库使用稀疏签出。

`codex plugin marketplace list` 打印范围内的市场名称和根，包括隐式发现的默认市场和配置的市场快照。

将 `--json` 添加到市场添加、列出、升级或删除命令，以实现自动化友好的输出。市场添加JSON包括`marketplaceName`、`installedRoot`和`alreadyAdded`； list JSON 包含一个 `marketplaces` 数组，其中包含 `name`、`root` 和可选的 `marketplaceSource`；升级JSON包括`selectedMarketplaces`、`upgradedRoots`和`errors`；删除 JSON 包括 `marketplaceName` 和 `installedRoot`。

<a id="codex-mcp-server"></a>

### `codex mcp-server`

`codex mcp-server` 命令和独立的 `codex-mcp-server` 二进制文件已被删除。请改用 [Codex 应用服务器](app-server.zh-CN.md)。

<a id="codex-resume"></a>

### `codex resume`

按 ID 继续交互式会话或恢复最近的聊天。 `codex resume` 将 `--last` 的范围限定为当前工作目录，除非您传递 `--all`。它接受与 `codex` 相同的全局标志，包括模型和沙箱覆盖。

如果当前工作目录与会话的保存目录不同，Codex 会询问使用哪个目录。将 [`tui.resume_cwd`](config-file/config-reference.zh-CN.md) 设置为 `"current"` 或 `"session"` 以在没有提示的情况下重复使用该选择。显式 `--cd` (`-C`) 覆盖优先于 `tui.resume_cwd`。

<ConfigTable client:load options={resumeOptions} />

<a id="codex-fork"></a>

### `codex fork`

将先前的交互式会话分叉为新的聊天。默认情况下，`codex fork` 打开会话选择器；添加 `--last` 来分叉您最近的会话。

当当前和保存的会话目录不同时，`codex fork` 使用与 `codex resume` 相同的工作目录提示和 `tui.resume_cwd` 设置。

<ConfigTable client:load options={forkOptions} />

<a id="codex-sandbox"></a>

### `codex sandbox`

使用沙箱帮助程序在 Codex 内部使用的相同策略下运行命令。

<a id="macos-seatbelt"></a>

#### macOS 安全带

<ConfigTable client:load options={sandboxMacOptions} />

<a id="linux-landlock"></a>

#### Linux 内锁

<ConfigTable client:load options={sandboxLinuxOptions} />

<a id="windows"></a>

#### 窗户

<ConfigTable client:load options={sandboxWindowsOptions} />

<a id="codex-update"></a>

### `codex update`

当安装的版本支持自我更新时，检查并应用 Codex CLI 更新。调试版本会打印一条消息，告诉您安装发布版本。

<a id="flag-combinations-and-safety-tips"></a>

## 标志组合和安全提示

- 使用 `--sandbox workspace-write` 进行可以留在工作区中的无人值守本地工作，并避免使用 `--dangerously-bypass-approvals-and-sandbox`，除非您位于专用沙箱 VM 内。
- 当您需要授予 Codex 对更多目录的写访问权限时，优先选择 `--add-dir` 而不是强制 `--sandbox danger-full-access`。
- 在 CI 中将 `--json` 与 `--output-last-message` 配对，以捕获机器可读的进度和最终的自然语言摘要。

<a id="interactive-shortcuts"></a>

## 交互式快捷键

- 键入 `@` 在工作区中搜索文件并将其路径添加到提示中。
- 新闻<kbd>Up</kbd>或<kbd>Down</kbd>恢复草稿历史记录。
- 新闻<kbd>Ctrl</kbd>+<kbd>R</kbd>搜索提示历史记录，然后按<kbd>Enter</kbd>使用匹配或<kbd>Esc</kbd>取消。
- 新闻<kbd>Ctrl</kbd>+<kbd>O</kbd>或运行 `/copy` 复制最新完成的 Codex 输出。
- 在行中添加 `!` 前缀，以在当前批准和沙箱设置下运行本地 shell 命令。
- 新闻<kbd>Tab</kbd>而 Codex 正在为下一轮排队后续提示、斜线命令或 shell 命令。
- 新闻<kbd>Enter</kbd>而 Codex 正在努力将新指令注入当前回合。
- 新闻<kbd>Esc</kbd>使用空输入框两次编辑先前的用户消息并从该点分叉聊天。
- 新闻<kbd>Ctrl</kbd>+<kbd>C</kbd>或运行 `/exit` 关闭会话。

<a id="related-resources"></a>

## 相关资源

- [Codex CLI 概述](codex/cli.zh-CN.md)：安装、升级和快速提示。
- [配置基础知识](config-file/config-basic.zh-CN.md)：保留模型和提供程序等默认值。
- [高级配置](config-file/config-advanced.zh-CN.md)：配置文件、提供程序、沙箱调整和集成。
- [AGENTS.md 自定义指令](agent-configuration/agents-md.zh-CN.md)：Codex 智能体功能和最佳实践的概念概述。

斜杠命令让您可以通过键盘快速控制 Codex。在编辑器中输入 `/` 以打开斜杠弹出窗口，选择命令，Codex 将执行切换模型、调整权限或总结长聊天等操作，而无需离开终端。

本指南向您展示如何：

- 为任务找到正确的内置斜杠命令
- 使用 `/model`、`/fast`、`/personality`、`/permissions`、`/approve`、`/raw`、`/agent` 和 `/status` 等命令引导活动会话

<a id="built-in-slash-commands"></a>

## 内置斜杠命令

Codex 附带以下命令。打开斜杠弹出窗口并开始输入命令名称以过滤列表。

当聊天已经在运行时，您可以键入斜线命令并按 `Tab` 将其排队等待下一轮。 Codex 在运行时解析排队的斜杠命令，因此在当前回合结束后会出现命令菜单和错误。在将命令排队之前，斜杠补全仍然有效。

| 命令 | 用途 | 何时使用 |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [`/permissions`](#update-permissions-with-permissions) | 设置 Codex 无需先询问即可执行的操作。                     | 在会话中放宽或收紧批准要求，例如在“自动”和“只读”之间切换。          |
| [`/ide`](#include-ide-context-with-ide) | 包括打开的文件、当前选择和其他 IDE 上下文。   | 将编辑器上下文拉入下一个提示，而无需重新解释 IDE 中打开的内容。                    |
| [`/keymap`](#remap-tui-shortcuts-with-keymap) | 重新映射 TUI 键盘快捷键。                                   | 检查并保留 `config.toml` 中的自定义快捷方式绑定。                                             |
| [`/vim`](#toggle-vim-mode-with-vim) | 为输入框切换 Vim 模式。                               | 在 Vim 正常/插入行为和默认编辑器编辑模式之间切换。                           |
| [`/setup-default-sandbox`](#set-up-the-elevated-windows-sandbox-with-setup-default-sandbox) | 设置提升的智能体沙箱（仅限 Windows）。               | 在 Codex 提供提升的设置后替换降级的 Windows 沙箱。                                |
| [`/sandbox-add-read-dir`](#grant-sandbox-read-access-with-sandbox-add-read-dir) | 授予沙箱对额外目录的读取权限（仅限 Windows）。 | 取消阻止需要读取当前可读根目录之外的绝对目录路径的命令。          |
| [`/agent`、`/subagents`](#switch-agent-threads-with-agent) | 切换活动智能体线程。                                 | 检查或继续在生成的子智能体线程中工作。                                                     |
| [`/apps`](#browse-apps-with-apps) | 浏览应用程序（连接器）并将其插入提示中。      | 在要求 Codex 使用应用程序之前，将其附加为 `$app-slug`。                                                |
| [`/plugins`](#browse-plugins-with-plugins) | 浏览已安装和可发现的插件。                      | 检查插件工具、安装建议的插件或管理插件可用性。                            |
| [`/hooks`](#view-and-manage-lifecycle-hooks-with-hooks) | 查看和管理生命周期挂钩。                                | 检查配置的挂钩、信任新的或更改的挂钩，或者在运行之前禁用非托管挂钩。        |
| [`/clear`](#clear-the-terminal-and-start-a-new-chat-with-clear) | 清除终端并开始新的聊天。                      | 当您想要重新开始时，请一起重置可见 UI 和聊天上下文。                                |
| [`/rename`](#rename-the-current-chat-with-rename) | 重命名当前聊天。                                        | 为保存的会话指定一个可识别的名称，而无需离开 TUI。                                          |
| [`/archive`](#archive-the-current-session-with-archive) | 存档当前会话并退出 Codex。                     | 从活动会话列表中删除当前会话，而不删除其记录。                      |
| [`/delete`](#delete-the-current-session-with-delete) | 永久删除当前会话并退出 Codex。          | 当存档不够时删除转录本和后代会话。                                 |
| [`/compact`](#keep-transcripts-lean-with-compact) | 将可见聊天汇总为免费Token。                      | 在长时间运行后使用，以便 Codex 保留关键点而不会破坏上下文窗口。                        |
| [`/copy`](#copy-the-latest-response-with-copy) | 复制最新完成的 Codex 输出。                         | 获取最新完成的响应或计划文本，无需手动选择。您也可以按 `Ctrl+O`。 |
| [`/diff`](#review-changes-with-diff) | 显示 Git 差异，包括 Git 尚未跟踪的文件。      | 在提交或运行测试之前检查 Codex 的编辑。                                                       |
| [`/exit`](#exit-the-cli-with-quit-or-exit) | 退出 CLI（与 `/quit` 相同）。                                 | 替代拼写；这两个命令都退出会话。                                                      |
| [`/experimental`](#toggle-experimental-features-with-experimental) | 切换实验功能。                                   | 启用网络代理或防止运行时睡眠等选项。                                       |
| [`/approve`](#approve-an-auto-review-denial-with-approve) | 批准最近一次自动审核拒绝的重试。               | 重试自动审阅器拒绝的命令或操作。                                                   |
| [`/memories`](#configure-memories-with-memories) | 配置内存使用和生成。                            | 无需离开 TUI，即可打开或关闭内存注入或内存生成。                              |
| [`/skills`](#use-skills-with-skills) | 浏览和使用技巧。                                          | 通过选择相关的本地技能来改善特定于任务的行为。                                        |
| [`/import`](#import-claude-code-or-cursor-setup-with-import) | 导入 Claude 代码或光标设置、项目和聊天。        | 将支持的外部智能体工件迁移到 Codex 配置和本地文件中。                       |
| [`/feedback`](#send-feedback-with-feedback) | 将日志发送给 Codex 维护者。                             | 报告问题或与支持人员共享诊断结果。                                                           |
| [`/init`](#generate-agentsmd-with-init) | 在当前目录生成`AGENTS.md`脚手架。      | 捕获您正在使用的仓库或子目录的持久指令。 |
| [`/logout`](#sign-out-with-logout) | 注销Codex。                                              | 使用共享计算机时清除本地凭据。                                                       |
| [`/mcp`](#list-mcp-tools-with-mcp) | 列出配置的模型上下文协议 (MCP) 工具。             | 检查Codex在会话期间可以调用哪些外部工具；添加 `verbose` 以获取服务器详细信息。            |
| [`/mention`](#highlight-files-with-mention) | 将文件附加到聊天中。                                      | 将 Codex 指向您希望接下来检查的特定文件或文件夹。                                      |
| [`/model`](#set-the-active-model-with-model) | 选择活动模型（以及推理工作，如果可用）。 | 运行任务前在`gpt-5.6-luna`和`gpt-5.6-terra`等模型之间切换。                    |
| [`/fast`](#toggle-fast-mode-with-fast) | 当模型目录公开快速服务层时切换快速服务层。  | 打开或关闭当前模型的快速层并保留选择。                                    |
| [`/plan`](#switch-to-plan-mode-with-plan) | 切换到计划模式并可选择发送提示。               | 在实施工作开始之前要求Codex提出执行计划。                                  |
| [`/goal`](#set-or-view-a-task-goal-with-goal) | 设置、编辑、暂停、恢复、查看或清除任务目标。           | 为 Codex 提供一个持久目标，以便在较大的任务运行时进行跟踪。                                          |
| [`/personality`](#set-a-communication-style-with-personality) | 选择响应的通信方式。                     | 使 Codex 更简洁、更具解释性或更具协作性，而无需更改您的说明。       |
| [`/ps`](#check-background-terminals-with-ps) | 显示后台终端及其最近的输出。              | 检查长时间运行的命令而不留下主要记录。                                           |
| [`/stop`](#stop-background-terminals-with-stop) | 停止所有后台终端。                                  | 取消当前会话启动的后台终端工作。                                            |
| [`/fork`](#fork-the-current-chat-with-fork) | 将当前聊天分叉为新聊天。                          | 分支活动会话以探索新方法而不丢失当前成绩单。                 |
| [`/app`](#continue-in-the-desktop-app-with-app) | 在 ChatGPT 桌面应用程序中继续当前会话。        | 从 TUI 移动到 macOS 或 Windows 上的桌面应用程序。                                                  |
| [`/side`、`/btw`](#start-a-side-chat-with-side) | 开始短暂的侧聊。                                   | 在不中断主要聊天记录的情况下询问有针对性的后续行动。                                     |
| [`/raw`](#toggle-raw-scrollback-with-raw) | 切换原始回滚模式。                                     | 在查看长输出时减少终端选择和复制的格式。                            |
| [`/resume`](#resume-a-saved-chat-with-resume) | 从会话列表中恢复已保存的聊天。                     | 继续之前的 CLI 会话的工作，无需重新开始。                                           |
| [`/new`](#start-a-new-chat-with-new) | 在同一 CLI 会话中开始新聊天。                   | 当您想要在同一仓库中获得新提示时，无需离开 CLI 即可重置聊天上下文。              |
| [`/quit`](#exit-the-cli-with-quit-or-exit) | 退出 CLI。                                                   | 立即离开会话。                                                                             |
| [`/review`](#ask-for-a-working-tree-review-with-review) | 要求 Codex 检查您的工作树。                          | 在 Codex 完成工作后或当您想要第二次关注本地更改时运行。                     |
| [`/status`](#inspect-the-session-with-status) | 显示会话配置和令牌使用情况。                  | 确认活动模型、审批策略、可写根和剩余上下文容量。                 |
| [`/usage`](#view-account-usage-with-usage) | 查看帐户令牌使用情况或使用速率限制重置。             | 从 TUI 内部检查每日、每周或累计 ChatGPT Token活动。                           |
| [`/debug-config`](#inspect-config-layers-with-debug-config) | 打印配置层和需求诊断。                | 调试优先级和策略要求，包括实验网络限制。                      |
| [`/statusline`](#configure-footer-items-with-statusline) | 交互配置 TUI 状态行字段。                 | 选择并重新排序页脚项目 (model/context/limits/git/tokens/session) 并保留在 config.toml 中。        |
| [`/title`](#configure-terminal-title-items-with-title) | 交互式配置终端窗口或选项卡标题字段。    | 选择并重新排序标题项，例如项目、状态、线程、分支、模型和任务进度。            |
| [`/theme`](#choose-a-syntax-theme-with-theme) | 选择语法突出显示主题。                             | 预览并保留终端语法突出显示主题。                                                  |
| [`/pets`、`/pet`](#choose-a-terminal-pet-with-pets) | 选择或隐藏终端宠物。                                  | 使用内置或自定义环境宠物来个性化 TUI。                                                 |

`/quit` 和 `/exit` 均退出 CLI。仅在保存或提交任何重要工作后才使用它们。

使用 `/permissions` 调整 Codex 可以做什么，无需先询问。仅当您需要重试自动审核拒绝的最近操作时才使用 `/approve`。

<a id="control-your-session-with-slash-commands"></a>

## 使用斜杠命令控制您的会话

以下工作流程使您的会话保持在正轨上，而无需重新启动 Codex。

<a id="set-the-active-model-with-model"></a>

### 使用 `/model` 设置活动模型

1. 启动Codex并打开composer。
2. 输入 `/model` 并按 Enter 键。
3. 从弹出窗口中选择一个模型，例如 `gpt-5.6-luna` 或 `gpt-5.6-terra`。

预期：Codex 确认了成绩单中的新模型。运行 `/status` 以验证更改。

<a id="toggle-fast-mode-with-fast"></a>

### 使用 `/fast` 切换快速模式

1. 键入 `/fast` 打开当前模型的快速服务层。
2. 再次输入 `/fast` 将其关闭。

预期：Codex 切换层并保存选择。在 TUI 页脚中，您还可以使用 `/statusline` 显示快速模式状态行项目。

快速层命令是目录驱动的。如果当前模型未宣传快速层，Codex 将不会显示 `/fast`。

<a id="set-a-communication-style-with-personality"></a>

### 使用 `/personality` 设置沟通方式

使用 `/personality` 更改 Codex 的通信方式，而无需重写提示。

1. 在活动聊天中，输入 `/personality` 并按 Enter。
2. 从弹出窗口中选择一种样式。

预期：Codex 确认记录中的新样式并将其用于聊天中的后续响应。

Codex 支持 `friendly`、`pragmatic` 和 `none` 个性。使用 `none` 禁用个性指令。

如果活动模型不支持个性特定指令，Codex 会隐藏此命令。

<a id="switch-to-plan-mode-with-plan"></a>

### 使用 `/plan` 切换到计划模式

1. 输入 `/plan` 并按 Enter 将活动聊天切换到计划模式。
2. 可选：提供内联提示文本（例如，`/plan Propose a migration plan for this service`）。
3. 您可以在使用内联 `/plan` 参数时粘贴内容或附加图像。

预期：Codex 进入计划模式并使用可选的内联提示作为第一个计划请求。

虽然 Codex 已经可以工作，但 `/plan` 暂时不可用。

<a id="set-or-view-a-task-goal-with-goal"></a>

### 使用 `/goal` 设置或查看任务目标

1. 类型`/goal<objective>` to set the goal, for example `/goal 完成迁移并保持测试绿色`。
2. 输入 `/goal` 查看当前目标。
3. 使用 `/goal edit` 修改目标。使用 `/goal pause`、`/goal resume` 或 `/goal clear` 暂停、恢复或删除它。

预期：Codex 在工作继续的同时将目标附加到活动聊天中。

目标必须非空且最多 4,000 个字符。对于较长的说明，请将详细信息放入文件中并将目标指向该文件。

<a id="toggle-experimental-features-with-experimental"></a>

### 使用 `/experimental` 切换实验功能

1. 输入 `/experimental` 并按 Enter 键。
2. 切换您想要的功能（例如，网络代理或防止运行时睡眠），然后如果提示要求您重新启动 Codex。

预期：Codex 将您的功能选择保存到配置并在重新启动时应用它们。

<a id="approve-an-auto-review-denial-with-approve"></a>

### 使用 `/approve` 批准自动审核拒绝

当自动审阅器拒绝最近的操作并且您希望 Codex 重试一次时，请使用 `/approve`。

1. 型号 `/approve`。
2. 当Codex显示相关拒绝操作时确认重试。

预期：Codex 在当前会话策略下重试拒绝操作一次。

<a id="configure-memories-with-memories"></a>

### 使用 `/memories` 配置存储器

1. 型号 `/memories`。
2. 选择 Codex 是否应使用现有存储器、生成新存储器或禁用存储器行为。

预期：Codex 更新未来会话的相关内存设置。

<a id="use-skills-with-skills"></a>

### 使用`/skills`的技巧

1. 型号 `/skills`。
2. 选择您想要 Codex 应用的技能。

预期：Codex 插入选定的技能上下文，以便下一个请求遵循该技能的指令。

<a id="import-claude-code-configuration-with-import"></a>
<a id="import-claude-code-setup-with-import"></a>
<a id="cli-import-claude-code-setup-with-import"></a>

<a id="import-claude-code-or-cursor-setup-with-import"></a>

### 使用 `/import` 导入 Claude 代码或光标设置

1. 型号 `/import`。
2. 选择**克劳德·科德**或**光标**。
3. 选择要迁移的设置、项目文件或最近的聊天记录。

预期：Codex 打开外部智能体导入选择器，并将选定的受支持工件导入到 Codex 配置和本地文件中。会话发现包括过去 30 天内最多 50 条聊天。

从本地 TUI 会话运行 `/import`。当任务正在运行、在远程会话中以及连接到本地应用程序服务器守护程序时，它不可用。

有关桌面应用程序工作流程和支持的工件类型，请参阅 [从其他智能体进口](import.zh-CN.md)。

<a id="clear-the-terminal-and-start-a-new-chat-with-clear"></a>
<a id="clear-the-terminal-and-start-a-new-task-with-clear"></a>

<a id="clear-the-terminal-and-start-a-new-chat-with-clear"></a>

### 清除终端并开始与 `/clear` 的新聊天

1. 输入 `/clear` 并按 Enter 键。

预期：Codex 清除终端，重置可见记录，并在同一 CLI 会话中开始新的聊天。

要在创建新聊天时为其命名，请运行 `/clear release prep`。

不像<kbd>Ctrl</kbd>+<kbd>L</kbd>, `/clear` 开始新的聊天。

<kbd>Ctrl</kbd>+<kbd>L</kbd>仅清除终端视图并保留当前聊天。 Codex 在任务正在进行时禁用这两个操作。

<a id="archive-the-current-session-with-archive"></a>

### 使用 `/archive` 存档当前会话

1. 输入 `/archive` 并按 Enter 键。
2. 确认您要存档当前会话并退出 Codex。

预期：Codex 存档当前会话并关闭交互式 TUI。 Codex 将会话记录保存在本地；稍后使用“codex unarchive”恢复它<SESSION>`.

任务正在运行时，`/archive` 不可用。

<a id="delete-the-current-session-with-delete"></a>

### 删除当前与 `/delete` 的会话

1. 输入 `/delete` 并按 Enter 键。
2. 确认您要删除当前会话并退出 Codex。

预期：Codex 删除当前会话记录并关闭交互式 TUI。删除是永久性的，并且还会删除生成的后代会话。

当聊天正在运行或处于侧聊状态时，`/delete` 不可用。

<a id="update-permissions-with-permissions"></a>

### 使用 `/permissions` 更新权限

1. 输入 `/permissions` 并按 Enter 键。
2. 选择与您的舒适度相匹配的批准预设，例如用于免提运行的 `Auto` 或用于审核编辑的 `Read Only`。当命名权限配置文件处于活动状态时，选择器还会显示配置的自定义配置文件及其描述。

预计：Codex 公布更新政策。未来的操作将遵循更新后的批准模式，直到您再次更改为止。

<a id="include-ide-context-with-ide"></a>

### 使用 `/ide` 包含 IDE 上下文

1. 型号 `/ide`。
2. 如果您想解释 Codex 应如何处理当前 IDE 选择或打开的文件，请添加可选的内联文本。

预期：Codex 在下一个提示中包含可用的 IDE 上下文。

<a id="toggle-vim-mode-with-vim"></a>

### 使用 `/vim` 切换 Vim 模式

1. 型号 `/vim`。
2. 继续在输入框中编辑。

预期：Codex 切换当前会话的 Composer Vim 模式。要使 Vim 模式成为新会话的默认模式，请在 `config.toml` 中设置 `tui.vim_mode_default = true`。

<a id="set-up-the-elevated-windows-sandbox-with-setup-default-sandbox"></a>

### 使用 `/setup-default-sandbox` 设置提升的 Windows 沙箱

当 Codex 使用降级的受限令牌沙箱时，此命令仅出现在 Windows 上。

1. 型号 `/setup-default-sandbox`。
2. 按照管理员设置流程进行操作。

预期：Codex 配置提升的 Windows 沙箱并选择相应的自动批准预设。

<a id="copy-the-latest-response-with-copy"></a>

### 复制最新回复 `/copy`

1. 输入 `/copy` 并按 Enter 键。

预期：Codex 将最新完成的 Codex 输出复制到剪贴板。

如果回合仍在运行，`/copy` 使用最新完成的输出而不是正在进行的响应。在第一次完成 Codex 输出之前和回滚之后，该命令不可用。

您还可以按<kbd>Ctrl</kbd>+<kbd>O</kbd>从主 TUI 复制最新完成的响应，而无需打开斜杠命令菜单。

<a id="toggle-raw-scrollback-with-raw"></a>

### 使用 `/raw` 切换原始回滚

1. 类型 `/raw`、`/raw on` 或 `/raw off`。

预期：Codex 切换原始回滚模式，这使得终端选择和复制更加直接。您也可以使用默认的<kbd>Alt</kbd>+<kbd>R</kbd>
使用 `tui.raw_output_mode = true` 绑定或保留默认值。

<a id="grant-sandbox-read-access-with-sandbox-add-read-dir"></a>

### 使用 `/sandbox-add-read-dir` 授予沙箱读取权限

仅当在 Windows 上本机运行 CLI 时，此命令才可用。

1. 输入 `/sandbox-add-read-dir C:\absolute\directory\path` 并按 Enter 键。
2. 确认路径是现有的绝对目录。

预期：Codex 刷新 Windows 沙箱策略并为沙箱中运行的后续命令授予对该目录的读取访问权限。

<a id="inspect-the-session-with-status"></a>

### 使用 `/status` 检查会话

1. 在任何聊天中，输入 `/status`。
2. 查看活动模型、审批策略、可写根和当前令牌使用情况的输出。当 TUI 远程连接时，输出还会显示远程地址和服务器版本。

预期：Codex 打印一份摘要，确认其运行符合您的预期。

<a id="view-account-usage-with-usage"></a>

### 使用 `/usage` 查看帐户使用情况

1. 输入 `/usage` 打开使用菜单。
2. 选择是否显示Token活动或兑换可用的赚取重置。
3. 要直接打开Token活动，请输入 `/usage daily`、`/usage weekly` 或 `/usage cumulative`。

预期：Codex 打开使用操作或显示所选视图的帐户令牌活动。如果会话没有 Codex 服务帐户身份验证，Codex 将显示登录要求。

<a id="inspect-config-layers-with-debug-config"></a>

### 使用 `/debug-config` 检查配置层

1. 型号 `/debug-config`。
2. 查看配置层顺序（优先级最低的优先）、开/关状态和策略源的输出。

预期：Codex 在配置时打印层诊断以及策略详细信息，例如 `allowed_approval_policies`、`allowed_sandbox_modes`、`mcp_servers`、`rules` 和 `experimental_network`。

使用此输出来调试有效设置与 `config.toml` 不同的原因。

<a id="configure-footer-items-with-statusline"></a>

### 使用 `/statusline` 配置页脚项目

1. 型号 `/statusline`。
2. 使用选择器切换和重新排序项目，然后确认。

预期：页脚状态行立即更新并持续显示为 `config.toml` 中的 `tui.status_line`。

可用的状态行项目包括模型、模型+推理、上下文统计、速率限制、git 分支、令牌计数器、会话 ID、当前目录/项目根和 Codex 版本。

<a id="configure-terminal-title-items-with-title"></a>

### 使用 `/title` 配置终端标题项

1. 型号 `/title`。
2. 使用选择器切换和重新排序项目，然后确认。

预期：终端窗口或选项卡标题立即更新并持续为 `config.toml` 中的 `tui.terminal_title`。

可用的标题项包括应用程序名称、项目、微调器、状态、线程、git 分支、模型和任务进度。

<a id="choose-a-syntax-theme-with-theme"></a>

### 使用 `/theme` 选择语法主题

1. 型号 `/theme`。
2. 从选择器中预览主题，然后确认。

预期：Codex 更新语法突出显示并将选择保留为 `config.toml` 中的 `tui.theme`。

<a id="choose-a-terminal-pet-with-pets"></a>

### 使用`/pets`选择末期宠物

1. 输入 `/pets`（或 `/pet`）以打开宠物选择器。
2. 选择内置或自定义宠物，或关闭宠物。

预期：Codex 在支持的终端中显示选定的环境宠物并保留选择。您还可以键入 `/pets off` 将其隐藏。

<a id="remap-tui-shortcuts-with-keymap"></a>

### 使用 `/keymap` 重新映射 TUI 快捷方式

使用 `/keymap` 检查、更新和保留 TUI 的键盘快捷键绑定。

1. 型号 `/keymap`。
2. 选择要更改的快捷方式上下文和操作。
3. 输入新绑定或删除现有绑定。

预期：Codex 更新活动键盘映射并将自定义绑定写入 `config.toml` 中的 `tui.keymap`。

键绑定使用 `ctrl-a`、`shift-enter` 和 `page-down` 等名称。上下文特定的绑定覆盖 `tui.keymap.global`；空的绑定列表会解除该操作的绑定。

<a id="check-background-terminals-with-ps"></a>

### 使用`/ps`检查后台终端

1. 型号 `/ps`。
2. 查看后台终端列表及其状态。

预期：Codex 显示每个后台终端的命令以及最多三个最近的非空输出行，以便您可以一目了然地衡量进度。

使用`unified_exec`时出现后台终端；否则，列表可能为空。

<a id="stop-background-terminals-with-stop"></a>

### 使用 `/stop` 停止后台终端

1. 型号 `/stop`。
2. 在停止列出的终端之前确认 Codex 是否询问。

预期：Codex 停止当前会话的所有后台终端。 `/clean` 仍可用作 `/stop` 的别名。

<a id="keep-transcripts-lean-with-compact"></a>

### 使用 `/compact` 保持成绩单精简

1. 长时间交换后，输入 `/compact`。
2. 确认 Codex 何时提出总结迄今为止的聊天内容。

预期：Codex 用简洁的摘要替换之前的内容，释放上下文，同时保留关键细节。

<a id="review-changes-with-diff"></a>

### 使用 `/diff` 查看更改

1. 输入 `/diff` 来检查 Git diff。
2. 滚动浏览 CLI 内的输出以查看编辑和添加的文件。

预期：Codex 显示您已暂存的更改、尚未暂存的更改以及 Git 尚未开始跟踪的文件，以便您可以决定保留哪些内容。

<a id="highlight-files-with-mention"></a>

### 使用 `/mention` 突出显示文件

1. 键入 `/mention`，后跟路径，例如 `/mention src/lib/api.ts`。
2. 从弹出窗口中选择匹配结果。

预期：Codex 将文件添加到聊天中，确保后续回合直接引用它。

<a id="start-a-new-conversation-with-new"></a>

<a id="start-a-new-chat-with-new"></a>

### 与 `/new` 开始新的聊天

1. 输入 `/new` 并按 Enter 键。

预期：Codex 在同一 CLI 会话中开始新的聊天，因此您无需离开终端即可切换聊天。

要在创建新聊天时为其命名，请运行 `/new bug bash`。

与 `/clear` 不同，`/new` 不会首先清除当前终端视图。

<a id="rename-the-current-chat-with-rename"></a>
<a id="rename-the-current-task-with-rename"></a>

<a id="rename-the-current-chat-with-rename"></a>

### 将当前聊天重命名为 `/rename`

1. 类型`/rename<name>`, or type `/rename` 打开命名提示。
2. 输入一个简短的名称，以帮助您稍后找到聊天。

预期：Codex 更新保存的聊天名称而不更改其记录。

<a id="resume-a-saved-conversation-with-resume"></a>

<a id="resume-a-saved-chat-with-resume"></a>

### 恢复与 `/resume` 保存的聊天

1. 输入 `/resume` 并按 Enter 键。
2. 从保存的会话选择器中选择您想要的会话。

预期：Codex 重新加载所选聊天记录，以便您可以从上次中断的地方继续，保持原始历史记录完好无损。

<a id="fork-the-current-conversation-with-fork"></a>

<a id="fork-the-current-chat-with-fork"></a>

### 分叉当前与 `/fork` 的聊天

1. 输入 `/fork` 并按 Enter 键。

预期：Codex 将当前聊天克隆到具有新 ID 的新聊天中，保持原始记录不变，以便您可以并行探索替代方法。

如果您需要分叉已保存的会话而不是当前会话，请在终端中运行 `codex fork` 以打开会话选择器。

<a id="continue-in-the-desktop-app-with-app"></a>

### 使用 `/app` 在桌面应用程序中继续

在 macOS 和 Windows 上，键入 `/app` 以在 ChatGPT 桌面应用程序中打开当前会话。如果该应用程序未安装或运行，Codex 将显示错误，要求您安装或启动它。

预期：桌面应用程序会打开相同的已保存聊天记录，以便您可以继续。

<a id="start-a-side-conversation-with-side"></a>

<a id="start-a-side-chat-with-side"></a>

### 与 `/side` 开始侧聊

使用 `/side` 从当前聊天启动临时分叉，而无需离开主聊天。

1. 输入 `/side` 打开侧聊。
2. 可以选择添加内联文本，例如 `/side Check whether this plan has an obvious risk`。
3. 重点绕行结束后返回父聊天室。

预期：Codex 打开一个侧聊天，其记录与父聊天是分开的。当您处于侧面模式时，TUI 会继续显示父聊天的状态，以便您可以查看主聊天是否仍在运行。

`/side` 在另一侧聊天中和审阅模式下不可用。

<a id="generate-agentsmd-with-init"></a>

### 使用 `/init` 生成 `AGENTS.md`

1. 在您希望 Codex 查找持久指令的目录中运行 `/init`。
2. 查看生成的 `AGENTS.md`，然后对其进行编辑以匹配您的仓库约定。

预期：Codex 创建一个 `AGENTS.md` 支架，您可以改进并提交给未来的会话。

<a id="ask-for-a-working-tree-review-with-review"></a>

### 请求 `/review` 进行工作树审查

1. 型号 `/review`。
2. 如果您想检查确切的文件更改，请跟进 `/diff`。

预期：Codex 总结了它在工作树中发现的问题，重点关注行为变化和缺失的测试。除非您在 `config.toml` 中设置 `review_model`，否则它将使用当前会话模型。

<a id="list-mcp-tools-with-mcp"></a>

### 列出 MCP 工具和 `/mcp`

1. 型号 `/mcp`。
2. 查看列表以确认哪些 MCP 服务器和工具可用。

预期：您会看到已配置的模型上下文协议 (MCP) 工具 Codex 可以在此会话中调用。

使用 `/mcp verbose` 包含详细的服务器诊断。如果传递 `verbose` 以外的任何内容，Codex 将显示命令用法。

<a id="browse-apps-with-apps"></a>

### 使用 `/apps` 浏览应用程序

1. 型号 `/apps`。
2. 从列表中选择一个应用程序。

预期：Codex 将应用提及作为 `$app-slug` 插入到 Composer 中，因此您可以立即要求 Codex 使用它。

<a id="browse-plugins-with-plugins"></a>

### 使用 `/plugins` 浏览插件

1. 型号 `/plugins`。
2. 选择一个市场选项卡，然后选择一个插件来检查其功能或可用操作。

预期：Codex 打开插件浏览器，以便您可以查看已安装的插件、您的配置允许的可发现插件以及已安装的插件状态。新闻<kbd>Space</kbd>在已安装的插件上切换其启用状态。

<a id="view-and-manage-lifecycle-hooks-with-hooks"></a>

### 使用 `/hooks` 查看和管理生命周期挂钩

1. 型号 `/hooks`。
2. 选择一个挂钩事件来检查匹配的处理程序。
3. 根据需要信任、禁用或重新启用非托管挂钩。

预期：Codex 打开挂钩浏览器，以便您可以查看配置的生命周期挂钩。托管挂钩显示为托管，并且无法从用户挂钩浏览器禁用。

<a id="switch-agent-threads-with-agent"></a>

### 使用 `/agent` 切换智能体线程

1. 输入 `/agent` 或 `/subagents` 并按 Enter。
2. 从选择器中选择您想要的线程。

预期：Codex 切换活动线程，以便您可以检查或继续该智能体的工作。

<a id="send-feedback-with-feedback"></a>

### 使用 `/feedback` 发送反馈

1. 输入 `/feedback` 并按 Enter 键。
2. 按照提示添加日志或诊断信息。

预期：Codex 收集请求的诊断并将其提交给维护人员。

<a id="sign-out-with-logout"></a>

### 使用 `/logout` 注销

1. 输入 `/logout` 并按 Enter 键。

预期：Codex 清除当前用户会话的本地凭据。

<a id="exit-the-cli-with-quit-or-exit"></a>

### 使用 `/quit` 或 `/exit` 退出 CLI

1. 输入 `/quit`（或 `/exit`）并按 Enter。

预期：Codex 立即退出。首先保存或提交任何重要的工作。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

使用这些命令从 VS Code 命令面板控制 Codex。您还可以将它们绑定到键盘快捷键。

<a id="assign-a-key-binding"></a>

## 分配键绑定

要分配或更改 Codex 命令的键绑定：

1. 打开命令面板（macOS 上为 **Cmd+Shift+P** 或 Windows/Linux 上为 **Ctrl+Shift+P**）。
2. 运行**首选项：打开键盘快捷键**。
3. 搜索 `Codex` 或命令 ID（例如 `chatgpt.newChat`）。
4. 选择铅笔图标，然后输入所需的快捷方式。

<a id="extension-commands"></a>

## 扩展命令

| 命令 | 默认按键绑定 | 说明 |
| ------------------------- | ------------------------------------------ | ------------------------------------------------------- |
| `chatgpt.addToThread` | - | 添加选定的文本范围作为当前聊天的上下文 |
| `chatgpt.addFileToThread` | - | 添加整个文件作为当前聊天的上下文 |
| `chatgpt.newChat` | macOS：`Cmd+N`<br />Windows/Linux: `Ctrl+N` | 创建新聊天 |
| `chatgpt.newCodexPanel` | - | 创建新的 Codex 面板 |
| `chatgpt.openCommandMenu` | - | 打开 Codex 命令菜单 |
| `chatgpt.openSidebar` | - | 打开Codex侧边栏面板|

斜线命令让您无需离开编辑器即可控制 Codex。使用它们来检查状态、在本地和云模式之间切换或发送反馈。

<a id="use-a-slash-command"></a>

## 使用斜杠命令

1. 在 Codex 编辑器中，输入 `/`。
2. 从列表中选择一个命令，或继续键入进行过滤（例如，`/status`）。
3. 按 **输入**。

<a id="available-slash-commands"></a>

## 可用的斜杠命令

| 斜杠命令 | 说明 |
| -------------------- | --------------------------------------------------------------------------------------- |
| `/approve` | 当自动审核处于活动状态时，批准最近一次自动审核拒绝的重试。 |
| `/cloud` | 当云执行可用时，在云中运行聊天。                           |
| `/cloud-environment` | 选择聊天的云环境。                                              |
| `/compact` | 压缩当前聊天的上下文。                                                     |
| `/fast` | 打开或关闭目录提供的快速服务层（如果可用）。                    |
| `/feedback` | 打开反馈对话框以提交反馈并可选择包含日志。                |
| `/fork` | 将本地聊天复制到新的本地聊天中。                                                |
| `/goal` | 为Codex 设定一个持续努力的目标。                                         |
| `/ide-context` | 打开或关闭自动 IDE 上下文。                                                   |
| `/init` | 为当前项目生成 `AGENTS.md` 脚手架。                               |
| `/local` | 在本地工作区中运行聊天。                                                   |
| `/mcp` | 打开MCP状态查看连接的服务器。                                              |
| `/memories` | 配置当记忆可用时聊天是否可以使用或生成记忆。    |
| `/model` | 选择当前聊天的模型。                                                  |
| `/personality` | 选择当前模型支持个性时 Codex 如何响应。               |
| `/plan` | 切换多步骤规划的计划模式。                                               |
| `/project` | 选择新聊天的项目。                                                         |
| `/reasoning` | 选择当前聊天的推理工作。                                       |
| `/review` | 启动代码审查模式以审查未提交的更改或与基础分支进行比较。  |
| `/side` | 开始临时侧聊而不中断主聊天。                         |
| `/status` | 显示聊天 ID、上下文使用情况和速率限制。                                       |
| `/worktree` | 在新的 Git 工作树中运行聊天。                                                     |

</ContentModeSwitch>