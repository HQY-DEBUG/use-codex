> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/hooks.md)。

<a id="hooks"></a>

# 生命周期钩子

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Hooks 是 Codex 的可扩展框架。它们允许您在代理循环期间运行脚本或 MCP 工具，从而实现以下功能：

- 将聊天发送到自定义日志记录/分析引擎
- 扫描团队的提示以阻止意外粘贴 API 密钥
- 总结聊天记录，自动创建持久记忆
- 当聊天停止时运行自定义验证检查，执行标准
- 自定义在某个目录时的提示

要记住的运行时行为：

- 来自多个文件的匹配钩子全部运行。
- 同一事件的多个匹配命令挂钩会同时启动，因此一个挂钩无法阻止另一个匹配挂钩的启动。
- 非托管挂钩在运行之前必须经过审查和信任。

钩子在对话中的不同点运行：

| 当|钩住|时
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 对话轮次时 | `PreToolUse`、`PermissionRequest`、`PostToolUse`、`PreCompact`、`PostCompact`、`UserPromptSubmit`、`SubagentStop`、`Stop` |
| 当您中断活动回合时 | `Interrupt`（不为子智能体运行） |
| 当会话或子智能体启动时 | `SessionStart`、`SubagentStart` |
| 当主线程结束时 | `SessionEnd` （不运行子智能体） |

<a id="where-codex-looks-for-hooks"></a>

## Codex 在哪里寻找钩子

Codex 以以下任一形式发现活动配置层旁边的挂钩：

- `hooks.json`
- `config.toml` 内的内联 `[hooks]` 表

