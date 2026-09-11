> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/app-server.md)。

<a id="codex-app-server"></a>

# Codex 应用服务器

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 应用程序服务器是 Codex 用于支持富客户端的接口（例如，Codex VS Code 扩展）。当您想要在自己的产品中进行深度集成时，请使用它：身份验证、对话历史记录、批准和流式智能体事件。应用程序服务器实现在 Codex GitHub 仓库 ([openai/codex/codex-rs/应用程序服务器](https://github.com/openai/codex/tree/main/codex-rs/app-server)) 中开源。有关开源 Codex 组件的完整列表，请参阅 [开源](open-source.zh-CN.md) 页面。

如果您要自动化作业或在 CI 中运行 Codex，请改用 [Codex SDK](codex-sdk.zh-CN.md)。

<a id="connect-the-cli-terminal-ui"></a>

## 连接 CLI 终端 UI

远程终端 UI 模式允许您在一台计算机上运行应用程序服务器并从另一台计算机连接 Codex CLI 终端界面。启动 WebSocket 监听器：

```bash
codex app-server --listen ws://127.0.0.1:4500
```

然后连接终端UI：

```bash
codex --remote ws://127.0.0.1:4500
```

对于非本地连接，配置 WebSocket 身份验证并将连接置于 TLS 后面。将不记名令牌存储在环境变量中并传递其名称，而不是将令牌放在命令行上：

```bash
export CODEX_REMOTE_TOKEN="$(cat "$HOME/.codex/app-server-token")"
codex --remote wss://remote-host:4500 \
  --remote-auth-token-env CODEX_REMOTE_TOKEN
```

`--remote` 选项接受 `ws://`、`wss://`、`unix://` 和 `unix://PATH` 端点。仅对本地主机或 SSH 端口转发连接使用普通 WebSocket。

<a id="connect-a-remote-code-mode-host"></a>

## 连接远程代码模式主机

默认情况下，app-server 启动本地代码模式主机。要使用远程主机，请传递其安全 WebSocket URL：

```bash
codex app-server --code-mode-host wss://code-mode.example.com/host
```

`--code-mode-host` 控制从应用程序服务器到其代码模式主机的出站连接。它不会更改 `--listen`，它控制客户端如何连接到应用程序服务器。同一应用程序服务器进程中的每个线程共享选定的代码模式主机连接。

将 `wss://` 用于远程主机。仅将 `ws://` 用于本地主机或 SSH 转发的连接。 app-server 命令和 WebSocket 传输是实验性的，不支持生产工作负载。

<a id="protocol"></a>

## 协议

与 [MCP](https://modelcontextprotocol.io/) 一样，`codex app-server` 支持使用 JSON-RPC 2.0 消息的双向通信（在线路上省略 `"jsonrpc":"2.0"` 标头）。

支持的运输：

- `stdio`（`--listen stdio://`，默认）：换行符分隔的 JSON (JSONL)。
- `websocket`（`--listen ws://IP:PORT`，实验性且不受支持）：每个 WebSocket 文本框架一条 JSON-RPC 消息。
- Unix 套接字（`--listen unix://` 或 `--listen unix://PATH`）：使用标准 HTTP 升级握手，通过 Codex 的默认应用程序服务器控制套接字或自定义 Unix 套接字路径进行 WebSocket 连接。
- `off` (`--listen off`)：不要公开本地传输。

当您使用 `--listen ws://IP:PORT` 运行时，同一侦听器还提供基本的 HTTP 运行状况探测：

- 一旦侦听器接受新连接，`GET /readyz` 将返回 `200 OK`。
- 当请求不包含 `Origin` 标头时，`GET /healthz` 返回 `200 OK`。
- 带有 `Origin` 标头的请求将被拒绝，并显示 `403 Forbidden`。

WebSocket 传输是实验性的且不受支持。本地侦听器（例如 `ws://127.0.0.1:PORT`）适用于本地主机和 SSH 端口转发工作流。目前，非环回 WebSocket 侦听器在推出期间默认允许未经身份验证的连接，因此在远程公开连接之前请配置 WebSocket 身份验证。

支持的 WebSocket 身份验证标志：

- `--ws-auth capability-token --ws-token-file /absolute/path`
- `--ws-auth capability-token --ws-token-sha256 HEX`
- `--ws-auth signed-bearer-token --ws-shared-secret-file /absolute/path`

对于签名的不记名令牌，您还可以设置 `--ws-issuer`、`--ws-audience` 和 `--ws-max-clock-skew-seconds`。客户端将凭证呈现为“授权：持有者”<token>` during the WebSocket handshake, and app-server enforces auth before JSON-RPC `初始化`。

优先选择 `--ws-token-file` 而不是在命令行上传递原始不记名令牌。仅当客户端将原始高熵令牌保存在单独的本地秘密存储中时才使用 `--ws-token-sha256` ；哈希只是一个验证者，客户端仍然需要原始令牌。

在WebSocket模式下，应用程序服务器使用有界队列。当请求入口已满时，服务器会拒绝新请求，并显示 JSON-RPC 错误代码 `-32001` 和消息 `"Server overloaded; retry later."` 客户端应以指数级增加的延迟和抖动重试。

<a id="message-schema"></a>

## 消息架构

请求包括 `method`、`params` 和 `id`：

```json
{ "method": "thread/start", "id": 10, "params": { "model": "gpt-5.6-terra" } }
```

响应用 `result` 或 `error` 回显 `id`：

```json
{ "id": 10, "result": { "thread": { "id": "thr_123" } } }
```

```json
{ "id": 10, "error": { "code": 123, "message": "Something went wrong" } }
```

通知省略 `id` 并仅使用 `method` 和 `params`：

```json
{ "method": "turn/started", "params": { "turn": { "id": "turn_456" } } }
```

您可以从 CLI 生成 TypeScript 架构或 JSON 架构捆绑包。每个输出都特定于您运行的 Codex 版本，因此生成的工件与该版本完全匹配：

```bash
codex app-server generate-ts --out ./schemas
codex app-server generate-json-schema --out ./schemas
```

<a id="getting-started"></a>

## 开始使用

1. 使用 `codex app-server`（默认 stdio 传输）、`codex app-server --listen ws://127.0.0.1:4500`（TCP WebSocket）或 `codex app-server --listen unix://`（默认 Unix 套接字）启动服务器。
2. 通过所选传输连接客户端，然后发送 `initialize`，后跟 `initialized` 通知。
3. 启动一个线程并循环，然后继续从活动传输流中读取通知。

示例（Node.js / TypeScript）：

```ts



const proc = spawn("codex", ["app-server"], {
  stdio: ["pipe", "pipe", "inherit"],
});
const rl = readline.createInterface({ input: proc.stdout });

const send = (message: unknown) => {
  proc.stdin.write(`${JSON.stringify(message)}\n`);
};

let threadId: string | null = null;

rl.on("line", (line) => {
  const msg = JSON.parse(line) as any;
  console.log("server:", msg);

  if (msg.id === 1 && msg.result?.thread?.id && !threadId) {
    threadId = msg.result.thread.id;
    send({
      method: "turn/start",
      id: 2,
      params: {
        threadId,
        input: [{ type: "text", text: "Summarize this repo." }],
      },
    });
  }
});

send({
  method: "initialize",
  id: 0,
  params: {
    clientInfo: {
      name: "my_product",
      title: "My Product",
      version: "0.1.0",
    },
  },
});
send({ method: "initialized", params: {} });
send({ method: "thread/start", id: 1, params: { model: "gpt-5.6-terra" } });
```

<a id="core-primitives"></a>

## 核心原语

- **线程**：用户和 Codex 智能体之间的对话。线程包含匝数。
- **转**：单个用户请求和随后的智能体工作。回合包含项目并流增量更新。
- **项目**：输入或输出单元（用户消息、智能体消息、命令运行、文件更改、工具调用等）。

使用线程 API 来创建、列出或存档对话。使用 Turn API 推动对话并通过 Turn 通知传输进度。

<a id="lifecycle-overview"></a>

## 生命周期概述

- **每个连接初始化一次**：打开传输连接后，立即发送带有客户端元数据的 `initialize` 请求，然后发出 `initialized`。在此握手之前，服务器拒绝该连接上的任何请求。
- **启动（或恢复）线程**：调用 `thread/start` 进行新对话，调用 `thread/resume` 继续现有对话，或调用 `thread/fork` 将历史记录分支到新的线程 ID。
- **开始对话轮次**：使用目标 `threadId` 和用户输入调用 `turn/start`。可选字段覆盖模型、个性、`cwd`、沙箱策略等。
- **转向主动对话轮次**：调用 `turn/steer` 将用户输入附加到当前正在进行的对话轮次，而不创建新的对话轮次。
- **直播事件**：在 `turn/start` 之后，继续阅读标准输出上的通知：`thread/archived`、`thread/unarchived`、`item/started`、`item/completed`、`item/agentMessage/delta`、工具进度和其他更新。
- **完成对话轮次**：当模型完成时或 `turn/interrupt` 取消后，服务器会发出 `turn/completed` 的最终状态。

<a id="initialization"></a>

## 初始化

在调用该连接上的任何其他方法之前，客户端必须为每个传输连接发送一个 `initialize` 请求，然后通过 `initialized` 通知进行确认。初始化之前发送的请求会收到 `Not initialized` 错误，并且在同一连接上重复进行 `initialize` 调用会返回 `Already initialized`。

服务器返回将呈现给上游服务的用户智能体字符串以及描述运行时目标的 `platformFamily` 和 `platformOs` 值。设置 `clientInfo` 以识别您的集成。

`initialize.params.capabilities` 还支持以下客户端功能：

- `optOutNotificationMethods` - 要抑制此连接的确切通知方法名称。匹配精确（无通配符或前缀）；未知的名称会被接受并被忽略。
- `requestAttestation` - 选择加入服务器发起的 `attestation/generate` 请求。提供上游证明的桌面主机以不透明的 `{ "token": "..." }` 值进行响应。
- `mcpServerOpenaiFormElicitation` - 允许下游 MCP 服务器发送 `mcpServer/elicitation/request` 的 OpenAI 扩展形式变体。

**重要**：使用 `clientInfo.name` 识别 OpenAI 合规性日志平台的客户端。如果您正在开发供企业使用的新 Codex 集成，请联系 OpenAI 以将其添加到已知客户列表中。有关更多上下文，请参阅 [Codex 日志参考](https://chatgpt.com/public/admin/api-reference#tag/Codex)。

示例（来自 Codex VS Code 扩展）：

```json
{
  "method": "initialize",
  "id": 0,
  "params": {
    "clientInfo": {
      "name": "codex_vscode",
      "title": "Codex VS Code Extension",
      "version": "0.1.0"
    }
  }
}
```

选择退出通知的示例：

```json
{
  "method": "initialize",
  "id": 1,
  "params": {
    "clientInfo": {
      "name": "my_client",
      "title": "My Client",
      "version": "0.1.0"
    },
    "capabilities": {
      "experimentalApi": true,
      "optOutNotificationMethods": ["thread/started", "item/agentMessage/delta"]
    }
  }
}
```

<a id="experimental-api-opt-in"></a>

## 实验性 API 选择加入

一些应用程序服务器方法和字段有意被限制在 `experimentalApi` 功能后面。

- 省略 `capabilities`（或将 `experimentalApi` 设置为 `false`）以保持稳定的 API 使用界面，并且服务器拒绝实验方法/字段。
- 将 `capabilities.experimentalApi` 设置为 `true` 以启用实验方法和字段。

```json
{
  "method": "initialize",
  "id": 1,
  "params": {
    "clientInfo": {
      "name": "my_client",
      "title": "My Client",
      "version": "0.1.0"
    },
    "capabilities": {
      "experimentalApi": true
    }
  }
}
```

如果客户端发送实验方法或字段而未选择加入，则应用程序服务器会拒绝它：

`<descriptor>需要实验性Api能力`

<a id="api-overview"></a>

## API概览

- `thread/start`——创建一个新线程；发出 `thread/started` 并自动为您订阅该线程的转动/项目事件。
- `thread/resume` - 通过 id 重新打开现有线程，以便稍后 `turn/start` 调用附加到它。
- `thread/fork` - 通过复制存储的历史记录将线程分叉为新的线程 ID。传递 `lastTurnId` 来复制该回合的历史记录并忽略后面的回合，或者传递 `ephemeral: true` 来创建内存中的分叉。为新线程发出 `thread/started` ；返回的线程包括 `forkedFromId`（如果可用）。
- `thread/read` - 通过 id 读取存储的线程而不恢复它；设置 `includeTurns` 返回完整的对话轮次历史记录。返回的 `thread` 对象包括运行时 `status`。
- `thread/list` - 分页存储的线程日志；支持基于光标的分页以及 `modelProviders`、`sourceKinds`、`archived`、`isPinned`、`cwd`、`useStateDbOnly`、`searchTerm` 和实验性 `parentThreadId` 或 `ancestorThreadId` 过滤器。返回的 `thread` 对象包括运行时 `status`。
- `thread/turns/list` - 实验性；翻阅已存储线程的轮次历史记录而不恢复它。 `itemsView` 控制轮转项是否被省略、汇总或满载。
- `thread/items/list` - 实验性；翻阅持久线程项目，可以选择限制为一个 `turnId`。活动线程存储必须支持项目分页。
- `thread/loaded/list` - 列出当前加载到内存中的线程 ID。
- `thread/name/set` - 设置或更新已加载线程或持久推出的线程的面向用户的名称；发出 `thread/name/updated`。
- `thread/goal/set` - 为线程设置目标；发出 `thread/goal/updated`。
- `thread/goal/get` - 读取线程的当前目标。
- `thread/goal/clear` - 清除线程的目标；发出 `thread/goal/cleared`。
- `thread/metadata/update` - 修补 SQLite 支持的存储线程元数据，包括持久化的 `gitInfo` 和 `isPinned`。
- `thread/archive` - 将线程的日志文件移动到存档目录中，并尝试存档尚未存档的派生后代线程日志；成功时返回 `{}` 并为每个存档线程发出 `thread/archived`。
- `thread/delete` - 永久删除持久的活动或存档线程以及任何生成的后代线程；成功时返回 `{}` ，并为每个删除的线程发出 `thread/deleted` 。
- `thread/unsubscribe` - 从线程转动/项目事件中取消订阅此连接。如果这是最后一个订阅者，则服务器会在无订阅者不活动宽限期后卸载线程并发出 `thread/closed`。
- `thread/unarchive` - 将存档的线程转出恢复到活动会话目录中；返回恢复的 `thread` 并发出 `thread/unarchived`。
- `thread/status/changed` - 当加载线程的运行时 `status` 更改时发出通知。
- `thread/compact/start` - 触发线程的对话历史压缩；立即返回 `{}`，同时通过 `turn/*` 和 `item/*` 通知进行流式传输。
- `thread/shellCommand` - 针对线程运行用户启动的 shell 命令。它在沙箱外部运行，具有完全访问权限，并且不继承线程沙箱策略。
- `thread/backgroundTerminals/clean` - 停止线程的所有正在运行的后台终端（实验性的；需要 `capabilities.experimentalApi`）。
- `thread/backgroundTerminals/list` - 列出已加载线程的正在运行的后台终端（实验性；需要 `capabilities.experimentalApi`）。
- `thread/backgroundTerminals/terminate` - 通过应用程序服务器 `processId` 终止一个正在运行的后台终端（实验性；需要 `capabilities.experimentalApi`）。
- `thread/rollback` - 已弃用；从内存上下文中删除最后 N 轮并保留回滚标记；返回更新后的 `thread`。
- `turn/start` - 将用户输入或独立工具输出添加到线程并开始 Codex 生成；以初始 `turn` 进行响应并流式传输事件。对于`collaborationMode`，`settings.developer_instructions: null`表示“对所选模式使用内置指令”。
- `thread/inject_items` - 将原始响应 API 项附加到已加载线程的模型可见历史记录中，而无需启动用户轮次。
- `turn/steer` - 将用户输入附加到线程的活动运行中对话轮次；返回接受的 `turnId`。
- `turn/interrupt` - 请求取消飞行中的对话轮次；成功是 `{}`，回合结束是 `status: "interrupted"`。
- `review/start` - 启动 Codex 审阅者的线程；发出 `enteredReviewMode` 和 `exitedReviewMode` 项目。
- `command/exec` - 在服务器沙箱下运行单个命令，无需启动线程/回合。
- `command/exec/write` - 将 `stdin` 字节写入正在运行的 `command/exec` 会话或关闭 `stdin`。
- `command/exec/resize` - 调整正在运行的 PTY 支持的 `command/exec` 会话的大小。
- `command/exec/terminate` - 停止正在运行的 `command/exec` 会话。
- `command/exec/outputDelta`（通知） - 从流 `command/exec` 会话中发出 Base64 编码的 stdout/stderr 块。
- `process/spawn` - 在 Codex 的沙箱外部启动显式进程会话（实验性；需要 `capabilities.experimentalApi`）。
- `process/writeStdin` - 将标准输入字节写入正在运行的 `process/spawn` 会话或关闭标准输入（实验性）。
- `process/resizePty` - 调整正在运行的 PTY 支持的进程会话的大小（实验性）。
- `process/kill` - 终止正在运行的进程会话（实验性）。
- `process/outputDelta` 和 `process/exited`（通知）- 为流处理输出和处理退出状态而发出（实验性）。
- `model/list` - 列出可用模型（设置 `includeHidden: true` 以包括带有 `hidden: true` 的条目）以及工作选项、可选 `upgrade` 和 `inputModalities`。
- `modelProvider/capabilities/read` - 读取模型/提供商组合的提供商能力范围。
- `experimentalFeature/list` - 列出具有生命周期阶段元数据和光标分页的功能标志。
- `experimentalFeature/enablement/set` - 为受支持的功能键（例如 `apps` 和 `plugins`）修补内存运行时设置。
- `environment/info` - 实验性；连接到配置的执行环境并返回其 shell 和默认工作目录。
- `permissionProfile/list` - 列出 beta 权限配置文件以及有效要求是否允许它们，并带有光标分页。
- `collaborationMode/list` - 列出协作模式预设（实验性，无分页）。
- `skills/list` - 列出一个或多个 `cwd` 值的技能（支持 `forceReload` 和可选的 `perCwdExtraUserRoots`）。
- `skills/extraRoots/set` - 替换用于发现独立技能而不保留它们的进程级额外根。
- `skills/changed`（通知）- 当观察本地技能文件更改时发出。
- `hooks/list` - 列出一个或多个 `cwd` 值的已发现生命周期挂钩。
- `marketplace/add` - 添加远程插件市场并将其保留到用户的市场配置中。
- `marketplace/remove` - 删除已配置的市场及其已安装的市场根（如果存在）。
- `marketplace/upgrade` - 当您省略市场名称时，刷新已配置的 Git 市场或所有已配置的 Git 市场。
- `plugin/list` - 正在开发中；列出已发现的插件市场和插件状态，包括安装/身份验证策略元数据、市场加载错误、特色插件 ID 以及本地、Git、包注册表或远程插件源元数据。摘要可以包括远程 `version`、本地 `localVersion`、结构化亮/暗图标和 `installPolicySource`，对于当前远程行，它可以是 `null`、`WORKSPACE_SETTING` 或 `IMPLICIT_CANONICAL_APP`。暂时不要从生产客户端调用此方法。
- `plugin/read` - 正在开发中；按市场路径或远程市场名称和插件名称读取一个插件，包括捆绑技能、应用程序、MCP 服务器名称以及远程插件 `shareUrl`（如果远程目录提供）。暂时不要从生产客户端调用此方法。
- `plugin/install` - 正在开发中；从市场路径或远程市场名称安装插件。暂时不要从生产客户端调用此方法。
- `plugin/uninstall` - 正在开发中；卸载已安装的插件。暂时不要从生产客户端调用此方法。
- `plugin/skill/read` - 通过远程市场、插件 ID 和技能名称按需读取远程插件技能 Markdown。
- `app/installed` - 读取已安装的应用程序运行时状态，包括每个应用程序的有效启用和可调用状态。
- `app/list` - 列出可用的应用程序（连接器），具有分页以及可访问性/启用的元数据。
- `app/read` - 获取特定应用程序 ID 的元数据和可选的仅显示工具摘要。
- `skills/config/write` - 按路径启用或禁用技能。
- `mcpServer/oauth/login` - 为已配置的 MCP 服务器启动 OAuth 登录；返回授权 URL 并在完成时发出 `mcpServer/oauthLogin/completed`。
- `tool/requestUserInput` - 提示用户 1-3 个简短问题以进行工具调用（实验性）；问题可以设置`isOther`为自由格式选项。
- `mcpServer/elicitation/request`（服务器请求）- 要求客户端进行结构化表单输入或确认 MCP 服务器请求的 URL 流。
- `item/permissions/requestApproval`（服务器请求）- 要求客户端授予内置 `request_permissions` 工具请求的网络或文件系统权限的子集。
- `config/mcpServer/reload` - 从磁盘重新加载 MCP 服务器配置并对已加载线程的刷新进行排队。
- `mcpServerStatus/list` - 列出 MCP 服务器、工具、资源和身份验证状态（光标+限制分页）。使用 `detail: "full"` 获取完整数据，或使用 `detail: "toolsAndAuthOnly"` 省略资源。
- `mcpServer/resource/read` - 通过初始化的 MCP 服务器读取单个 MCP 资源。
- `mcpServer/tool/call` - 在线程配置的 MCP 服务器上调用工具。
- `mcpServer/startupStatus/updated`（通知）- 当已配置的 MCP 服务器的启动状态针对已加载线程发生更改时发出。
- `windowsSandbox/setupStart` - 启动 `elevated` 或 `unelevated` 模式的 Windows 沙箱设置；快速返回并随后发出 `windowsSandbox/setupCompleted`。
- `feedback/upload` - 提交反馈报告（分类+可选原因/日志+对话ID，以及可选`extraLogFiles`附件）。
- `config/read` - 解决配置分层后，在磁盘上获取有效配置。
- `externalAgentConfig/detect` - 检测可以使用 `includeHome` 和可选的 `cwds` 迁移的外部智能体工件；每个检测到的项目包括 `cwd`（家用 `null`）。
- `externalAgentConfig/import` - 通过显式传递 `migrationItems` 和 `cwd`（`null` 用于主目录）来应用选定的外部智能体迁移项目。支持的项目类型包括配置、技能、`AGENTS.md`、插件、MCP 服务器配置、子智能体、挂钩、命令和会话；非空导入在工作完成时发出 `externalAgentConfig/import/progress` 和 `externalAgentConfig/import/completed` 。插件和会话导入可以异步完成。
- `config/value/write` - 将单个配置键/值写入磁盘上用户的 `config.toml`。
- `config/batchWrite` - 将配置编辑自动应用到磁盘上用户的 `config.toml`。
- `configRequirements/read` - 从 `requirements.toml` 和/或 MDM 获取要求，包括精确的托管配置、允许列表、固定的 `featureRequirements` 和网络要求（如果您尚未进行任何设置，则为 `null`）。
- `fs/readFile`、`fs/writeFile`、`fs/createDirectory`、`fs/getMetadata`、`fs/readDirectory`、`fs/remove`、`fs/copy`、`fs/watch`、`fs/unwatch` 和 `fs/changed`（通知） - 操作通过 app-server v2 文件系统 API 的绝对文件系统路径。

插件摘要包括 `source` 联合。本地插件返回 `{ "type": "local", "path": ... }`，Git 支持的市场条目返回 `{ "type": "git", "url": ..., "path": ..., "refName": ..., "sha": ... }`，包注册表条目返回 `{ "type": "npm", "package": ..., "version": ..., "registry": ... }`，远程目录条目返回 `{ "type": "remote" }`。对于仅远程目录条目，`PluginMarketplaceEntry.path` 可以是 `null`；读取或安装这些插件时传递 `remoteMarketplaceName` 而不是 `marketplacePath`。

<a id="models"></a>

## 模型

<a id="list-models-modellist"></a>

### 列出模型（`model/list`）

在渲染模型或个性选择器之前，请致电 `model/list` 以发现可用模型及其功能。

```json
{ "method": "model/list", "id": 6, "params": { "limit": 20, "includeHidden": false } }
{ "id": 6, "result": {
  "data": [{
    "id": "gpt-5.6-sol",
    "model": "gpt-5.6-sol",
    "displayName": "GPT-5.6-Sol",
    "hidden": false,
    "defaultReasoningEffort": "low",
    "supportedReasoningEfforts": [{
      "reasoningEffort": "low",
      "description": "Fast responses with lighter reasoning"
    }],
    "inputModalities": ["text", "image"],
    "supportsPersonality": true,
    "isDefault": true
  }],
  "nextCursor": null
} }
```

每个模型条目可以包括：

- `supportedReasoningEfforts` - 模型支持的工作量选项。
- `defaultReasoningEffort` - 为客户建议的默认工作量。
- `upgrade` - 客户端中迁移提示的可选推荐升级模型 ID。
- `upgradeInfo` - 客户端中迁移提示的可选升级元数据。
- `hidden` - 模型是否在默认选择器列表中隐藏。
- `inputModalities` - 模型支持的输入类型（例如 `text`、`image`）。
- `supportsPersonality` - 模型是否支持个性特定指令，例如`/personality`。
- `isDefault` - 该模型是否为推荐默认值。

默认情况下，`model/list` 仅返回选择器可见的模型。如果您需要完整列表并希望使用 `hidden` 在客户端进行过滤，请设置 `includeHidden: true`。

当 `inputModalities` 缺失（旧模型目录）时，将其视为 `["text", "image"]` 以实现向后兼容性。

<a id="list-experimental-features-experimentalfeaturelist"></a>

### 列出实验功能（`experimentalFeature/list`）

使用此端点来发现具有元数据和生命周期阶段的功能标志：

```json
{ "method": "experimentalFeature/list", "id": 7, "params": { "limit": 20 } }
{ "id": 7, "result": {
  "data": [{
    "name": "unified_exec",
    "stage": "beta",
    "displayName": "Unified exec",
    "description": "Use the unified PTY-backed execution tool.",
    "announcement": "Beta rollout for improved command execution reliability.",
    "enabled": false,
    "defaultEnabled": false
  }],
  "nextCursor": null
} }
```

`stage` 可以是 `beta`、`underDevelopment`、`stable`、`deprecated` 或 `removed`。对于非 Beta 标志，`displayName`、`description` 和 `announcement` 可能是 `null`。

<a id="inspect-an-execution-environment-experimental"></a>

### 检查执行环境（实验）

在开始工作之前，使用 `environment/info` 检查已配置的远程环境。该方法需要`capabilities.experimentalApi = true`。

```json
{ "method": "environment/info", "id": 8, "params": { "environmentId": "devbox" } }
{ "id": 8, "result": {
  "shell": { "name": "zsh", "path": "/bin/zsh" },
  "cwd": "file:///workspace/project"
} }
```

`cwd` 可以是 `null`。如果存在，它是使用环境的本机路径语法的规范 `file:` URI。未知的环境 ID 以及连接或协议失败会返回请求错误。

<a id="threads"></a>

## 线程数

- `thread/read` 读取存储的线程而不订阅它；设置 `includeTurns` 以包括匝数。
- `thread/turns/list` 是实验性的，它会翻阅存储的线程的轮次历史记录而不恢复它。使用 `itemsView` 选择是否省略、汇总或满载轮流项目。
- `thread/items/list` 是实验性的，可对持久线程项目进行分页，可选择限制为一圈。
- `thread/list` 支持光标分页以及 `modelProviders`、`sourceKinds`、`archived`、`isPinned`、`cwd`、`useStateDbOnly`、`searchTerm` 和实验性 `parentThreadId` 或 `ancestorThreadId`过滤。
- `thread/loaded/list` 返回当前内存中的线程ID。
- `thread/archive` 将线程的持久 JSONL 日志移动到存档目录中，并尝试存档尚未存档的衍生后代线程日志。
- `thread/delete` 永久删除持久的活动或存档线程及其派生的后代线程。
- `thread/metadata/update` 修补存储的线程元数据，包括持久化的 `gitInfo` 和 `isPinned`。
- `thread/unsubscribe` 取消订阅已加载线程的当前连接，并可以在不活动宽限期后触发 `thread/closed`。
- `thread/unarchive` 将存档的线程转出恢复到活动会话目录中。
- `thread/compact/start` 触发压缩并立即返回 `{}`。
- `thread/rollback` 已弃用。它从内存上下文中删除最后 N 轮，并在线程的持久 JSONL 日志中记录回滚标记。
- `thread/inject_items` 将原始响应 API 项附加到已加载线程的模型可见历史记录中，而无需启动用户轮询。

<a id="start-or-resume-a-thread"></a>

### 启动或恢复线程

当您需要新的 Codex 对话时，开始新的线程。

```json
{ "method": "thread/start", "id": 10, "params": {
  "model": "gpt-5.6-terra",
  "cwd": "/Users/me/project",
  "approvalPolicy": "never",
  "sandbox": "workspaceWrite",
  "personality": "friendly",
  "serviceName": "my_app_server_client"
} }
{ "id": 10, "result": {
  "thread": {
    "id": "thr_123",
    "sessionId": "thr_123",
    "preview": "",
    "ephemeral": false,
    "modelProvider": "openai",
    "createdAt": 1730910000
  }
} }
{ "method": "thread/started", "params": { "thread": { "id": "thr_123" } } }
```

`serviceName` 是可选的。当您希望应用程序服务器使用集成的服务名称标记线程级指标时，请设置它。

`thread/start`、`thread/resume` 和 `thread/fork` 返回 `instructionSources`，即加载的指令文件路径的数组。每个路径都使用其源环境的本机绝对语法，包括远程环境。

实验客户端可以将 `thread/start` 上的 `historyMode` 设置为 `"legacy"`（默认）或 `"paginated"`。尚不支持分页线程创建，并返回 JSON-RPC 错误 `-32601`。应用程序服务器可以列出和读取现有分页记录的摘要，但完整历史记录读取、转向分页和恢复失败关闭，直到支持分页历史记录为止。

选择加入 `capabilities.experimentalApi` 的 Beta 客户端可以在 `permissions` 中传递命名的权限配置文件 ID，而不是旧的 `sandbox` 字段。请勿将 `permissions` 和 `sandbox` 一起发送。将 `permissionProfile/list` 与项目 `cwd` 结合使用，以发现可用的配置文件以及托管需求是否允许每个配置文件。

`thread.sessionId` 标识当前实时会话树根。根线程使用自己的线程id作为会话id；分叉线程保留它们来自的根的会话 ID。客户端应该从 `thread.sessionId` 读取会话 id，而不是从线程 id 中获取它。

要继续存储的会话，请使用您之前录制的 `thread.id` 调用 `thread/resume`。响应形状与 `thread/start` 匹配。您还可以传递 `thread/start` 支持的相同配置覆盖，例如 `personality`：

```json
{ "method": "thread/resume", "id": 11, "params": {
  "threadId": "thr_123",
  "personality": "friendly"
} }
{ "id": 11, "result": { "thread": { "id": "thr_123", "name": "Bug bash notes", "ephemeral": false } } }
```

恢复线程本身不会更新 `thread.updatedAt` （或转出文件的修改时间）。当您开始对话轮次时，时间戳会更新。

如果您在配置中将已启用的 MCP 服务器标记为 `required`，并且该服务器无法初始化，则 `thread/start` 和 `thread/resume` 会失败，而不是在没有它的情况下继续。

`thread/start`上的`dynamicTools`是一个实验场（需要`capabilities.experimentalApi = true`）。 Codex 将这些动态工具保留在线程推出元数据中，并在您不提供新的动态工具时在 `thread/resume` 上恢复它们​​。

如果您继续使用与首次部署中记录的模型不同的模型，Codex 会发出警告并在下一回合应用一次性模型切换指令。

<a id="manage-a-thread-goal"></a>

### 管理线程目标

使用 `thread/goal/set`、`thread/goal/get` 和 `thread/goal/clear` 管理 TUI 中 `/goal` 所显示的相同持久目标状态。

```json
{ "method": "thread/goal/set", "id": 13, "params": {
  "threadId": "thr_123",
  "objective": "Finish the migration and keep tests green",
  "status": "active",
  "tokenBudget": 40000
} }
{ "id": 13, "result": { "goal": {
  "threadId": "thr_123",
  "objective": "Finish the migration and keep tests green",
  "status": "active",
  "tokenBudget": 40000,
  "tokensUsed": 0,
  "timeUsedSeconds": 0
} } }
{ "method": "thread/goal/updated", "params": {
  "threadId": "thr_123",
  "goal": {
    "threadId": "thr_123",
    "objective": "Finish the migration and keep tests green",
    "status": "active",
    "tokenBudget": 40000,
    "tokensUsed": 0,
    "timeUsedSeconds": 0
  }
} }
```

目标必须非空且最多 4,000 个字符。提供新目标会替换目标并重置使用情况统计。提供当前的非终端目标，或省略 `objective`，更新状态或Token预算，同时保留使用历史记录。

要从存储的会话分支，请使用 `thread.id` 调用 `thread/fork`。这将创建一个新的线程 ID 并为其发出 `thread/started` 通知。通过 `lastTurnId` 复制该回合的历史记录（包含在内），并忽略后面的回合：

```json
{ "method": "thread/fork", "id": 12, "params": { "threadId": "thr_123", "lastTurnId": "turn_456" } }
{ "id": 12, "result": { "thread": { "id": "thr_456", "sessionId": "thr_123", "forkedFromId": "thr_123" } } }
{ "method": "thread/started", "params": { "thread": { "id": "thr_456" } } }
```

应用程序服务器拒绝正在进行的 `lastTurnId`。如果在源线程处于中间转动时省略该字段，则前叉会记录中断标记，而不是保留未标记的部分转动。

传递 `ephemeral: true` 来创建内存中的分叉，而不将其添加到存储的线程列表中：

```json
{
  "method": "thread/fork",
  "id": 13,
  "params": {
    "threadId": "thr_123",
    "ephemeral": true
  }
}
{
  "id": 13,
  "result": {
    "thread": {
      "id": "thr_789",
      "sessionId": "thr_789",
      "forkedFromId": "thr_123",
      "ephemeral": true
    }
  }
}
```

分页线程的临时分支也需要 `excludeTurns: true`。该字段是实验性的，需要 `capabilities.experimentalApi = true`。

设置面向用户的线程标题后，应用程序服务器会在 `thread/list`、`thread/read`、`thread/resume`、`thread/unarchive` 和 `thread/rollback` 响应上水化 `thread.name`。 `thread/start` 和 `thread/fork` 可以省略 `name`（或返回 `null`），直到稍后设置标题。

<a id="read-a-stored-thread-without-resuming"></a>

### 读取存储的线程（无需恢复）

当您想要存储线程数据但不想恢复线程或订阅其事件时，请使用 `thread/read`。

- `includeTurns` - 当 `true` 时，响应包括螺纹圈数；当 `false` 或省略时，您仅获得线程摘要。
- 返回的 `thread` 对象包括运行时 `status`（`notLoaded`、`idle`、`systemError` 或 `active` 与 `activeFlags`）。

```json
{ "method": "thread/read", "id": 19, "params": { "threadId": "thr_123", "includeTurns": true } }
{ "id": 19, "result": { "thread": { "id": "thr_123", "name": "Bug bash notes", "ephemeral": false, "status": { "type": "notLoaded" }, "turns": [] } } }
```

与 `thread/resume` 不同，`thread/read` 不会将线程加载到内存中或发出 `thread/started`。

<a id="list-thread-turns"></a>

### 列出螺纹圈数

`thread/turns/list` 是实验性的。使用它可以对存储的线程的轮次历史记录进行分页，而无需恢复它。结果默认为最新优先，因此客户端可以使用 `nextCursor` 获取较旧的轮次。响应还包括`backwardsCursor`；将其作为 `cursor` 与 `sortDirection: "asc"` 一起传递，以获取比前一页中的第一项更新的轮数。

`itemsView` 控制响应包含多少回合项目数据：

- `notLoaded` 省略项目。
- `summary` 返回汇总项目数据，省略时为默认值。
- `full` 返回完整的项目数据。

```json
{ "method": "thread/turns/list", "id": 20, "params": {
  "threadId": "thr_123",
  "limit": 50,
  "sortDirection": "desc",
  "itemsView": "summary"
} }
{ "id": 20, "result": {
  "data": [],
  "nextCursor": "older-turns-cursor-or-null",
  "backwardsCursor": "newer-turns-cursor-or-null"
} }
```

`thread/items/list` 也是实验性的。它对持久项目进行分页而不恢复线程。传递 `turnId` 将结果限制为一圈，或忽略它以跨线程对项目进行分页。活动线程存储必须支持项目分页；否则，服务器将返回不支持的方法错误。

<a id="list-threads-with-pagination--filters"></a>

### 列出主题（带分页和过滤器）

`thread/list` 允许您渲染历史 UI。 `createdAt` 默认结果为最新优先。过滤器在分页之前应用。通过以下任意组合：

- `cursor` - 来自先前响应的不透明字符串；省略第一页。
- `limit` - 如果未设置，服务器默认为合理的页面大小。
- `sortKey` - `created_at`（默认）、`updated_at` 或 `recency_at`。
- `sortDirection` - `desc`（默认）或 `asc`。
- `modelProviders` - 将结果限制为特定提供商； unset、null 或空数组包含所有提供程序。
- `sourceKinds` - 将结果限制为特定线程源。当省略或 `[]` 时，服务器默认仅使用交互式源：`cli` 和 `vscode`。
- `archived` - 当 `true` 时，仅列出已存档的线程。当 `false` 或省略时，列出非归档线程（默认）。
- `isPinned` - 如果提供，则仅返回具有匹配的持久引脚状态的线程。省略它可返回固定和未固定的线程。
- `cwd` - 将结果限制为会话当前工作目录与此路径或数组中的路径之一完全匹配的线程。从应用程序服务器进程工作目录解析相对路径。
- `useStateDbOnly` - 当 `true` 时，返回状态数据库结果，而不扫描 JSONL 线程日志来修复元数据。忽略它或传递 `false` 以获得默认扫描和修复行为。
- `searchTerm` - 将结果限制为提取的标题包含此区分大小写的文本片段的线程。
- `parentThreadId` - 将结果限制为给定父线程的直接子线程。该过滤器是实验性的，需要 `capabilities.experimentalApi = true`。
- `ancestorThreadId` - 将结果限制为给定线程在任何深度生成的后代。该滤波器是实验性的，需要 `capabilities.experimentalApi = true`；请勿将其与 `parentThreadId` 结合使用。

`sourceKinds` 接受以下值：

- `cli`
- `vscode`
- `exec`
- `appServer`
- `subAgent`
- `subAgentReview`
- `subAgentCompact`
- `subAgentThreadSpawn`
- `subAgentOther`
- `unknown`

示例：

```json
{ "method": "thread/list", "id": 20, "params": {
  "cursor": null,
  "limit": 25,
  "sortKey": "created_at"
} }
{ "id": 20, "result": {
  "data": [
    { "id": "thr_a", "preview": "Create a TUI", "ephemeral": false, "isPinned": true, "modelProvider": "openai", "createdAt": 1730831111, "updatedAt": 1730831111, "name": "TUI prototype", "status": { "type": "notLoaded" } },
    { "id": "thr_b", "preview": "Fix tests", "ephemeral": false, "isPinned": false, "modelProvider": "openai", "createdAt": 1730750000, "updatedAt": 1730750000, "status": { "type": "notLoaded" } }
  ],
  "nextCursor": "opaque-token-or-null"
} }
```

当 `nextCursor` 为 `null` 时，您已到达最后一页。

<a id="update-stored-thread-metadata"></a>

### 更新存储的线程元数据

使用 `thread/metadata/update` 修补存储的线程元数据而不恢复线程。设置 `isPinned` 以固定或取消固定线程，或更新 `gitInfo` 以更改持久的 Git 元数据。省略的字段保持不变；显式 `null` 清除存储的 Git 元数据值。

```json
{ "method": "thread/metadata/update", "id": 21, "params": {
  "threadId": "thr_123",
  "isPinned": true,
  "gitInfo": { "branch": "feature/sidebar-pr" }
} }
{ "id": 21, "result": {
  "thread": {
    "id": "thr_123",
    "isPinned": true,
    "gitInfo": { "sha": null, "branch": "feature/sidebar-pr", "originUrl": null }
  }
} }
```

<a id="track-thread-status-changes"></a>

### 跟踪线程状态变化

每当加载线程的运行时状态发生变化时，就会发出 `thread/status/changed`。有效负载包括`threadId`和新的`status`。

```json
{
  "method": "thread/status/changed",
  "params": {
    "threadId": "thr_123",
    "status": { "type": "active", "activeFlags": ["waitingOnApproval"] }
  }
}
```

<a id="list-loaded-threads"></a>

### 列出已加载的线程

`thread/loaded/list` 返回当前加载到内存中的线程 ID。

```json
{ "method": "thread/loaded/list", "id": 21 }
{ "id": 21, "result": { "data": ["thr_123", "thr_456"] } }
```

<a id="unsubscribe-from-a-loaded-thread"></a>

### 取消订阅已加载的线程

`thread/unsubscribe` 删除当前连接对线程的订阅。响应状态是以下之一：

- `unsubscribed` 连接已订阅，现已删除。
- `notSubscribed` 当连接未订阅该线程时。
- 当线程未加载时为 `notLoaded`。

如果这是最后一个订阅者，服务器将保持线程加载，直到 30 分钟内没有订阅者且没有线程活动。当宽限期到期时，应用服务器卸载线程并发出 `thread/status/changed` 转换到 `notLoaded` 加上 `thread/closed`。

```json
{ "method": "thread/unsubscribe", "id": 22, "params": { "threadId": "thr_123" } }
{ "id": 22, "result": { "status": "unsubscribed" } }
```

如果线程稍后过期：

```json
{ "method": "thread/status/changed", "params": {
    "threadId": "thr_123",
    "status": { "type": "notLoaded" }
} }
{ "method": "thread/closed", "params": { "threadId": "thr_123" } }
```

<a id="archive-a-thread"></a>

### 归档主题

使用 `thread/archive` 将持久线程日志（作为 JSONL 文件存储在磁盘上）移动到存档会话目录中。归档线程还会尝试归档尚未归档的衍生后代线程。

```json
{ "method": "thread/archive", "id": 22, "params": { "threadId": "thr_b" } }
{ "id": 22, "result": {} }
{ "method": "thread/archived", "params": { "threadId": "thr_b" } }
{ "method": "thread/archived", "params": { "threadId": "thr_child" } }
```

除非您传递 `archived: true`，否则存档的线程不会出现在以后对 `thread/list` 的调用中。服务器为其实际归档的每个线程发出一个 `thread/archived` 通知；如果无法存档生成的后代，则请求仍然可以成功，而无需该后代的存档通知。

<a id="delete-a-thread"></a>

### 删除话题

使用 `thread/delete` 永久删除持久的活动或存档线程及其派生的后代线程。服务器在返回成功之前删除现有的部署文件和关联的元数据；丢失的部署文件将被视为已删除。临时根线程无法删除。

```json
{ "method": "thread/delete", "id": 23, "params": { "threadId": "thr_b" } }
{ "id": 23, "result": {} }
{ "method": "thread/deleted", "params": { "threadId": "thr_b" } }
{ "method": "thread/deleted", "params": { "threadId": "thr_child" } }
```

<a id="unarchive-a-thread"></a>

### 取消归档线程

使用 `thread/unarchive` 将存档线程转出移回活动会话目录。

```json
{ "method": "thread/unarchive", "id": 24, "params": { "threadId": "thr_b" } }
{ "id": 24, "result": { "thread": { "id": "thr_b", "name": "Bug bash notes" } } }
{ "method": "thread/unarchived", "params": { "threadId": "thr_b" } }
```

<a id="trigger-thread-compaction"></a>

### 触发线程压缩

使用 `thread/compact/start` 触发线程的手动历史记录压缩。该请求立即返回 `{}`。

应用程序服务器在同一 `threadId` 上以标准 `turn/*` 和 `item/*` 通知的形式发出进度，包括 `contextCompaction` 项目生命周期（`item/started` 然后 `item/completed`）。

```json
{ "method": "thread/compact/start", "id": 25, "params": { "threadId": "thr_b" } }
{ "id": 25, "result": {} }
```

<a id="run-a-thread-shell-command"></a>

### 运行线程 shell 命令

对于属于线程的用户启动的 shell 命令，使用 `thread/shellCommand`。请求立即返回 `{}`，同时进度流通过标准 `turn/*` 和 `item/*` 通知。

此 API 在沙箱外运行，具有完全访问权限，并且不继承线程沙箱策略。客户端应该仅针对显式用户启动的命令公开它。

如果线程已经有一个活动轮次，则该命令将作为该轮次上的辅助操作运行，并且其格式化输出将被注入到该轮次的消息流中。如果线程空闲，app-server 会启动 shell 命令的独立轮次。

设置 `timeoutMs` 以限制执行时间（以毫秒为单位）。省略它或传递 `null` 使用一小时默认值。 `0` 请求立即超时；负值被拒绝。超时不会延迟立即 RPC 确认。

```json
{ "method": "thread/shellCommand", "id": 26, "params": { "threadId": "thr_b", "command": "git status --short", "timeoutMs": 10000 } }
{ "id": 26, "result": {} }
```

<a id="clean-background-terminals"></a>

### 清理后台终端

使用 `thread/backgroundTerminals/clean` 停止与线程关联的所有正在运行的后台终端。此方法是实验性的，需要 `capabilities.experimentalApi = true`。

```json
{ "method": "thread/backgroundTerminals/clean", "id": 27, "params": { "threadId": "thr_b" } }
{ "id": 27, "result": {} }
```

使用 `thread/backgroundTerminals/list` 检查正在运行的后台终端是否有加载的线程。该请求支持标准`cursor`和`limit`分页，返回的`processId`是应用程序服务器进程id。此方法是实验性的，需要 `capabilities.experimentalApi = true`：

```json
{ "method": "thread/backgroundTerminals/list", "id": 28, "params": { "threadId": "thr_b" } }
{ "id": 28, "result": { "data": [
  {
    "itemId": "item_456",
    "processId": "42",
    "command": "python3 -m http.server",
    "cwd": "/workspace",
    "osPid": null,
    "cpuPercent": null,
    "rssKb": null
  }
], "nextCursor": null } }
```

使用 `thread/backgroundTerminals/terminate` 和 `processId` 来停止一个后台终端。此方法是实验性的，需要 `capabilities.experimentalApi = true`：

```json
{ "method": "thread/backgroundTerminals/terminate", "id": 29, "params": { "threadId": "thr_b", "processId": "42" } }
{ "id": 29, "result": { "terminated": true } }
```

<a id="roll-back-recent-turns"></a>

### 回滚最近的回合

`thread/rollback` 已弃用并将被删除。它从内存上下文中删除最后的 `numTurns` 条目，并在转出日志中保留回滚标记。返回的`thread`包括回滚后填充的`turns`。

```json
{ "method": "thread/rollback", "id": 30, "params": { "threadId": "thr_b", "numTurns": 1 } }
{ "id": 30, "result": { "thread": { "id": "thr_b", "name": "Bug bash notes", "ephemeral": false } } }
```

<a id="turns"></a>

## 对话轮次

`input` 字段接受项目列表：

- `{ "type": "text", "text": "Explain this diff" }`
- `{ "type": "image", "url": "https://.../design.png" }`
- `{ "type": "localImage", "path": "/tmp/screenshot.png" }`

您可以覆盖每回合的配置设置（模型、工作量、个性、`cwd`、沙箱策略、摘要）。指定后，这些设置将成为稍后打开同一线程的默认设置。 `outputSchema` 仅适用于当前回合。对于`sandboxPolicy.type = "externalSandbox"`，将`networkAccess`设置为`restricted`或`enabled`；对于 `workspaceWrite`，`networkAccess` 仍然是布尔值。

对于`turn/start.collaborationMode`，`settings.developer_instructions: null`表示“对所选模式使用内置指令”而不是清除模式指令。

<a id="sandbox-read-access-readonlyaccess"></a>

### 沙箱读取访问（`ReadOnlyAccess`）

`sandboxPolicy` 支持显式读取访问控制：

- `readOnly`：可选`access`（默认为`{ "type": "fullAccess" }`，或受限根）。
- `workspaceWrite`：可选`readOnlyAccess`（默认为`{ "type": "fullAccess" }`，或受限根）。

限制读取访问形状：

```json
{
  "type": "restricted",
  "includePlatformDefaults": true,
  "readableRoots": ["/Users/me/shared-read-only"]
}
```

在 macOS 上，`includePlatformDefaults: true` 为受限读取会话附加策划的平台默认安全带策略。这提高了工具兼容性，而无需广泛允许所有 `/System`。

示例：

```json
{ "type": "readOnly", "access": { "type": "fullAccess" } }
```

```json
{
  "type": "workspaceWrite",
  "writableRoots": ["/Users/me/project"],
  "readOnlyAccess": {
    "type": "restricted",
    "includePlatformDefaults": true,
    "readableRoots": ["/Users/me/shared-read-only"]
  },
  "networkAccess": false
}
```

<a id="start-a-turn"></a>

### 开始对话轮次

```json
{ "method": "turn/start", "id": 30, "params": {
  "threadId": "thr_123",
  "input": [ { "type": "text", "text": "Run tests" } ],
  "cwd": "/Users/me/project",
  "approvalPolicy": "unlessTrusted",
  "sandboxPolicy": {
    "type": "workspaceWrite",
    "writableRoots": ["/Users/me/project"],
    "networkAccess": true
  },
  "model": "gpt-5.6-terra",
  "effort": "medium",
  "summary": "concise",
  "personality": "friendly",
  "outputSchema": {
    "type": "object",
    "properties": { "answer": { "type": "string" } },
    "required": ["answer"],
    "additionalProperties": false
  }
} }
{ "id": 30, "result": { "turn": { "id": "turn_456", "status": "inProgress", "items": [], "error": null } } }
```

要使用客户端运行的工具的输出开始回合，请传递带有非空 `name`、可选 `namespace` 和 `output` 字符串或内容项数组的 `toolOutput`。将`input`设置为空数组；您不能将 `toolOutput` 与非空用户输入结合起来。

```json
{
  "method": "turn/start",
  "id": 31,
  "params": {
    "threadId": "thr_123",
    "input": [],
    "toolOutput": {
      "name": "run_tests",
      "namespace": null,
      "output": "All 42 tests passed."
    }
  }
}
```

输出仍然是对话中的工具输出，并在通知和持久历史记录中显示为 `functionCallOutput` 项目。如果常规轮次已处于活动状态，则 Codex 对该轮次的输出进行排队。

<a id="inject-items-into-a-thread"></a>

### 将项目注入到线程中

使用 `thread/inject_items` 将预构建的响应 API 项目附加到已加载线程的提示历史记录中，而无需启动用户轮次。这些项目将保留到推出并包含在后续模型请求中。

```json
{ "method": "thread/inject_items", "id": 31, "params": {
  "threadId": "thr_123",
  "items": [
    {
      "type": "message",
      "role": "assistant",
      "content": [{ "type": "output_text", "text": "Previously computed context." }]
    }
  ]
} }
{ "id": 31, "result": {} }
```

<a id="steer-an-active-turn"></a>

### 转向主动对话轮次

使用 `turn/steer` 将更多用户输入附加到活动的飞行对话轮次中。

- 包括`expectedTurnId`；它必须与活动回合 ID 匹配。
- 如果线程上没有活动的开启，则请求失败。
- `turn/steer` 不会发出新的 `turn/started` 通知。
- `turn/steer` 不接受对话轮次级别覆盖（`model`、`cwd`、`sandboxPolicy` 或 `outputSchema`）。

```json
{ "method": "turn/steer", "id": 32, "params": {
  "threadId": "thr_123",
  "input": [ { "type": "text", "text": "Actually focus on failing tests first." } ],
  "expectedTurnId": "turn_456"
} }
{ "id": 32, "result": { "turnId": "turn_456" } }
```

<a id="start-a-turn-invoke-a-skill"></a>

### 开始一个回合（调用技能）

通过包含 `$ 显式调用技能<skill-name>旁边有 ` in the text input and adding a `skill` 输入项。

```json
{ "method": "turn/start", "id": 33, "params": {
  "threadId": "thr_123",
  "input": [
    { "type": "text", "text": "$skill-creator Add a new skill for triaging flaky CI and include step-by-step usage." },
    { "type": "skill", "name": "skill-creator", "path": "/Users/me/.codex/skills/skill-creator/SKILL.md" }
  ]
} }
{ "id": 33, "result": { "turn": { "id": "turn_457", "status": "inProgress", "items": [], "error": null } } }
```

<a id="interrupt-a-turn"></a>

### 中断对话轮次

```json
{ "method": "turn/interrupt", "id": 31, "params": { "threadId": "thr_123", "turnId": "turn_456" } }
{ "id": 31, "result": {} }
```

成功后，回合结束时为 `status: "interrupted"`。

<a id="review"></a>

## 评论

`review/start` 为线程运行 Codex 审阅器并流式传输审阅项目。目标包括：

- `uncommittedChanges`
- `baseBranch`（与分支的差异）
- `commit`（查看特定提交）
- `custom`（自由格式指令）

使用 `delivery: "inline"`（默认）在现有线程上运行审核，或使用 `delivery: "detached"` 分叉新的审核线程。

请求/响应示例：

```json
{ "method": "review/start", "id": 40, "params": {
  "threadId": "thr_123",
  "delivery": "inline",
  "target": { "type": "commit", "sha": "1234567deadbeef", "title": "Polish tui colors" }
} }
{ "id": 40, "result": {
  "turn": {
    "id": "turn_900",
    "status": "inProgress",
    "items": [
      { "type": "userMessage", "id": "turn_900", "content": [ { "type": "text", "text": "Review commit 1234567: Polish tui colors" } ] }
    ],
    "error": null
  },
  "reviewThreadId": "thr_123"
} }
```

对于独立审查，请使用 `"delivery": "detached"`。响应的形状相同，但 `reviewThreadId` 将是新评论线程的 id（与原始 `threadId` 不同）。在流式传输审阅轮次之前，服务器还会针对该新线程发出 `thread/started` 通知。

Codex 流式传输通常的 `turn/started` 通知，后跟带有 `enteredReviewMode` 项目的 `item/started`：

```json
{
  "method": "item/started",
  "params": {
    "item": {
      "type": "enteredReviewMode",
      "id": "turn_900",
      "review": "current changes"
    }
  }
}
```

当审阅者完成时，服务器会发出 `item/started` 和 `item/completed`，其中包含带有最终审阅文本的 `exitedReviewMode` 项目：

```json
{
  "method": "item/completed",
  "params": {
    "item": {
      "type": "exitedReviewMode",
      "id": "turn_900",
      "review": "Looks solid overall..."
    }
  }
}
```

使用此通知在您的客户端中呈现审阅者输出。

<a id="process-execution"></a>

## 流程执行

`process/*` 是一个实验性的显式过程控制 API。它需要 `capabilities.experimentalApi = true` 并在 Codex 的沙箱外部运行。仅当您的客户端故意在没有沙箱的情况下公开本地进程控制时才使用它。

使用 `process/spawn` 启动进程并提供 `processHandle`，然后使用该句柄处理 stdin、调整大小和终止请求。通过 `process/outputDelta` 通知输出流，通过 `process/exited` 完成流。

```json
{ "method": "process/spawn", "id": 48, "params": {
  "command": ["python3", "-m", "pytest", "-q"],
  "processHandle": "pytest-1",
  "cwd": "/Users/me/project",
  "tty": true
} }
{ "id": 48, "result": {} }
{ "method": "process/outputDelta", "params": {
  "processHandle": "pytest-1",
  "stream": "stdout",
  "deltaBase64": "Li4u"
} }
{ "method": "process/exited", "params": {
  "processHandle": "pytest-1",
  "exitCode": 0
} }
```

将 `process/writeStdin` 与 `deltaBase64`、`closeStdin` 或两者一起使用来发送输入。使用 `process/resizePty` 进行 PTY 调整大小事件，使用 `process/kill` 终止正在运行的进程。

<a id="command-execution"></a>

## 命令执行

`command/exec` 在服务器沙箱下运行单个命令（`argv` 数组），无需创建线程。

```json
{ "method": "command/exec", "id": 50, "params": {
  "command": ["ls", "-la"],
  "cwd": "/Users/me/project",
  "sandboxPolicy": { "type": "workspaceWrite" },
  "timeoutMs": 10000
} }
{ "id": 50, "result": { "exitCode": 0, "stdout": "...", "stderr": "" } }
```

如果您已经对服务器进程进行沙箱处理并希望 Codex 跳过其自己的沙箱强制执行，请使用 `sandboxPolicy.type = "externalSandbox"`。对于外部沙箱模式，将 `networkAccess` 设置为 `restricted`（默认）或 `enabled`。对于 `readOnly` 和 `workspaceWrite`，请使用与上面所示相同的可选 `access` / `readOnlyAccess` 结构。

注意事项：

- 服务器拒绝空 `command` 阵列。
- `sandboxPolicy` 接受与 `turn/start` 使用的相同形状（例如，`dangerFullAccess`、`readOnly`、`workspaceWrite`、`externalSandbox`）。
- 当省略时，`timeoutMs` 回退到服务器默认值。
- 为 PTY 支持的会话设置 `tty: true`，并在计划跟进 `command/exec/write`、`command/exec/resize` 或 `command/exec/terminate` 时使用 `processId`。
- 设置 `streamStdoutStderr: true` 以在命令运行时接收 `command/exec/outputDelta` 通知。

<a id="read-admin-requirements-configrequirementsread"></a>

### 阅读管理要求 (`configRequirements/read`)

使用 `configRequirements/read` 检查从 `requirements.toml` 和/或 MDM 加载的有效管理要求。

```json
{ "method": "configRequirements/read", "id": 52, "params": {} }
{ "id": 52, "result": {
  "requirements": {
    "allowedApprovalPolicies": ["onRequest", "unlessTrusted"],
    "allowedSandboxModes": ["readOnly", "workspaceWrite"],
    "featureRequirements": {
      "personality": true,
      "unified_exec": false
    },
    "network": {
      "enabled": true,
      "allowedDomains": ["api.openai.com"],
      "allowUnixSockets": ["/tmp/example.sock"],
      "dangerouslyAllowAllUnixSockets": false
    }
  }
} }
```

不配置时，`result.requirements` 为 `null`。有关支持的键和值的详细信息，请参阅 [`requirements.toml`](config-file/config-reference.zh-CN.md#requirementstoml) 上的文档。

<a id="windows-sandbox-setup-windowssandboxsetupstart"></a>

### Windows 沙箱设置 (`windowsSandbox/setupStart`)

自定义 Windows 客户端可以异步触发沙箱设置，而不是阻止启动检查。

```json
{ "method": "windowsSandbox/setupStart", "id": 53, "params": { "mode": "elevated" } }
{ "id": 53, "result": { "started": true } }
```

应用程序服务器在后台启动安装，然后发出完成通知：

```json
{
  "method": "windowsSandbox/setupCompleted",
  "params": { "mode": "elevated", "success": true, "error": null }
}
```

模式：

- `elevated` - 运行提升的 Windows 沙箱安装路径。
- `unelevated` - 运行旧设置/预检路径。

<a id="filesystem"></a>

## 文件系统

v2 文件系统 API 在绝对路径上运行。当客户端需要在文件或目录更改后使 UI 状态无效时，请使用 `fs/watch`。

```json
{ "method": "fs/watch", "id": 54, "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1",
  "path": "/Users/me/project/.git/HEAD"
} }
{ "id": 54, "result": { "path": "/Users/me/project/.git/HEAD" } }
{ "method": "fs/changed", "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1",
  "changedPaths": ["/Users/me/project/.git/HEAD"]
} }
{ "method": "fs/unwatch", "id": 55, "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1"
} }
{ "id": 55, "result": {} }
```

监视文件会针对该文件路径发出 `fs/changed`，包括通过替换或重命名操作提供的更新。

<a id="events"></a>

## 活动

事件通知是服务器启动的线程生命周期、轮次生命周期以及其中的项目的流。启动或恢复线程后，继续读取 `thread/started`、`thread/archived`、`thread/unarchived`、`thread/closed`、`thread/status/changed`、`turn/*`、`item/*` 和 `serverRequest/resolved` 通知的活动传输流。

<a id="notification-opt-out"></a>

### 通知选择退出

客户端可以通过在 `initialize.params.capabilities.optOutNotificationMethods` 中发送确切的方法名称来抑制每个连接的特定通知。

- 仅精确匹配：`item/agentMessage/delta` 仅抑制该方法。
- 未知的方法名称将被忽略。
- 适用于当前的 `thread/*`、`turn/*`、`item/*` 及相关 v2 通知。
- 不适用于请求、响应或错误。

<a id="fuzzy-file-search-events-experimental"></a>

### 模糊文件搜索事件（实验）

模糊文件搜索会话 API 发出每个查询的通知：

- `fuzzyFileSearch/sessionUpdated` - `{ sessionId, query, files }` 与活动查询的当前匹配项。
- `fuzzyFileSearch/sessionCompleted` - 一旦该查询的索引和匹配完成，`{ sessionId }`。

<a id="warning-events"></a>

### 警告事件

- `configWarning` - `{ summary, details?, path?, range? }` 用于可恢复的配置或初始化问题。
- `warning` - `{ threadId?, message }` 用于非致命运行时警告。

<a id="windows-sandbox-setup-events"></a>

### Windows 沙箱设置事件

- `windowsSandbox/setupCompleted` - `windowsSandbox/setupStart` 请求完成后发出 `{ mode, success, error }`。

<a id="turn-events"></a>

### 转事件

- `turn/started` - 带回合 ID 的 `{ turn }`、空 `items` 和 `status: "inProgress"`。
- `turn/completed` - `{ turn }`，其中 `turn.status` 是 `completed`、`interrupted` 或 `failed`；故障编号为`{ error: { message, codexErrorInfo?, additionalDetails? } }`。
- `turn/diff/updated` - `{ threadId, turnId, diff }` 具有跨每个文件更改的最新聚合统一差异。
- `turn/plan/updated` - `{ turnId, explanation?, plan }` 每当智能体人分享或更改其计划时；每个 `plan` 条目都是 `{ step, status }`，`pending`、`inProgress` 或 `completed` 中的 `status`。
- `hook/started` 和 `hook/completed` - 当同步生命周期挂钩启动且其最终运行摘要可用时，`{ threadId, turnId?, run }`。异步挂钩不会发出这些通知。
- `model/safetyBuffering/updated` - `{ threadId, turnId, model, useCases, reasons, showBufferingUi, fasterModel }` 当响应进入瞬态安全缓冲时。
- `model/rerouted` - `{ threadId, turnId, fromModel, toModel, reason }` 当服务将请求路由到另一个模型时。
- `model/verification` - `{ threadId, turnId, verifications }`（当服务需要额外帐户验证时）。
- `thread/tokenUsage/updated` - 活动线程的使用更新。

即使项目事件流式传输，`turn/diff/updated` 和 `turn/plan/updated` 目前也包含空的 `items` 数组。使用 `item/*` 通知作为回合项目的事实来源。

<a id="items"></a>

### 项目

`ThreadItem` 是依次携带响应和 `item/*` 通知的标记联合体。常见的物品类型包括：

- `userMessage` - `{id, content}`，其中 `content` 是用户输入的列表（`text`、`image` 或 `localImage`）。
- `functionCallOutput` - `{id, name, namespace, output}` 用于通过 `turn/start.toolOutput` 提供的独立工具输出。 `namespace` 可以是 `null`。
- `agentMessage` - `{id, text, phase?}` 包含累积的智能体回复。如果存在，`phase` 使用响应 API 线值（`commentary`、`final_answer`）。
- `plan` - `{id, text}` 包含计划模式下建议的计划文本。将 `item/completed` 中的最终 `plan` 项目视为权威。
- `reasoning` - `{id, summary, content}`，其中 `summary` 保存流式推理摘要，`content` 保存原始推理块。
- `commandExecution` - `{id, command, cwd, status, commandActions, aggregatedOutput?, exitCode?, durationMs?}`。
- `fileChange` - `{id, changes, status}` 描述建议的编辑； `changes` 列出 `{path, kind, diff}`。
- `mcpToolCall` - `{id, server, tool, status, arguments, appContext?, pluginId?, result?, error?}`。对于受信任的 MCP 应用程序，`appContext` 可以包括 `connectorId`、`linkId`、`resourceUri`、`appName`、`templateId` 和稳定连接器 `actionName`。较旧的持久项目可以忽略较新的元数据。使用 `appContext.resourceUri` 代替已弃用的顶级 `mcpAppResourceUri`。
- `dynamicToolCall` - `{id, tool, arguments, status, contentItems?, success?, durationMs?}` 用于客户端执行的动态工具调用。
- `collabToolCall` - `{id, tool, status, senderThreadId, receiverThreadId?, newThreadId?, prompt?, agentStatus?}`。
- `webSearch` - `{id, query, action?}` 用于智能体发出的 Web 搜索请求。
- `imageView` - 当智能体调用图像查看器工具时发出 `{id, path}`。
- `enteredReviewMode` - `{id, review}` 在审阅者开始时发送。
- `exitedReviewMode` - 审阅者完成时发出 `{id, review}`。
- `contextCompaction` - Codex 压缩对话历史记录时发出 `{id}`。

对于 `webSearch.action`，动作 `type` 可以是 `search`（`query?`、`queries?`）、`openPage`（`url?`）或 `findInPage`（`url?`） `pattern?`）。

应用程序服务器弃用了旧版 `thread/compacted` 通知；请改用 `contextCompaction` 项目。

所有项目都会发出两个共享生命周期事件：

- `item/started` - 当新的工作单元开始时发出完整的 `item` ； `item.id` 与 Delta 使用的 `itemId` 匹配。
- `item/completed` - 工作完成后发送最终的 `item`；将此视为权威状态。

<a id="item-deltas"></a>

### 项目增量

- `item/agentMessage/delta` - 附加智能体消息的流文本。
- `item/plan/delta` - 流提议的计划文本。最终的 `plan` 项可能不完全等于串联的增量。
- `item/reasoning/summaryTextDelta` - 流可读的推理摘要；当新的摘要部分打开时，`summaryIndex` 会递增。
- `item/reasoning/summaryPartAdded` - 标记推理摘要部分之间的边界。
- `item/reasoning/textDelta` - 流原始推理文本（当模型支持时）。
- `item/commandExecution/outputDelta` - 流命令的标准输出/标准错误；按顺序附加增量。
- `item/fileChange/outputDelta` - 已弃用旧版 `apply_patch` 文本输出的兼容性通知。当前的应用程序服务器版本不再发出它；使用 `fileChange` 物品和 `turn/diff/updated` 代替。

<a id="errors"></a>

## 错误

如果回合失败，服务器会发出 `{ error: { message, codexErrorInfo?, additionalDetails? } }` 的 `error` 事件，然后以 `status: "failed"` 结束回合。当上游 HTTP 状态可用时，它会显示在 `codexErrorInfo.httpStatusCode` 中。

常见的 `codexErrorInfo` 值包括：

- `ContextWindowExceeded`
- `UsageLimitExceeded`
- `HttpConnectionFailed`（4xx/5xx 上游错误）
- `ResponseStreamConnectionFailed`
- `ResponseStreamDisconnected`
- `ResponseTooManyFailedAttempts`
- `BadRequest`、`Unauthorized`、`SandboxError`、`InternalServerError`、`Other`

当上游 HTTP 状态可用时，服务器在相关 `codexErrorInfo` 变体上的 `httpStatusCode` 中转发它。

<a id="approvals"></a>

## 批准

根据用户的 Codex 设置，命令执行和文件更改可能需要批准。应用程序服务器向客户端发送服务器发起的 JSON-RPC 请求，客户端以决策负载进行响应。

- 命令执行决策：`accept`、`acceptForSession`、`decline`、`cancel` 或 `{ "acceptWithExecpolicyAmendment": { "execpolicy_amendment": ["cmd", "..."] } }`。
- 文件更改决策：`accept`、`acceptForSession`、`decline`、`cancel`。

- 请求包括 `threadId` 和 `turnId` - 使用它们将 UI 状态范围限定为活动对话。
- 服务器恢复或拒绝工作并以 `item/completed` 结束该项目。

<a id="command-execution-approvals"></a>

### 命令执行批准

消息顺序：

1. `item/started` 显示待处理的 `commandExecution` 项目以及 `command`、`cwd` 和其他字段。
2. `item/commandExecution/requestApproval` 包括 `itemId`、`threadId`、`turnId`、可选的 `reason`、可选的 `command`、可选的 `cwd`、可选的 `commandActions`、可选的 `proposedExecpolicyAmendment`、可选的 `networkApprovalContext` 和可选`availableDecisions`。当 `initialize.params.capabilities.experimentalApi = true` 时，有效负载还可以包括描述所请求的每命令沙箱访问的实验性 `additionalPermissions`。 `additionalPermissions` 内的任何文件系统路径在线路上都是绝对的。
3. 客户端以上述命令执行批准决策之一进行响应。
4. `serverRequest/resolved` 确认待处理请求已得到答复或清除。
5. `item/completed` 使用 `status: completed | failed | declined` 返回最终的 `commandExecution` 项。

当 `networkApprovalContext` 存在时，提示是受管理的网络访问（不是一般的 shell 命令批准）。当前的 v2 架构公开了目标 `host` 和 `protocol`；客户端应该呈现特定于网络的提示符，而不是依赖 `command` 作为对用户有意义的 shell 命令预览。

Codex 按目标（`host`、协议和端口）对并发网络批准提示进行分组。因此，应用程序服务器可能会发送一个提示，以解除对同一目的地的多个排队请求的阻止，而同一主机上的不同端口将被单独处理。

<a id="file-change-approvals"></a>

### 文件变更审批

消息顺序：

1. `item/started` 发出 `fileChange` 项目以及建议的 `changes` 和 `status: "inProgress"`。
2. `item/fileChange/requestApproval` 包括 `itemId`、`threadId`、`turnId`、可选 `reason` 和可选 `grantRoot`。
3. 客户以上述文件变更批准决定之一进行响应。
4. `serverRequest/resolved` 确认待处理请求已得到答复或清除。
5. `item/completed` 使用 `status: completed | failed | declined` 返回最终的 `fileChange` 项。

<a id="toolrequestuserinput"></a>

### `tool/requestUserInput`

当客户端响应 `item/tool/requestUserInput` 时，应用程序服务器会发出 `serverRequest/resolved` 和 `{ threadId, requestId }`。如果在客户端应答之前通过轮次开始、轮次完成或轮次中断清除了挂起的请求，则服务器会针对该清理发出相同的通知。

请求参数包括 `autoResolutionMs` 作为整数毫秒超时或 `null`。如果存在，如果用户没有应答，主机客户端可以在该时间间隔后自动解决提示。

<a id="permission-requests"></a>

### 权限请求

内置 `request_permissions` 工具发送 `item/permissions/requestApproval` 以及 `threadId`、`turnId`、`itemId`、`environmentId`、`cwd`、可选的 `reason` 以及请求的网络或文件系统权限。使用仅包含授予的子集的 `permissions` 进行响应。将 `scope` 设置为 `"session"` 以在同一会话中的后续轮次中保留授予；忽略它或使用 `"turn"` 获得回合范围的补助金。未请求的权限将被忽略。

<a id="mcp-server-elicitation-requests"></a>

### MCP 服务器引发请求

MCP 服务器可以用 `mcpServer/elicitation/request` 中断回合。该请求包括 `threadId`、可选的 `turnId`、`serverName` 以及以下请求形状之一：

- `mode: "form"` 或 `mode: "openai/form"`，以及 `message` 和 `requestedSchema`。
- `mode: "url"`、`message`、`url` 和 `elicitationId`。

使用 `action: "accept"` 和请求的 `content` 进行响应，或者使用 `action: "decline"` 或 `"cancel"` 和 `content: null` 进行响应。然后应用程序服务器发出 `serverRequest/resolved`。要接收 `openai/form` 变体，请选择加入 `initialize.params.capabilities.mcpServerOpenaiFormElicitation`。

<a id="dynamic-tool-calls-experimental"></a>

### 动态工具调用（实验性）

`thread/start` 上的 `dynamicTools` 以及相应的 `item/tool/call` 请求或响应流程是实验性 API。

动态工具名称和命名空间名称必须遵循 Responses API 命名约束。避免内置 Codex 工具使用保留的命名空间名称。

当在回合中调用动态工具时，应用程序服务器会发出：

1. `item/started` 与 `item.type = "dynamicToolCall"`、`status = "inProgress"` 以及 `tool` 和 `arguments`。
2. `item/tool/call`作为服务器向客户端发出请求。
3. 带有返回内容项的客户端响应负载。
4. `item/completed` 与 `item.type = "dynamicToolCall"`、最终的 `status` 以及任何返回的 `contentItems` 或 `success` 值。

<a id="mcp-tool-call-approvals-apps"></a>

### MCP 工具调用批准（应用程序）

应用程序（连接器）工具调用也可能需要批准。当应用程序工具调用有副作用时，服务器可能会使用 `tool/requestUserInput` 和 **接受**、**拒绝** 和 **取消** 等选项来引发批准。即使该工具还公布了特权较低的提示，破坏性工具注释也始终会触发批准。如果用户拒绝或取消，相关的 `mcpToolCall` 项目将完成并出现错误，而不是运行该工具。

<a id="skills"></a>

## 技能

通过包含`$来调用技能<skill-name>` in the user text input. Add a `skill` 输入项（推荐），以便服务器注入完整的技能指令，而不是依赖模型来解析名称。

```json
{
  "method": "turn/start",
  "id": 101,
  "params": {
    "threadId": "thread-1",
    "input": [
      {
        "type": "text",
        "text": "$skill-creator Add a new skill for triaging flaky CI."
      },
      {
        "type": "skill",
        "name": "skill-creator",
        "path": "/Users/me/.codex/skills/skill-creator/SKILL.md"
      }
    ]
  }
}
```

如果省略 `skill` 项，模型仍会解析 `$<skill-name>` 标记并尝试找到该技能，这可能会增加延迟。

示例：

```
$skill-creator Add a new skill for triaging flaky CI and include step-by-step usage.
```

使用 `skills/list` 获取可用技能（可以选择以 `cwds` 为范围，使用 `forceReload`）。您还可以包含 `perCwdExtraUserRoots` 来扫描额外的绝对路径，作为特定 `cwd` 值的 `user` 范围。应用程序服务器会忽略 `cwds` 中不存在 `cwd` 的条目。 `skills/list` 可以重用每个 `cwd` 的缓存结果；设置 `forceReload: true` 从磁盘刷新。如果存在，服务器会从 `SKILL.json` 读取 `interface` 和 `dependencies`。

```json
{ "method": "skills/list", "id": 25, "params": {
  "cwds": ["/Users/me/project", "/Users/me/other-project"],
  "forceReload": true,
  "perCwdExtraUserRoots": [
    {
      "cwd": "/Users/me/project",
      "extraUserRoots": ["/Users/me/shared-skills"]
    }
  ]
} }
{ "id": 25, "result": {
  "data": [{
    "cwd": "/Users/me/project",
    "skills": [
      {
        "name": "skill-creator",
        "description": "Create or update a Codex skill",
        "enabled": true,
        "interface": {
          "displayName": "Skill Creator",
          "shortDescription": "Create or update a Codex skill"
        },
        "dependencies": {
          "tools": [
            {
              "type": "env_var",
              "value": "GITHUB_TOKEN",
              "description": "GitHub API token"
            },
            {
              "type": "mcp",
              "value": "github",
              "transport": "streamable_http",
              "url": "https://example.com/mcp"
            }
          ]
        }
      }
    ],
    "errors": []
  }]
} }
```

当看到本地技能文件发生变化时，服务器还会发出 `skills/changed` 通知。将此视为无效信号，并在需要时使用当前参数重新运行 `skills/list`。

要按路径启用或禁用技能：

```json
{
  "method": "skills/config/write",
  "id": 26,
  "params": {
    "path": "/Users/me/.codex/skills/skill-creator/SKILL.md",
    "enabled": false
  }
}
```

<a id="apps-connectors"></a>

## 应用程序（连接器）

使用 `app/installed` 读取最新提交的已安装应用程序运行时快照。每个结果包括应用程序 `id`、`runtimeName`（或 `null`）、有效 `enabled` 状态和 `callable` 状态。仅当有效配置启用并且至少一个模型可见工具符合应用程序和工具策略时，应用程序才可调用。

```json
{
  "method": "app/installed",
  "id": 49,
  "params": {
    "threadId": "thread-1",
    "forceRefresh": false
  }
}
{
  "id": 49,
  "result": {
    "apps": [
      {
        "id": "demo-app",
        "runtimeName": "Demo App",
        "enabled": true,
        "callable": true
      }
    ]
  }
}
```

省略 `threadId` 以使用全局配置而不是已加载线程的配置。设置 `forceRefresh: true` 以在读取连接器运行时快照之前刷新它。当全局或工作区策略阻止应用程序访问时，观察到的应用程序仍会显示，且 `enabled` 和 `callable` 设置为 `false`。

使用 `app/list` 获取可用的应用程序。在CLI/TUI中，`/apps`是面向用户的选择器；在自定义客户端中，直接调用`app/list`。每个条目都包含 `isAccessible`（用户可用）和 `isEnabled`（在 `config.toml` 中启用），因此客户端可以区分安装/访问与本地启用状态。应用程序条目还可以包括可选的 `branding`、`appMetadata` 和 `labels` 字段。

```json
{ "method": "app/list", "id": 50, "params": {
  "cursor": null,
  "limit": 50,
  "threadId": "thread-1",
  "forceRefetch": false
} }
{ "id": 50, "result": {
  "data": [
    {
      "id": "demo-app",
      "name": "Demo App",
      "description": "Example connector for documentation.",
      "logoUrl": "https://example.com/demo-app.png",
      "logoUrlDark": null,
      "distributionChannel": null,
      "branding": null,
      "appMetadata": null,
      "labels": null,
      "installUrl": "https://chatgpt.com/apps/demo-app/demo-app",
      "isAccessible": true,
      "isEnabled": true
    }
  ],
  "nextCursor": null
} }
```

如果您提供 `threadId`，应用程序功能门控 (`features.apps`) 将使用该线程的配置快照。省略时，应用程序服务器使用最新的全局配置。

`app/list` 在可访问应用程序和目录应用程序加载后返回。设置 `forceRefetch: true` 以绕过应用程序缓存并获取新数据。仅当刷新成功时才会替换缓存条目。

每当源（可访问的应用程序或目录应用程序）完成加载时，服务器还会发出 `app/list/updated` 通知。每个通知都包含最新合并的应用程序列表。

```json
{
  "method": "app/list/updated",
  "params": {
    "data": [
      {
        "id": "demo-app",
        "name": "Demo App",
        "description": "Example connector for documentation.",
        "logoUrl": "https://example.com/demo-app.png",
        "logoUrlDark": null,
        "distributionChannel": null,
        "branding": null,
        "appMetadata": null,
        "labels": null,
        "installUrl": "https://chatgpt.com/apps/demo-app/demo-app",
        "isAccessible": true,
        "isEnabled": true
      }
    ]
  }
}
```

当您已经知道应用程序 ID 并且需要应用程序元数据而不是安装的运行时状态时，请使用 `app/read`。最多通过100个`appIds`。服务器仅保留每个重复 ID 的第一次出现，并在 `apps` 和 `missingAppIds` 中保留该顺序。未知或无法访问的应用程序会在 `missingAppIds` 中返回，而不会导致整个请求失败。

```json
{
  "method": "app/read",
  "id": 52,
  "params": {
    "appIds": ["demo-app", "missing-app"],
    "includeTools": true
  }
}
{
  "id": 52,
  "result": {
    "apps": [
      {
        "id": "demo-app",
        "name": "Demo App",
        "description": "Example connector for documentation.",
        "iconUrl": null,
        "iconUrlDark": null,
        "distributionChannel": null,
        "installUrl": null,
        "pluginDisplayNames": [],
        "toolSummaries": [
          {
            "name": "search",
            "title": "Search",
            "description": "Search the app.",
            "isEnabled": true,
            "disabledReason": null,
            "isReadOnly": true
          }
        ]
      }
    ],
    "missingAppIds": ["missing-app"]
  }
}
```

设置 `includeTools: true` 以请求仅显示的公共工具摘要。元数据响应不包括已安装的应用程序运行时状态或授权工具调用；使用 `app/installed` 检查 `enabled` 和 `callable` 的有效状态。

通过插入“$”来调用应用程序<app-slug>` in the text input and adding a `提及` input item with the `app://<id>` 路径（推荐）。

```json
{
  "method": "turn/start",
  "id": 51,
  "params": {
    "threadId": "thread-1",
    "input": [
      {
        "type": "text",
        "text": "$demo-app Pull the latest updates from the team."
      },
      {
        "type": "mention",
        "name": "Demo App",
        "path": "app://demo-app"
      }
    ]
  }
}
```

<a id="config-rpc-examples-for-app-settings"></a>

### 应用程序设置的配置 RPC 示例

使用 `config/read`、`config/value/write` 和 `config/batchWrite` 检查或更新 `config.toml` 中的应用程序控件。

读取有效的应用程序配置形状（包括 `_default` 和每个工具的覆盖）：

```json
{ "method": "config/read", "id": 60, "params": { "includeLayers": false } }
{ "id": 60, "result": {
  "config": {
    "apps": {
      "_default": {
        "enabled": true,
        "destructive_enabled": true,
        "open_world_enabled": true,
        "approvals_reviewer": "user",
        "default_tools_approval_mode": "auto"
      },
      "google_drive": {
        "enabled": true,
        "destructive_enabled": false,
        "approvals_reviewer": "auto_review",
        "default_tools_approval_mode": "prompt",
        "tools": {
          "files/delete": { "enabled": false, "approval_mode": "approve" }
        }
      }
    }
  }
} }
```

`apps._default.approvals_reviewer` 为所有应用程序设置审阅者，除非每个应用程序的值覆盖它。当两者都被省略时，应用程序继承顶级 `approvals_reviewer` 值。 `apps._default.default_tools_approval_mode` 为没有按应用程序或按工具覆盖的工具设置后备批准模式。托管审批模式要求会覆盖工具审批模式设置。

更新单个应用程序设置：

```json
{
  "method": "config/value/write",
  "id": 61,
  "params": {
    "keyPath": "apps.google_drive.default_tools_approval_mode",
    "value": "prompt",
    "mergeStrategy": "replace"
  }
}
```

以原子方式应用多个应用程序编辑：

```json
{
  "method": "config/batchWrite",
  "id": 62,
  "params": {
    "edits": [
      {
        "keyPath": "apps._default.destructive_enabled",
        "value": false,
        "mergeStrategy": "upsert"
      },
      {
        "keyPath": "apps.google_drive.tools.files/delete.approval_mode",
        "value": "approve",
        "mergeStrategy": "upsert"
      }
    ]
  }
}
```

<a id="detect-and-import-external-agent-config"></a>

### 检测并导入外部智能体配置

使用 `externalAgentConfig/detect` 发现可以迁移的外部智能体工件，然后将选定的条目传递给 `externalAgentConfig/import`。

检测示例：

```json
{ "method": "externalAgentConfig/detect", "id": 63, "params": {
  "includeHome": true,
  "cwds": ["/Users/me/project"]
} }
{ "id": 63, "result": {
  "items": [
    {
      "itemType": "AGENTS_MD",
      "description": "Import /Users/me/project/CLAUDE.md to /Users/me/project/AGENTS.md.",
      "cwd": "/Users/me/project"
    },
    {
      "itemType": "SKILLS",
      "description": "Copy skill folders from /Users/me/.claude/skills to /Users/me/.agents/skills.",
      "cwd": null
    }
  ]
} }
```

导入示例：

```json
{ "method": "externalAgentConfig/import", "id": 64, "params": {
  "migrationItems": [
    {
      "itemType": "AGENTS_MD",
      "description": "Import /Users/me/project/CLAUDE.md to /Users/me/project/AGENTS.md.",
      "cwd": "/Users/me/project"
    }
  ],
  "source": "claude-code"
} }
{ "id": 64, "result": { "importId": "8ae96ff3-3425-4f4c-8772-b6fd61502868" } }
```

可选的顶级 `source` 导入参数标记生成所选迁移项目的产品。

服务器在项目类型完成时发出 `externalAgentConfig/import/progress`，并在所有同步和后台导入完成后发出 `externalAgentConfig/import/completed`。这些通知包括来自响应的相同 `importId` 以及 `itemTypeResults` 以及每种类型的 `successes` 和 `failures`。完成可能会在响应后或后台远程导入完成后立即到达。

```json
{ "method": "externalAgentConfig/import/progress", "params": {
  "importId": "8ae96ff3-3425-4f4c-8772-b6fd61502868",
  "itemTypeResults": [
    {
      "itemType": "AGENTS_MD",
      "successes": [
        { "itemType": "AGENTS_MD", "cwd": "/Users/me/project", "source": null, "target": "/Users/me/project/AGENTS.md" }
      ],
      "failures": []
    }
  ]
} }
{ "method": "externalAgentConfig/import/completed", "params": {
  "importId": "8ae96ff3-3425-4f4c-8772-b6fd61502868",
  "itemTypeResults": [
    {
      "itemType": "AGENTS_MD",
      "successes": [
        { "itemType": "AGENTS_MD", "cwd": "/Users/me/project", "source": null, "target": "/Users/me/project/AGENTS.md" }
      ],
      "failures": []
    }
  ]
} }
```

阅读之前完成的导入：

```json
{ "method": "externalAgentConfig/import/readHistories", "id": 65 }
{ "id": 65, "result": { "data": [
  {
    "importId": "8ae96ff3-3425-4f4c-8772-b6fd61502868",
    "completedAtMs": 1781784000000,
    "successes": [
      { "itemType": "AGENTS_MD", "cwd": "/Users/me/project", "source": null, "target": "/Users/me/project/AGENTS.md" }
    ],
    "failures": []
  }
] } }
```

支持的 `itemType` 值为 `AGENTS_MD`、`CONFIG`、`SKILLS`、`PLUGINS`、`MCP_SERVER_CONFIG`、`SUBAGENTS`、`HOOKS`、`COMMANDS` 和 `SESSIONS`。对于 `PLUGINS` 项目，`details.plugins` 列出每个 `marketplaceName` 和 `pluginNames` Codex 可以尝试迁移。检测仅返回仍有工作要做的项目。例如，当 `AGENTS.md` 已存在且非空时，Codex 会跳过 AGENTS 迁移，并且技能导入不会覆盖现有技能目录。

当检测来自 `.claude/settings.json` 的插件时，Codex 会从 `extraKnownMarketplaces` 读取配置的市场源。如果 `enabledPlugins` 包含来自 `claude-plugins-official` 的插件但缺少市场源，则 Codex 会推断 `anthropics/claude-plugins-official` 为源。

<a id="auth-endpoints"></a>

## 身份验证端点

JSON-RPC 身份验证/帐户使用界面公开请求/响应方法以及服务器启动的通知（无 `id`）。使用这些来确定身份验证状态、启动或取消登录、注销、检查 ChatGPT 速率限制，并通知工作区所有者有关耗尽的额度或使用限制。

<a id="authentication-modes"></a>

### 认证方式

Codex支持这些认证方式。 `account/updated.authMode` 显示活动模式，并包括当前的 ChatGPT `planType`（如果可用）。 `account/read` 还报告帐户和计划详细信息。

- **API 密钥 (`apikey`)** - 调用者通过 `type: "apiKey"` 提供 OpenAI API 密钥，Codex 存储它以用于 API 请求。
- **ChatGPT 托管 (`chatgpt`)** - Codex 拥有 ChatGPT OAuth 流程，保留令牌并自动刷新它们。从浏览器流程的 `type: "chatgpt"` 开始，或从设备代码流程的 `type: "chatgptDeviceCode"` 开始。
- **ChatGPT 外部令牌 (`chatgptAuthTokens`)** - 实验性的，适用于已经拥有用户 ChatGPT 身份验证生命周期的主机应用程序。主机应用程序直接提供 `accessToken`、`chatgptAccountId` 和可选的 `chatgptPlanType`，并且必须在询问时刷新令牌。
- **亚马逊基岩** - `account/read` 将 Bedrock 账户报告为 `type: "amazonBedrock"`，并指示凭证是来自 Codex 管理的 Bedrock API 密钥 (`credentialSource: "codexManaged"`) 还是外部 AWS 凭证链 (`credentialSource: "awsManaged"`)。 `account/updated.authMode` 使用 `bedrockApiKey` 作为 Codex 管理的 Bedrock API 密钥。

<a id="api-overview"></a>

### API概览

- `account/read` - 获取当前帐户信息；可选地刷新令牌。
- `account/login/start` - 开始登录（`apiKey`、`chatgpt`、`chatgptDeviceCode` 或实验性 `chatgptAuthTokens`）。
- `account/login/completed`（通知）- 登录尝试完成（成功或错误）时发出。
- `account/login/cancel` - 取消 `loginId` 待处理的托管 ChatGPT 登录。
- `account/logout` - 注销；触发 `account/updated`。
- `account/updated`（通知）- 每当身份验证模式更改时发出（`authMode`：`apikey`、`chatgpt`、`chatgptAuthTokens`、`agentIdentity`、`personalAccessToken`、`bedrockApiKey` 或 `null`）并包括`planType`（如有）。
- `account/chatgptAuthTokens/refresh`（服务器请求）- 在授权错误后请求新的外部管理的 ChatGPT 令牌。
- `account/rateLimits/read` - 获取 ChatGPT 速率限制。
- `account/rateLimits/updated`（通知）- 每当用户的 ChatGPT 速率限制发生变化时发出。
- `account/sendAddCreditsNudgeEmail` - 要求 ChatGPT 通过电子邮件向工作区所有者发送有关额度耗尽或达到使用限制的信息。
- `account/rateLimitResetCredit/consume` - 使用调用者提供的 `idempotencyKey` 值消耗一次赚取的速率限制重置。
- `account/usage/read` - 获取 ChatGPT 账户Token活动摘要和每日存储桶。
- `account/workspaceMessages/read` - 获取活动工作区消息，包括可用的通知标题。
- `mcpServer/oauthLogin/completed`（通知）- `mcpServer/oauth/login` 流程完成后发出；有效负载包括`{ name, threadId, success, error? }`。对于应用程序范围或插件 OAuth 流，`threadId` 可以是 `null`。
- `mcpServer/startupStatus/updated`（通知）- 当配置的 MCP 服务器的启动状态发生变化时发出；有效负载包括`{ threadId, name, status, error, failureReason }`。 `threadId` 是用于应用程序范围启动的 `null`。启动失败时，`failureReason: "reauthenticationRequired"` 表示存储的 OAuth 凭据已过期且无法刷新，因此客户端应主动重新连接服务器。

<a id="1-check-auth-state"></a>

### 1) 检查授权状态

要求：

```json
{ "method": "account/read", "id": 1, "params": { "refreshToken": false } }
```

响应示例：

```json
{ "id": 1, "result": { "account": null, "requiresOpenaiAuth": false } }
```

```json
{ "id": 1, "result": { "account": null, "requiresOpenaiAuth": true } }
```

```json
{
  "id": 1,
  "result": { "account": { "type": "apiKey" }, "requiresOpenaiAuth": true }
}
```

```json
{
  "id": 1,
  "result": {
    "account": {
      "type": "amazonBedrock",
      "credentialSource": "codexManaged"
    },
    "requiresOpenaiAuth": false
  }
}
```

```json
{
  "id": 1,
  "result": {
    "account": {
      "type": "amazonBedrock",
      "credentialSource": "awsManaged"
    },
    "requiresOpenaiAuth": false
  }
}
```

```json
{
  "id": 1,
  "result": {
    "account": {
      "type": "chatgpt",
      "email": "user@example.com",
      "planType": "pro"
    },
    "requiresOpenaiAuth": true
  }
}
```

现场笔记：

- `refreshToken`（布尔值）：设置 `true` 以在托管 ChatGPT 模式下强制刷新令牌。在外部令牌模式（`chatgptAuthTokens`）下，应用程序服务器忽略此标志。
- 当 ChatGPT 帐户没有电子邮件地址时，`email` 为 `null`。
- `requiresOpenaiAuth` 反映活跃提供商；当 `false`、Codex 可以在没有 OpenAI 凭据的情况下运行。
- 当 Amazon Bedrock 使用 Codex 管理的 Bedrock API 密钥时，它会报告 `credentialSource: "codexManaged"`。它报告外部 AWS 凭证路径的 `credentialSource: "awsManaged"`。这标识了所选的凭证来源；它不验证 AWS 凭证链是否可以解析凭证。

<a id="2-log-in-with-an-api-key"></a>

### 2) 使用 API 密钥登录

1. 发送：

```json
   {
     "method": "account/login/start",
     "id": 2,
     "params": { "type": "apiKey", "apiKey": "sk-..." }
   }
```

2. 期望：

```json
   { "id": 2, "result": { "type": "apiKey" } }
```

3. 通知：

```json
   {
     "method": "account/login/completed",
     "params": { "loginId": null, "success": true, "error": null }
   }
```

```json
   {
     "method": "account/updated",
     "params": { "authMode": "apikey", "planType": null }
   }
```

<a id="3-log-in-with-chatgpt-browser-flow"></a>

### 3）使用ChatGPT登录（浏览器流程）

1. 开始：

```json
   {
     "method": "account/login/start",
     "id": 3,
     "params": {
       "type": "chatgpt",
       "useHostedLoginSuccessPage": true,
       "appBrand": "chatgpt"
     }
   }
```

默认情况下，成功的浏览器回调会重定向到本地成功页面。将 `useHostedLoginSuccessPage: true` 设置为在不需要组织设置时使用托管成功页面。启用托管成功后，`appBrand` 可以是 `"codex"` 或 `"chatgpt"`；省略或 `null` 值默认为 `"codex"`。

```json
   {
     "id": 3,
     "result": {
       "type": "chatgpt",
       "loginId": "<uuid>",
       "authUrl": "https://chatgpt.com/...&redirect_uri=http%3A%2F%2Flocalhost%3A<port>%2Fauth%2Fcallback"
     }
   }
```

2. 在浏览器中打开`authUrl`；应用程序服务器托管本地回调。
3. 等待通知：

```json
   {
     "method": "account/login/completed",
     "params": { "loginId": "<uuid>", "success": true, "error": null }
   }
```

```json
   {
     "method": "account/updated",
     "params": { "authMode": "chatgpt", "planType": "plus" }
   }
```

<a id="3b-log-in-with-chatgpt-device-code-flow"></a>

### 3b) 使用 ChatGPT 登录（设备代码流程）

当您的客户端拥有登录仪式或浏览器回调很脆弱时，请使用此流程。

1. 开始：

```json
   {
     "method": "account/login/start",
     "id": 4,
     "params": { "type": "chatgptDeviceCode" }
   }
```

```json
   {
     "id": 4,
     "result": {
       "type": "chatgptDeviceCode",
       "loginId": "<uuid>",
       "verificationUrl": "https://auth.openai.com/codex/device",
       "userCode": "ABCD-1234"
     }
   }
```

2. 向用户显示`verificationUrl`和`userCode`；前端拥有用户体验。
3. 等待通知：

```json
   {
     "method": "account/login/completed",
     "params": { "loginId": "<uuid>", "success": true, "error": null }
   }
```

```json
   {
     "method": "account/updated",
     "params": { "authMode": "chatgpt", "planType": "plus" }
   }
```

<a id="3c-log-in-with-externally-managed-chatgpt-tokens-chatgptauthtokens"></a>

### 3c) 使用外部管理的 ChatGPT Token (`chatgptAuthTokens`) 登录

仅当主机应用程序拥有用户的 ChatGPT 身份验证生命周期并直接提供令牌时，才使用此实验模式。在使用此登录类型之前，客户端必须在 `initialize` 期间设置 `capabilities.experimentalApi = true`。

1. 发送：

```json
   {
     "method": "account/login/start",
     "id": 7,
     "params": {
       "type": "chatgptAuthTokens",
       "accessToken": "<jwt>",
       "chatgptAccountId": "org-123",
       "chatgptPlanType": "business"
     }
   }
```

2. 期望：

```json
   { "id": 7, "result": { "type": "chatgptAuthTokens" } }
```

3. 通知：

```json
   {
     "method": "account/login/completed",
     "params": { "loginId": null, "success": true, "error": null }
   }
```

```json
   {
     "method": "account/updated",
     "params": { "authMode": "chatgptAuthTokens", "planType": "business" }
   }
```

当服务器收到 `401 Unauthorized` 时，它可能会向主机应用程序请求刷新的令牌：

```json
{
  "method": "account/chatgptAuthTokens/refresh",
  "id": 8,
  "params": { "reason": "unauthorized", "previousAccountId": "org-123" }
}
{ "id": 8, "result": { "accessToken": "<jwt>", "chatgptAccountId": "org-123", "chatgptPlanType": "business" } }
```

服务器在成功刷新响应后重试原始请求。请求大约 10 秒后超时。

<a id="4-cancel-a-chatgpt-login"></a>

### 4) 取消ChatGPT登录

```json
{ "method": "account/login/cancel", "id": 4, "params": { "loginId": "<uuid>" } }
{ "method": "account/login/completed", "params": { "loginId": "<uuid>", "success": false, "error": "..." } }
```

<a id="5-logout"></a>

### 5) 退出

```json
{ "method": "account/logout", "id": 5 }
{ "id": 5, "result": {} }
{ "method": "account/updated", "params": { "authMode": null, "planType": null } }
```

<a id="6-rate-limits-chatgpt"></a>

### 6）速率限制（ChatGPT）

```json
{ "method": "account/rateLimits/read", "id": 6 }
{ "id": 6, "result": {
  "rateLimits": {
    "limitId": "codex",
    "limitName": null,
    "primary": { "usedPercent": 25, "windowDurationMins": 15, "resetsAt": 1730947200 },
    "secondary": null,
    "rateLimitReachedType": null
  },
  "rateLimitsByLimitId": {
    "codex": {
      "limitId": "codex",
      "limitName": null,
      "primary": { "usedPercent": 25, "windowDurationMins": 15, "resetsAt": 1730947200 },
      "secondary": null,
      "rateLimitReachedType": null
    },
    "codex_other": {
      "limitId": "codex_other",
      "limitName": "codex_other",
      "primary": { "usedPercent": 42, "windowDurationMins": 60, "resetsAt": 1730950800 },
      "secondary": null,
      "rateLimitReachedType": null
    }
  },
  "rateLimitResetCredits": {
    "availableCount": 2,
    "credits": [{
      "id": "RateLimitResetCredit_1",
      "resetType": "codexRateLimits",
      "status": "available",
      "grantedAt": 1781654400,
      "expiresAt": 1784246400,
      "title": "Rate-limit reset",
      "description": "Reset an eligible Codex rate-limit window."
    }]
  }
} }
{ "method": "account/rateLimits/updated", "params": {
  "rateLimits": {
    "limitId": "codex",
    "primary": { "usedPercent": 31, "windowDurationMins": 15, "resetsAt": 1730948100 }
  }
} }
```

现场笔记：

- `rateLimits` 是向后兼容的单桶视图。
- `rateLimitsByLimitId`（如果存在）是由计量的 `limit_id`（例如 `codex`）键入的多存储桶视图。
- `limitId` 是计量桶标识符。
- `limitName` 是可选的面向用户的铲斗标签。
- `usedPercent` 是配额窗口内的当前使用情况。
- `windowDurationMins` 是配额窗口长度。
- `resetsAt` 是下次重置的 Unix 时间戳（秒）。
- 当服务器返回与存储桶关联的 ChatGPT 计划时，会包含 `planType`。
- 当服务器返回剩余工作区额度详细信息时，将包含 `credits`。
- 当达到某一限制时，`rateLimitReachedType` 标识服务器分类的限制状态。
- `rateLimitResetCredits` 包含服务提供时可用的赢得重置计数；否则为 `null`。
- 当仅知道计数时，`rateLimitResetCredits.credits` 是 `null`。空数组意味着服务获取了详细信息并且没有返回可用的额度。该服务可以限制详细信息行，因此 `availableCount` 具有权威性。
- 每个详细信息行包括不透明的 `id`、`resetType`、`status`、`grantedAt`、`expiresAt`（可以是 `null`）、`title`（可以是 `null`）和 `description`（可以是 `null`）。
- 消耗复位后获取 `account/rateLimits/read`。

<a id="7-token-usage-chatgpt"></a>

### 7）Token使用（ChatGPT）

使用 `account/usage/read` 获取 ChatGPT Token活动摘要字段和可选的每日存储桶。

```json
{ "method": "account/usage/read", "id": 7 }
{ "id": 7, "result": {
  "summary": {
    "lifetimeTokens": 1234567,
    "peakDailyTokens": 45678,
    "longestRunningTurnSec": 540,
    "currentStreakDays": 8,
    "longestStreakDays": 14
  },
  "dailyUsageBuckets": [
    { "startDate": "2026-06-18", "tokens": 12345 }
  ]
} }
```

现场笔记：

- 当服务未返回该指标时，`summary` 值可能是 `null`。
- `dailyUsageBuckets`可能是`null`；如果存在，每个桶包括 `startDate` 和 `tokens`。
- 端点需要 Codex 服务支持的身份验证。 ChatGPT、外部 ChatGPT 令牌、智能体身份和个人访问令牌身份验证工作；仅 API 密钥，而 Bedrock 身份验证则不然。

<a id="8-earned-rate-limit-resets-chatgpt"></a>

### 8) 赚取速率限制重置(ChatGPT)

使用 `account/rateLimitResetCredit/consume` 消耗一次获得的重置。

```json
{ "method": "account/rateLimitResetCredit/consume", "id": 8, "params": { "idempotencyKey": "8ae96ff3-3425-4f4c-8772-b6fd61502868", "creditId": "RateLimitResetCredit_1" } }
{ "id": 8, "result": { "outcome": "reset" } }
```

现场笔记：

- `idempotencyKey` 必须非空。对每个逻辑兑换尝试使用 UUID，并在重试该尝试时重复使用相同的值。
- `creditId` 是可选的。如果提供，它必须是 `account/rateLimits/read` 中的非空不透明 ID。省略时，服务会选择下一个可用积分。
- `reset` 表示已消耗积分。
- `alreadyRedeemed` 表示之前完成的相同兑换。将其视为幂等成功并刷新帐户限制。
- `nothingToReset` 表示没有符合条件的速率限制窗口可以重置。
- `noCredit` 表示该帐户没有可用的重置额度。
- 使用重置后获取 `account/rateLimits/read`，而不是从此响应推断更新的窗口。

<a id="9-notify-a-workspace-owner-about-a-limit"></a>

### 9) 通知工作区所有者有关限制

使用 `account/sendAddCreditsNudgeEmail` 要求 ChatGPT 在额度耗尽或达到使用限制时向工作区所有者发送电子邮件。

```json
{ "method": "account/sendAddCreditsNudgeEmail", "id": 9, "params": { "creditType": "credits" } }
{ "id": 9, "result": { "status": "sent" } }
```

当工作区额度耗尽时使用 `creditType: "credits"`，或者当达到工作区使用限制时使用 `creditType: "usage_limit"`。如果最近已通知所有者，则响应状态为 `cooldown_active`。

<a id="10-workspace-messages-chatgpt"></a>

### 10) 工作区消息(ChatGPT)

使用 `account/workspaceMessages/read` 获取当前工作区的活动消息，包括可用的通知标题。

```json
{ "method": "account/workspaceMessages/read", "id": 10 }
{ "id": 10, "result": { "featureEnabled": true, "messages": [
  { "messageId": "msg_123", "messageType": "headline", "messageBody": "Workspace maintenance starts at 5pm.", "createdAt": 1781395200, "archivedAt": null }
] } }
```