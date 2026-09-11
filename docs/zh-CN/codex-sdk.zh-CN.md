> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/codex-sdk.md)。

<a id="codex-sdk"></a>

# Codex SDK

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

如果您通过 Codex CLI、IDE 扩展或 Codex 云使用 Codex，您还可以通过编程方式控制它。

当您需要执行以下操作时，请使用 SDK：

- 控制 Codex 作为 CI/CD 管道的一部分
- 创建您自己的智能体，可以与 Codex 交互来执行复杂的工程任务
- 将 Codex 构建到您自己的内部工具和工作流程中
- 将 Codex 集成到您自己的应用程序中

使用 Codex SDK 自动执行编码任务，包括 CI 中的作业。使用 [Codex 应用服务器](app-server.zh-CN.md) 构建处理身份验证、对话历史记录、批准和流智能体事件的自定义客户端。

`codex mcp-server` 命令和独立的 `codex-mcp-server` 二进制文件已被删除。使用 [Codex 应用服务器](app-server.zh-CN.md) 进行现有集成。

如果您具有测试版访问权限并需要具有结构化安全结果和覆盖范围的仓库或更改扫描，请使用 [Security TypeScript SDK](security/sdk.zh-CN.md)。

<a id="typescript-library"></a>

## TypeScript 库

TypeScript 库允许您的应用程序启动、继续和恢复本地 Codex 线程。

使用库服务器端；它需要 Node.js 18 或更高版本。

<a id="installation"></a>

### 安装

首先，使用 `npm` 安装 Codex SDK：

```bash
npm install @openai/codex-sdk
```

<a id="usage"></a>

### 用途

使用 Codex 启动一个线程并根据提示运行它。

```ts


const codex = new Codex();
const thread = codex.startThread();
const result = await thread.run(
  "Make a plan to diagnose and fix the CI failures"
);

console.log(result.finalResponse);
```

再次调用 `run()` 以继续同一线程，或通过提供线程 ID 来恢复过去的线程。

```ts
// 运行同一个线程
const result = await thread.run("Implement the plan");

console.log(result.finalResponse);

// 恢复过去的线程

const threadId = "<thread-id>";
const thread2 = codex.resumeThread(threadId);
const result2 = await thread2.run("Pick up where you left off");

console.log(result2.finalResponse);
```

有关更多详细信息，请查看 [TypeScript 仓库](https://github.com/openai/codex/tree/main/sdk/typescript)。

<a id="python-library"></a>

## Python库

Python SDK 通过 JSON-RPC 控制本地 Codex 应用程序服务器。它需要 Python 3.10 或更高版本。已发布的 SDK 版本包含固定的 Codex CLI 运行时依赖项。

<a id="installation"></a>

### 安装

要安装 SDK，请运行：

```bash
pip install openai-codex
```

已发布的 SDK 版本会自动使用其固定的运行时。仅当您有意要针对特定​​本地 Codex 可执行文件运行时才传递 `CodexConfig(codex_bin=...)`。

Python SDK 作为稳定版本提供。 `pip install openai-codex` 安装最新的稳定版本。使用 `pip install --pre openai-codex` 选择加入较新的预发布版本。

<a id="usage"></a>

### 用途

启动Codex，创建一个线程，并运行提示符：

```python
from openai_codex import Codex, Sandbox

with Codex() as codex:
    thread = codex.thread_start(
        model="gpt-5.6-terra",
        sandbox=Sandbox.workspace_write,
    )
    result = thread.run("Make a plan to diagnose and fix the CI failures")
    print(result.final_response)
```

当您的应用程序已经异步时，请使用 `AsyncCodex`：

```python
import asyncio

from openai_codex import AsyncCodex


async def main() -> None:
    async with AsyncCodex() as codex:
        thread = await codex.thread_start(model="gpt-5.6-terra")
        result = await thread.run("Implement the plan")
        print(result.final_response)


asyncio.run(main())
```

<a id="sandbox-presets"></a>

### 沙箱预设

创建线程或更改其文件系统访问权限以供稍后使用时，请使用相同的 `Sandbox` 预设：

```python
from openai_codex import Codex, Sandbox

with Codex() as codex:
    thread = codex.thread_start(sandbox=Sandbox.workspace_write)
    thread.run("Make the requested change.")
    review = thread.run("Review the diff only.", sandbox=Sandbox.read_only)
```

可用预设：

- `Sandbox.read_only`：读取文件但不允许写入。
- `Sandbox.workspace_write`：在工作区和配置的可写根目录内读取文件并写入。
- `Sandbox.full_access`：在没有文件系统访问限制的情况下运行。

当您省略 `sandbox=` 时，应用程序服务器将使用其配置的默认值。传递到 `run(...)` 或 `turn(...)` 的沙箱适用于该轮次，并随后在线程上轮次。

有关更多详细信息，请查看 [Python 仓库](https://github.com/openai/codex/tree/main/sdk/python)。