安装的插件还可以通过其插件清单或默认的 `hooks/hooks.json` 文件捆绑生命周期配置。插件打包规则参见[构建插件](https://developers.openai.com/plugins/build/plugins#bundled-mcp-servers-and-lifecycle-hooks)。

实际上，四个最有用的位置是：

- `~/.codex/hooks.json`
- `~/.codex/config.toml`
- `<repo>/.codex/hooks.json`
- `<repo>/.codex/config.toml`

如果存在多个钩子源，则 Codex 加载所有匹配的钩子。较高优先级的配置层不会取代较低优先级的挂钩。如果单个层同时包含 `hooks.json` 和内联 `[hooks]`，Codex 会将它们合并并在启动时发出警告。更喜欢每层一个表示。

Codex 还可以发现与启用的插件捆绑在一起的挂钩。插件捆绑的挂钩与其他挂钩源一起加载，并使用与其他非托管挂钩相同的信任审核流程。

仅当项目 `.codex/` 层受信任时，才会加载项目本地挂钩。在不受信任的项目中，Codex 仍然从自己的活动配置层加载用户和系统挂钩。

<a id="review-and-trust-hooks"></a>

## 审查和信任挂钩

Codex 在决定哪些可以运行之前列出已配置的挂钩。在非托管挂钩运行之前，Codex 要求您检查并信任确切的挂钩定义。 Codex 记录针对钩子当前哈希的信任，因此新的或更改的钩子将被标记以供审查并跳过，直到受信任为止。

在 CLI 中使用 `/hooks` 来检查挂钩源、查看新的或更改的挂钩、信任挂钩或禁用单个非托管挂钩。如果钩子需要在启动时检查，Codex 会打印一条警告，告诉您打开 `/hooks`。

来自系统、MDM、云或 `requirements.toml` 源的托管挂钩被标记为托管、受策略信任，并且无法从用户挂钩浏览器禁用。

对于已经审查 Codex 之外的钩子源的一次性自动化，请传递 `--dangerously-bypass-hook-trust` 来运行启用的钩子，而不需要对该调用持久的钩子信任。

<a id="config-shape"></a>

## 配置形状

钩子分为三个级别：

- 挂钩事件，例如 `PreToolUse`、`PostToolUse`、`PreCompact`、`SubagentStart` 或 `Stop`
- 决定事件何时匹配的匹配器组
- 当匹配器组匹配时运行的一个或多个钩子处理程序

```json
{
  "description": "Optional lifecycle hooks for this workspace.",
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume",
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.codex/hooks/session_start.py",
            "statusMessage": "Loading session notes",
            "additionalContextLimit": 5000
          }
        ]
      }
    ],
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.codex/hooks/session_end.py",
            "timeout": 3
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py\"",
            "statusMessage": "Checking Bash command"
          }
        ]
      }
    ],
    "PermissionRequest": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/permission_request.py\"",
            "statusMessage": "Checking approval request"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/post_tool_use_review.py\"",
            "statusMessage": "Reviewing Bash output"
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/user_prompt_submit_data_flywheel.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/stop_continue.py\"",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

注意事项：

- `description` 是 `hooks.json` 文件的可选顶级元数据。它不会改变运行的钩子。
- `timeout` 以秒为单位。
- 如果省略 `timeout`，则 Codex 对大多数挂钩使用 `600` 秒。
  - `SessionEnd`和`Interrupt`默认使用`1`秒，最多支持`3`秒。
- `statusMessage` 是可选的。
- `additionalContextLimit` 设置在 Codex 将全文保存到磁盘并发送较短的预览之前，命令挂钩可以向模型发送多少 `additionalContext`。参见 [大钩输出](#large-hook-output)。
- `commandWindows` 是可选的仅限 Windows 的命令覆盖。在 TOML 中，使用 `command_windows` 或 `commandWindows`。
- 设置 `async` 至 `true` 至 [在后台运行命令挂钩](#run-hooks-in-the-background)。
- 支持 `command` 和 `mcp_tool` 处理程序。 `prompt` 和 `agent` 处理程序被解析但被跳过。
- 命令以会话 `cwd` 作为工作目录运行。
- 对于仓库本地挂钩，更喜欢从 git 根解析，而不是使用相对路径（例如 `.codex/hooks/...`）。 Codex 可以从子目录启动，并且基于 git-root 的路径保持钩子位置稳定。

`config.toml` 中的等效内联 TOML：

```toml
[[hooks.SessionStart]]
matcher = "^compact$"

[[hooks.SessionStart.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/session_start.py"'
additionalContextLimit = 5000

[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"

[[hooks.PostToolUse]]
matcher = "^Bash$"

[[hooks.PostToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/post_tool_use_review.py"'
timeout = 30
statusMessage = "Reviewing Bash output"
```

<a id="mcp-tool-hooks"></a>

## MCP 工具挂钩

MCP 工具挂钩允许生命周期事件调用已连接的 MCP 服务器上的工具。它将结构化参数直接发送到工具，并使用与命令挂钩相同的信任审查和输出契约。

<a id="configure-an-mcp-tool-hook"></a>

### 配置 MCP 工具挂钩

该钩子要求 `scanner` MCP 服务器在 Codex 写入或编辑文件后扫描每个补丁：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "mcp_tool",
            "server": "scanner",
            "tool": "scan_patch",
            "input": { "patch": "${tool_input.command}" },
            "timeout": 30,
            "statusMessage": "Scanning edited files"
          }
        ]
      }
    ]
  }
}
```

| 字段 | 含义 |
| --------------- | ---------------------------------------------------------------- |
| `type` | 必须是 `mcp_tool`。                                              |
| `server` | 已连接的 MCP 服务器的必需名称。                |
| `tool` | 该服务器公开的工具的必需名称。                  |
| `input` | 参数模板的可选 JSON 对象。默认为 `{}`。    |
| `timeout` | 可选的活动执行超时（以秒为单位）。默认为 `600`。 |
| `statusMessage` | 挂钩运行时显示的可选消息。                      |

<a id="expand-arguments-from-the-hook-event"></a>

### 扩展钩子事件的参数

使用 `${field.nested}` 从挂钩事件中读取虚线字段。填充整个值的占位符保留其 JSON 类型。较大字符串内的占位符将呈现为文本。 Codex 递归扩展对象和数组。

对于包含 `{"tool_input":{"file_path":"src/main.rs","count":3}}` 的事件，此参数模板：

```json
{
  "path": "${tool_input.file_path}",
  "count": "${tool_input.count}",
  "message": "Scanning ${tool_input.file_path}"
}
```

变成：

```json
{
  "path": "src/main.rs",
  "count": 3,
  "message": "Scanning src/main.rs"
}
```

<a id="execution-and-lifecycle"></a>

### 执行和生命周期

- 挂钩使用现有的 MCP 连接。他们不会启动或重新连接服务器。
- 当工具返回阻止决策时，挂钩可以阻止操作。错误、丢失的服务器和不可用的工具不会阻止操作。
- MCP工具挂钩同步运行。他们不请求工具批准或触发其他挂钩。
- 适用较短的挂钩或服务器超时。等待 MCP 诱导响应所花费的时间不计入超时。
- `SessionStart` 挂钩可以在 MCP 服务器准备就绪之前运行。如果发生这种情况，他们不会阻止会话。
- `SessionEnd` 不支持 MCP 工具挂钩。

<a id="turn-hooks-off"></a>

## 关闭挂钩

默认情况下启用挂钩。要在 `config.toml` 中关闭它们，请设置：

```toml
[features]
hooks = false
```

使用 `hooks` 作为规范功能密钥。 `codex_hooks` 仍然作为已弃用的别名使用。管理员可以在 `requirements.toml` 和 `[features].hooks = false` 中以相同的方式强制挂钩。

<a id="managed-hooks-from-requirementstoml"></a>

## 来自 `requirements.toml` 的托管挂钩

企业管理的需求还可以在 `[hooks]` 下定义内联挂钩。当管理员想要在通过 MDM 或其他设备管理系统交付实际脚本时强制执行挂钩配置时，这非常有用。要强制执行托管挂钩（甚至对于本地禁用挂钩的用户），请将 `[features].hooks = true` 与 `[hooks]` 一起固定在 `requirements.toml` 中。要忽略用户、项目、会话和插件挂钩，同时仍允许管理员管理挂钩，请设置 `allow_managed_hooks_only = true`。

```toml
allow_managed_hooks_only = true

[features]
hooks = true

[hooks]
managed_dir = "/enterprise/hooks"
windows_managed_dir = 'C:\enterprise\hooks'

[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = "python3 /enterprise/hooks/pre_tool_use_policy.py"
command_windows = 'py -3 C:\enterprise\hooks\pre_tool_use_policy.py'
timeout = 30
statusMessage = "Checking managed Bash command"
```

托管钩子的注意事项：

- `managed_dir` 用于 macOS 和 Linux。
- `windows_managed_dir` 在 Windows 上使用。
- Codex不分发`managed_dir`中的脚本；您的企业工具必须单独安装和更新它们。
- 托管挂钩命令应使用配置的托管目录下的绝对脚本路径。
- `allow_managed_hooks_only = true` 跳过来自用户、项目、会话和插件源的挂钩，但仍然加载来自 `requirements.toml` 和其他托管配置层的托管挂钩。

<a id="plugin-bundled-hooks"></a>

## 插件捆绑钩子

启用插件后，Codex 可以从该插件加载生命周期挂钩以及用户、项目和托管挂钩。

默认情况下，Codex 在插件根目录中查找 `hooks/hooks.json`。插件清单可以使用 `.codex-plugin/plugin.json` 中的 `hooks` 条目覆盖该默认值。清单条目可以是 `./` 前缀的路径、`./` 前缀的路径数组、内联钩子对象或内联钩子对象数组。

```json
{
  "name": "repo-policy",
  "hooks": "./hooks/hooks.json"
}
```

清单挂钩路径是相对于插件根目录解析的，并且必须保留在该根目录内。如果清单定义了 `hooks`，则 Codex 使用这些清单条目而不是默认的 `hooks/hooks.json`。

插件挂钩命令接收这些环境变量：

- `PLUGIN_ROOT` 是 Codex 特定的扩展，指向已安装的插件根目录。
- `PLUGIN_DATA` 是 Codex 特定的扩展，它指向插件的可写数据目录。
- Codex 还设置了 `CLAUDE_PLUGIN_ROOT` 和 `CLAUDE_PLUGIN_DATA` 以与现有插件挂钩兼容。

插件挂钩使用与其他挂钩相同的事件模式。安装或启用插件不会自动信任其挂钩； Codex 会跳过插件捆绑的挂钩，直到您查看并信任当前的挂钩定义。

<a id="matcher-patterns"></a>

## 匹配器模式

`matcher` 字段是一个正则表达式字符串，用于在钩子触发时进行过滤。使用 `"*"`、`""` 或完全省略 `matcher` 以匹配受支持事件的每次出现。

当前只有一些 Codex 事件荣誉 `matcher`：

| 事件 | `matcher` 过滤的内容 | 备注 |
| ------------------- | ---------------------- | ------------------------------------------------------------ |
| `PermissionRequest` | 工具名称 | 支持包括 `Bash`、`apply_patch`\* 和 MCP 工具名称 |
| `PostToolUse` | 工具名称 | 参见 [工具覆盖范围](#tool-coverage) |
| `PostCompact` | 压实触发器 | 值为 `manual` 或 `auto` |
| `PreCompact` | 压实触发器 | 值为 `manual` 或 `auto` |
| `PreToolUse` | 工具名称 | 参见 [工具覆盖范围](#tool-coverage) |
| `SessionEnd` | 结束原因 | 目前仅 `other` |
| `SessionStart` | 启动源 | 值为 `startup`、`resume`、`clear` 和 `compact` |
| `SubagentStart` | 子智能体类型 | 值取决于启动 | 的子智能体
| `SubagentStop` | 子智能体类型 | 值取决于停止 | 的子智能体
| `UserPromptSubmit` 不支持 | | 对于此事件，任何配置的 `matcher` 都会被忽略 |
| `Stop` 不支持 | | 对于此事件，任何配置的 `matcher` 都会被忽略 |
| `Interrupt` 不支持 | | 对于此事件，任何配置的 `matcher` 都会被忽略 |

\* 对于 `apply_patch`、`matcher` 值也可以使用 `Edit` 或 `Write`。

示例：

- `Bash`
- `^apply_patch$`
- `Edit|Write`
- `mcp__filesystem__read_file`
- `mcp__filesystem__.*`
- `startup|resume|clear|compact`
- `manual|auto`

<a id="tool-coverage"></a>

### 工具覆盖范围

`PreToolUse`和`PostToolUse`可以观察到多个shell和MCP的调用。大多数本地函数工具使用相同的挂钩路径，因此您可以匹配它们的工具名称，检查它们的 JSON 参数，并且对于 `PreToolUse`，阻止或重写调用。

| 刀具路径 | `PreToolUse` | `PostToolUse` | 备注 |
| --------------------------------- | ------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Shell 命令 | 是 | 是 | 匹配为 `Bash`。                                                                                                         |
| 统一执行 (`exec_command`) | 是 | 是 | 匹配为 `Bash`。当该命令完成时，稍后的 `write_stdin` 轮询可以传递原始命令的 `PostToolUse`。 |
| `apply_patch` | 是 | 是 | 匹配为 `apply_patch`、`Edit` 或 `Write`。                                                                              |
| MCP 工具 | 是 | 是 | 匹配 MCP 工具名称，例如 `mcp__filesystem__read_file`。                                                           |
| 其他本地功能工具 | 是 | 是 | 匹配功能工具名称，如 `update_plan`。 `spawn_agent` 也匹配 `Agent`。                                 |
| 托管工具，例如 `WebSearch` | 否 | 否 | 这些不使用本地函数工具挂钩路径。                                                                       |

`write_stdin` 是现有统一执行会话的传输。当它发送输入或轮询已经通过 `PreToolUse` 的命令时，它不会再次运行 `PreToolUse`。

一些专用工具路径可以选择退出默认挂钩路径。将工具挂钩视为有用的护栏，而不是完整的强制边界。

<a id="common-input-fields"></a>

## 常用输入字段

每个命令挂钩都会在 `stdin` 上接收一个 JSON 对象。

这些是您通常会使用的共享字段：

| 字段 | 类型 | 含义 |
| ----------------- | ---------------- | ------------------------------------------------------------------- |
| `session_id` | `string` | 当前 Codex 会话 ID。子智能体挂钩使用父会话 ID。 |
| `transcript_path` | `string \| null` | 会话记录文件的路径（如果有） |
| `cwd` | `string` | 会话的工作目录 |
| `hook_event_name` | `string` | 当前挂钩事件名称 |
| `model` | `string` | Codex 专用扩展。活动模型弹头 |

回合范围挂钩在其事件特定表中将 `turn_id` 列为 Codex 特定扩展。

`SessionStart`、`PreToolUse`、`PermissionRequest`、`PostToolUse`、`UserPromptSubmit`、`SubagentStart`、`SubagentStop`、`Stop`和`Interrupt`还包括`permission_mode`，描述当前权限模式为 `default`、`acceptEdits`、`plan`、`dontAsk` 或 `bypassPermissions`。

为了方便起见，`transcript_path` 指向聊天记录，但记录格式不是挂钩的稳定接口，并且可能会随着时间而改变。

如果您需要完整的有线格式，请参阅 [模式](#schemas)。

<a id="common-output-fields"></a>

## 公共输出字段

`SessionStart`、`PreCompact`、`PostCompact`、`UserPromptSubmit`、`SubagentStop` 和 `Stop` 支持这些共享 JSON 字段。 `SubagentStart` 接受 `systemMessage` 和特定于钩子的上下文的相同形状，但 `continue: false` 不会停止子智能体：

```json
{
  "continue": true,
  "stopReason": "optional",
  "systemMessage": "optional",
  "suppressOutput": false
}
```

| 现场 | 效果 |
| ---------------- | ----------------------------------------------- |
| `continue` | 如果是 `false`，则标记该钩子运行已停止 |
| `stopReason` | 记录为停止原因 |
| `systemMessage` | 在 UI 或事件流中作为警告出现 |
| `suppressOutput` | 今天解析但尚未实现 |

退出 `0` 无输出视为成功，Codex 继续。

`PreToolUse` 和 `PermissionRequest` 支持 `systemMessage`，但这些事件当前不支持 `continue`、`stopReason` 和 `suppressOutput`。如果 `PreToolUse` 挂钩返回这些不受支持的字段之一，则 Codex 会将该挂钩运行标记为失败，报告错误并继续工具调用。

`PostToolUse` 支持 `systemMessage`、`continue: false` 和 `stopReason`。 `suppressOutput` 已解析，但当前不支持该事件。

<a id="large-hook-output"></a>

### 大钩输出

默认情况下，Codex 将每个模型可见的挂钩输出消息限制为大约 2,500 个令牌。如果挂钩返回更多内容，则 Codex 将全文保存在 `<temp_dir>/hook_outputs/<session_id>/<uuid>.txt` 并为模型提供带有保存文件路径的头尾预览。此行为称为 **溢出**：Codex 将过大的输出存储在磁盘上，并用较短的模型可见预览替换它。如果无法写入文件，模型仍会收到截断的预览。

保持钩子和插件上下文简洁。来自多个挂钩和插件的上下文加起来可能会降低模型性能。提高 `additionalContextLimit` 会增加这种风险。避免将限制设置为 `0`，除非挂钩强制执行严格的输出上限；否则，单个钩子可以消耗整个上下文窗口。

对于返回 `additionalContext` 的任何命令挂钩，请在处理程序上设置 `additionalContextLimit` 以自定义近似令牌阈值：

```json
{
  "type": "command",
  "command": "python3 ~/.codex/hooks/session_start.py",
  "additionalContextLimit": 5000
}
```

省略 `additionalContextLimit` 以使用默认的 `2500` 令牌阈值。使用正整数选择不同的阈值，或使用 `0` 将处理程序的完整附加上下文直接传递给模型。 Codex 独立评估每个匹配的处理程序。对于无法生成附加上下文的事件，Codex 会忽略 `additionalContextLimit` 并报告配置警告。

该设置仅适用于`additionalContext`。工具反馈和继续提示保持默认限制。

由于超大输出可以写入磁盘，因此请避免在挂钩输出中返回机密或其他敏感数据。

<a id="run-hooks-in-the-background"></a>

## 在后台运行钩子

默认情况下，Codex 等待命令挂钩完成，然后再继续触发它的操作。将 `async` 设置为 `true` 以在 Codex 继续的同时在后台运行命令挂钩。

<a id="configure-a-background-hook"></a>

### 配置后台钩子

将 `"async": true` 添加到 `hooks.json` 中的命令处理程序：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.codex/hooks/post_tool_use.py",
            "async": true,
            "timeout": 120
          }
        ]
      }
    ]
  }
}
```

对于 `config.toml` 中的内联挂钩，设置 `async = true`：

```toml
[[hooks.PostToolUse]]
matcher = "Bash"

[[hooks.PostToolUse.hooks]]
type = "command"
command = "python3 ~/.codex/hooks/post_tool_use.py"
async = true
timeout = 120
```

后台挂钩使用与同步命令挂钩相同的输入、匹配器、信任审查、超时和 [大输出处理](#large-hook-output)。与其他命令挂钩一样，`timeout` 以秒为单位，默认为 `600`。 `Interrupt` 挂钩使用默认值一秒和最大值三秒，包括当它们在后台运行时。

<a id="how-background-hooks-run"></a>

### 后台钩子如何运行

当后台挂钩完成时，Codex 在对话中的下一个安全点提供支持的信息输出：

- 如果回合处于活动状态，则 Codex 等待当前模型请求和工具调用完成，然后使输出可用于该回合中的下一个模型请求。
- 如果没有活动的回合，Codex 会等待，直到下一个用户回合。完成背景挂钩并不会开始新的回合。

使用相同的特定于事件的 JSON 输出作为同步挂钩。 Codex 将 `additionalContext` 添加到模型的上下文中，并曲面 `systemMessage` 作为警告。

后台挂钩无法阻止、批准、重写或以其他方式控制触发它们的操作。使用工具策略、权限决策、提示拒绝或轮流延续的同步挂钩。

<a id="limitations"></a>

### 局限性

- Codex 每个会话最多同时运行 8 个后台挂钩。其他挂钩会等待，直到正在运行的挂钩完成。
- 每个匹配的调用独立运行，并且后台挂钩可以按照与开始时不同的顺序完成。
- 当会话结束时，Codex 取消未完成的后台挂钩并丢弃尚未传送的输出。
- `SessionEnd` 挂钩始终同步运行。

<a id="hooks"></a>

## 挂钩

<a id="sessionstart"></a>

### 会话开始

对于本次活动，`matcher` 适用于 `source`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| -------- | -------- | ------------------------------------------------------------------- |
| `source` | `string` | 会话如何开始：`startup`、`resume`、`clear` 或 `compact` |

`stdout` 上的纯文本被添加为额外的开发人员上下文。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields) 和此钩子特定形状：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "SessionStart",
    "additionalContext": "Load the workspace conventions before editing."
  }
}
```

`additionalContext` 文本被添加为额外的开发人员上下文。

Codex 压缩根会话后，`SessionStart` 与 `source: "compact"` 匹配的挂钩在下一个模型请求之前运行。当自动压缩发生在回合中间时，这也适用：Codex 将钩子的附加上下文传递到立即延续，而不是等待稍后的用户回合。如果钩子返回`continue: false`，则Codex结束回合而不发送另一个模型请求。

<a id="sessionend"></a>

### 会话结束

`SessionEnd` 允许您在会话结束时运行命令，例如保存最终笔记或清理文件。当您归档或删除仍处于打开状态的对话时、当 Codex 正常关闭时、或者当对话处于空闲状态且在任何连接的客户端中 30 分钟未打开时，它会在主线程中运行。它不会为子智能体运行。

退出对话或呼叫 `thread/unsubscribe` 不会立即结束会话，因此不会立即运行 `SessionEnd`。您的钩子在运行时仍然可以读取会话记录。

`matcher` 筛选此事件的 `reason`。目前，`reason` 始终是 `other`。您可以省略 `matcher` 或使用 `other` 在每个 `SessionEnd` 事件上运行。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| -------- | -------- | ------------------------------ |
| `reason` | `string` | 会话结束原因：`other` |

例如，`SessionEnd` 命令接收：

```json
{
  "session_id": "thr_123",
  "transcript_path": "/workspace/.codex/rollout.jsonl",
  "cwd": "/workspace",
  "hook_event_name": "SessionEnd",
  "reason": "other"
}
```

`SessionEnd` 挂钩始终同步运行，即使 `async` 是 `true` 时也是如此。它们是建议性的，因此它们的输出不会引导 Codex 或保持线程打开。如果命令超时或因错误退出，Codex 会将其报告为挂钩失败。

<a id="subagentstart"></a>

### 子代理启动

对于本次活动，`matcher` 适用于 `agent_type`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| ----------------- | -------- | ---------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `agent_id` | `string` | 子智能体 | 的标识符
| `agent_type` | `string` | 子智能体类型或配置文件 |
| `permission_mode` | `string` | 当前权限模式 |

`stdout` 上的纯文本被添加为子智能体的额外开发人员上下文。

`stdout` 上的 JSON 支持 `systemMessage` 和此钩子特定形状：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "SubagentStart",
    "additionalContext": "Review the repository test conventions first."
  }
}
```

该 `additionalContext` 文本将作为子智能体的额外开发人员上下文添加。 `continue: false` 被解析为兼容性，但它不会阻止子智能体启动。

<a id="pretooluse"></a>

### 预工具使用

`PreToolUse`可以拦截Bash、通过`apply_patch`、MCP工具调用执行的文件编辑以及其他本地功能工具。有关支持的路径和例外，请参阅 [工具覆盖范围](#tool-coverage)。

`matcher` 适用于 `tool_name` 和匹配器别名。对于通过 `apply_patch` 进行文件编辑，`matcher` 值可以使用 `apply_patch`、`Edit` 或 `Write`；挂钩输入仍报告 `tool_name: "apply_patch"`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| ------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `tool_name` | `string` | 规范挂钩工具名称，例如 `Bash`、`apply_patch` 或 MCP 名称，例如 `mcp__fs__read` |
| `tool_use_id` | `string` | 此调用的工具调用 ID |
| `tool_input` | `JSON value` | 工具特定输入。 `Bash`和`apply_patch`使用`tool_input.command`。 MCP 和其他本地函数工具发送它们的参数。 |

`stdout` 上的纯文本将被忽略。

`stdout` 上的 JSON 可以使用 `systemMessage`。要拒绝支持的工具调用，请返回此特定于钩子的形状：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "Destructive command blocked by hook."
  }
}
```

Codex 也接受这种旧的块形状：

```json
{
  "decision": "block",
  "reason": "Destructive command blocked by hook."
}
```

您还可以使用退出代码 `2` 并将阻止原因写入 `stderr`。

要添加模型可见上下文而不阻塞，请返回 `hookSpecificOutput.additionalContext`：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "additionalContext": "The pending command touches generated files."
  }
}
```

要重写支持的工具调用而不阻塞，请返回 `permissionDecision: "allow"` 和 `updatedInput`：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "updatedInput": {
      "command": "echo rewritten"
    }
  }
}
```

对于 Bash 命令和 `apply_patch`、`updatedInput` 必须包含字符串 `command` 字段。对于MCP和其他本地函数工具，`updatedInput`是替换参数对象。仅与 `permissionDecision: "allow"` 一起返回 `updatedInput`；其他 `updatedInput` 形状报告为错误。

`permissionDecision: "ask"`、旧版 `decision: "approve"`、`continue: false`、`stopReason` 和 `suppressOutput` 已解析，但尚不支持。 Codex 将钩子运行标记为失败，报告错误，并继续工具调用。

<a id="permissionrequest"></a>

### 许可请求

当 Codex 即将请求批准（例如 shell 升级或托管网络批准）时，`PermissionRequest` 运行。它可以允许请求、拒绝请求或拒绝决定并让正常的批准提示继续。它不会针对不需要批准的命令运行。

`matcher` 适用于 `tool_name` 和匹配器别名。当前规范值包括 `Bash`、`apply_patch` 和 MCP 工具名称，例如 `mcp__server__tool`； `apply_patch` 还匹配 `Edit` 和 `Write`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| ------------------------ | ---------------- | -------------------------------------------------------------------------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `tool_name` | `string` | 规范挂钩工具名称，例如 `Bash`、`apply_patch` 或 MCP 名称，例如 `mcp__fs__read` |
| `tool_input` | `JSON value` | 工具特定输入。 `Bash` 和 `apply_patch` 使用 `tool_input.command`，而 MCP 工具发送所有参数。 |
| `tool_input.description` | `string \| null` | 人类可读的审批原因，当 Codex 有一个 | 时

`stdout` 上的纯文本将被忽略。

某些工具输入可能包括人类可读的描述，但不要依赖每个工具的 `tool_input.description` 字段。

要批准请求，请返回：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow"
    }
  }
}
```

要拒绝请求，请返回：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "deny",
      "message": "Blocked by repository policy."
    }
  }
}
```

如果多个匹配的钩子返回决策，则任何 `deny` 获胜。否则，`allow` 会允许请求继续进行，而不显示批准提示。如果没有决定匹配的挂钩，Codex 将使用正常的审批流程。

不要为 `PermissionRequest` 返回 `updatedInput`、`updatedPermissions` 或 `interrupt`；这些字段是为将来的行为保留的，今天无法关闭。

<a id="posttooluse"></a>

### 后期工具使用

`PostToolUse` 在支持的工具产生输出后运行，包括 Bash、`apply_patch`、MCP 工具调用和其他本地函数工具。对于 Bash，它也在以非零状态退出的命令之后运行。它无法消除已经运行的工具的副作用。有关支持的路径和例外，请参阅 [工具覆盖范围](#tool-coverage)。

`matcher` 适用于 `tool_name` 和匹配器别名。对于通过 `apply_patch` 进行文件编辑，`matcher` 值可以使用 `apply_patch`、`Edit` 或 `Write`；挂钩输入仍报告 `tool_name: "apply_patch"`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| --------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `tool_name` | `string` | 规范挂钩工具名称，例如 `Bash`、`apply_patch` 或 MCP 名称，例如 `mcp__fs__read` |
| `tool_use_id` | `string` | 此调用的工具调用 ID |
| `tool_input` | `JSON value` | 工具特定输入。 `Bash`和`apply_patch`使用`tool_input.command`。 MCP 和其他本地函数工具发送它们的参数。 |
| `tool_response` | `JSON value` | 工具特定输出。 MCP 工具发送 MCP 调用结果。其他本地函数工具通常发送其面向模型的输出。    |

`stdout` 上的纯文本将被忽略。

`stdout` 上的 JSON 可以使用 `systemMessage` 和这个钩子特定的形状：

```json
{
  "decision": "block",
  "reason": "The Bash output needs review before continuing.",
  "hookSpecificOutput": {
    "hookEventName": "PostToolUse",
    "additionalContext": "The command updated generated files."
  }
}
```

`additionalContext` 文本被添加为额外的开发人员上下文。

对于此事件，`decision: "block"` 不会撤消已完成的 Bash 命令。相反，Codex 记录反馈，用该反馈替换工具结果，并从钩子提供的消息继续模型。

您还可以使用退出代码 `2` 并将反馈原因写入 `stderr`。

要在命令运行后停止对原始工具结果的正常处理，请返回 `continue: false`。 Codex 将用您的反馈替换工具结果或停止文本并从那里继续。

`updatedMCPToolOutput` 和 `suppressOutput` 已解析，但尚不支持。 Codex 将挂钩运行标记为失败，报告错误，并继续正常处理工具结果。

<a id="tool-calls-from-code-mode"></a>

#### 从代码模式调用工具

当模型使用代码模式从 JavaScript 调用工具时，挂钩决策将应用于该嵌套调用。 `PreToolUse` 可以在工具运行之前停止该工具或重写其输入。阻止 `PostToolUse` 无法消除该工具的副作用，但它可以阻止原始结果到达正在运行的脚本。

| 挂钩结果 | 什么代码模式看到 |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `PreToolUse` 阻塞 | 工具允许在工具运行之前拒绝。                                                         |
| `PreToolUse` 返回 `updatedInput` | 该工具使用重写的输入运行，并且承诺以该结果解析。                      |
| `PostToolUse` 返回 `decision: "block"` 或退出，代码为 `2` | 工具运行，然后 Promise 因钩子原因而拒绝。                                          |
| `PostToolUse` 返回 `continue: false` | Codex 使用模型可见结果的钩子反馈，但不拒绝嵌套工具承诺。 |

<a id="precompact"></a>

### 预紧凑型

`PreCompact` 在 Codex 压缩聊天之前运行。 `matcher` 适用于 `trigger`，其值为 `manual` 和 `auto`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| --------- | -------- | ---------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `trigger` | `string` | 触发压缩的内容：`manual` 或 `auto` |

`stdout` 上的纯文本将被忽略。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields)。如果匹配的 `PreCompact` 挂钩返回 `continue: false`，则 Codex 在压缩之前停止。

<a id="postcompact"></a>

### 后紧凑型

`PostCompact` 在 Codex 压缩聊天后运行。 `matcher` 适用于 `trigger`，其值为 `manual` 和 `auto`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| --------- | -------- | ---------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `trigger` | `string` | 触发压缩的内容：`manual` 或 `auto` |

`stdout` 上的纯文本将被忽略。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields)。如果匹配的 `PostCompact` 挂钩返回 `continue: false`，则 Codex 在压缩后停止。

<a id="userpromptsubmit"></a>

### 用户提示提交

`matcher` 当前未用于此活动。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| --------- | -------- | ---------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `prompt` | `string` | 即将发送的用户提示 |

`stdout` 上的纯文本被添加为额外的开发人员上下文。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields) 和此钩子特定形状：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "UserPromptSubmit",
    "additionalContext": "Ask for a clearer reproduction before editing files."
  }
}
```

`additionalContext` 文本被添加为额外的开发人员上下文。

要阻止提示，请返回：

```json
{
  "decision": "block",
  "reason": "Ask for confirmation before doing that."
}
```

您还可以使用退出代码 `2` 并将阻止原因写入 `stderr`。

<a id="subagentstop"></a>

### 子代理停止

对于本次活动，`matcher` 适用于 `agent_type`。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| ------------------------ | ---------------- | ----------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `agent_id` | `string` | 子智能体 | 的标识符
| `agent_type` | `string` | 子智能体类型或配置文件 |
| `agent_transcript_path` | `string \| null` | 子智能体转录文件的路径（如果有） |
| `stop_hook_active` | `boolean` | 该子智能体是否已继续 |
| `last_assistant_message` | `string \| null` | 最新子智能体助理消息（如果有） |

当 `SubagentStop` 退出 `0` 时，`stdout` 需要 JSON。纯文本输出对此事件无效。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields)。要要求 Codex 继续子智能体流程，请返回：

```json
{
  "decision": "block",
  "reason": "Run one more focused pass inside the subagent."
}
```

您还可以使用退出代码 `2` 并将继续原因写入 `stderr`。

如果任何匹配的 `SubagentStop` 钩子返回 `continue: false`，则该钩子优先于其他匹配的 `SubagentStop` 钩子的继续决策。

<a id="stop"></a>

### 停止

`matcher` 当前未用于此活动。

[常用输入字段](#common-input-fields) 之外的字段：

| 字段 | 类型 | 含义 |
| ------------------------ | ---------------- | ------------------------------------------------- |
| `turn_id` | `string` | Codex 专用扩展。活动 Codex 对话轮次 ID |
| `stop_hook_active` | `boolean` | 本回合是否已由 `Stop` | 继续
| `last_assistant_message` | `string \| null` | 最新助理消息文本（如果有） |

当 `Stop` 退出 `0` 时，`stdout` 需要 JSON。纯文本输出对此事件无效。

`stdout` 上的 JSON 支持 [公共输出字段](#common-output-fields)。要使 Codex 继续运行，请返回：

```json
{
  "decision": "block",
  "reason": "Run one more pass over the failing tests."
}
```

您还可以使用退出代码 `2` 并将继续原因写入 `stderr`。

对于此事件，`decision: "block"` 不拒绝该回合。相反，它会告诉 Codex 继续并自动创建一个新的继续提示，充当新的用户提示，并使用 `reason` 作为提示文本。

如果任何匹配的 `Stop` 钩子返回 `continue: false`，则该钩子优先于其他匹配的 `Stop` 钩子的继续决策。

<a id="interrupt"></a>

### 中断

当您中断主线程上的活动回合时，`Interrupt` 就会运行。用它来记录由钩子启动的中断或清理工作。它不会针对空闲线程或子智能体运行，并且任何配置的 `matcher` 都会被忽略。

除了 [常用输入字段](#common-input-fields) 之外，事件还包括 `turn_id`、中断回合的 id 和 `permission_mode`。

命令挂钩默认为一秒超时。配置的超时限制为一到三秒。挂钩输出不能防止中断或重新开始对话轮次。退出 `0` 且无输出，或返回 JSON 并带有可选的 `systemMessage` 以显示警告。纯文本输出对此事件无效。

```json
{ "systemMessage": "Saved the interrupted turn to the local audit log." }
```

<a id="schemas"></a>

## 模式

链接的 `main` 分支架构可能包含当前版本中不存在的挂钩字段。使用此页面作为发布行为参考。

如果您需要准确的当前线路格式，请参阅 [Codex GitHub 仓库](https://github.com/openai/codex/tree/main/codex-rs/hooks/schema/generated) 中生成的模式。

<a id="plain-text-aliases"></a>

### 纯文本别名

- 字符串 | 空