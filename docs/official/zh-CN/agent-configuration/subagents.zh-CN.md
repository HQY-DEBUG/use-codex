> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/agent-configuration/subagents.md)。

<a id="subagents"></a>

# 子智能体

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT Work 和 Codex 可以通过并行生成专用智能体然后在一个响应中收集其结果来运行子智能体工作流程。这对于高度并行的复杂任务特别有帮助，例如代码库探索或实施多步骤功能计划。

在本地Codex客户端中，您还可以为不同的任务定义具有不同模型配置和指令的自定义智能体。

<a id="availability"></a>

## 可用性

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT Work 向符合条件的账户公开子智能体工作流程和活动。

</ContentModeSwitch>

<a id="custom-agents"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

当前的 Codex 版本默认启用子智能体工作流程。子智能体活动显示在 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中。

</ContentModeSwitch>

由于每个子智能体执行自己的模型和工具工作，因此子智能体工作流比同类单智能体运行消耗更多的令牌。

<ContentModeSwitch group="codex-surface" id="web">

在 ChatGPT Work 中，请求 ChatGPT 将独立工作委派给子智能体。它们在 ChatGPT 的托管环境中运行，对话会显示其活动和结果。在大多数推理级别下，需要明确要求委派；使用 Ultra 时，如果并行智能体能明显提高速度或质量，ChatGPT 可以主动委派工作。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

在应用对话中，请求 Codex 将独立的工作部分委派给子智能体。当前本地 Codex 版本会在你明确提出要求，或适用的 `AGENTS.md` 或技能指令要求委派时这样做。应用会显示各子智能体的对话，便于你查看其工作，以及返回主对话的摘要。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在交互式 CLI 会话中要求 Codex 使用子智能体。 Codex 还可以遵循适用的 `AGENTS.md` 或请求委派的技能说明。使用 `/agent` 在智能体线程运行时检查智能体线程并在它们之间切换。主线程将子智能体结果收集到其最终响应中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

在 IDE 聊天中要求 Codex 将工作的独立部分委托给子智能体。 Codex 还可以遵循适用的 `AGENTS.md` 或请求委派的技能说明。当后台智能体 UI 可用时，活动子智能体会出现在输入框上方。展开面板以查看其状态、停止所有活动子智能体或打开单个子智能体线程。

</ContentModeSwitch>

<a id="why-subagent-workflows-help"></a>

## 为什么子智能体工作流程有帮助

即使有很大的上下文窗口，模型也有局限性。如果您用嘈杂的中间输出（例如探索笔记、测试日志、堆栈跟踪和命令输出）淹没主聊天（在其中定义需求、约束和决策），随着时间的推移，会话的可靠性可能会降低。

这通常被描述为：

- **上下文污染**：有用的信息被淹没在嘈杂的中间输出中。
- **上下文腐烂**：当聊天中充满了不太相关的细节时，性能会下降。

有关背景信息，请参阅 [上下文腐烂](https://research.trychroma.com/context-rot) 上的 Chroma 文章。

子智能体工作流程有助于将嘈杂的工作移出主线程：

- 让 **主智能体** 专注于需求、决策和最终输出。
- 并行运行专门的 **分智能体** 以进行探索、测试或日志分析。
- 从子智能体返回 **摘要**，而不是原始中间输出。

当工作可以独立并行运行时，它们还可以节省时间，并且通过将较大的任务分解为有界的部分，使它们变得更容易处理。例如，Codex 可以将数百万令牌文档的分析拆分为更小的问题，并将提炼的结果返回到主线程。

作为起点，使用并行智能体执行大量读取任务，例如探索、测试、分类和总结。对于并行写入密集型工作流程要更加小心，因为智能体一次编辑代码可能会产生冲突并增加协调开销。

<a id="core-terms"></a>

## 核心术语

Codex 在子智能体工作流程中使用一些相关术语：

- **子智能体工作流程**：Codex 运行并行智能体并组合其结果的工作流程。
- **子智能体**：Codex启动处理特定任务的委托智能体。
- **智能体线程**：子智能体执行其工作的线程。支持的客户端允许您打开这些线程来检查进度或结果。

<a id="triggering-subagent-workflows"></a>

## 触发子智能体工作流程

<ContentModeSwitch group="codex-surface" id="web">

在大多数推理级别，直接要求子智能体或并行智能体工作。 Ultra 支持主动委派，因此 ChatGPT 可以委派合适的独立工作，而无需单独请求。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

直接要求子智能体或并行智能体工作。当适用的项目或技能指令有要求时，Codex 也可以进行委派。

</ContentModeSwitch>

在实践中，手动触发意味着使用直接指令，例如“生成两个智能体”、“并行委托这项工作”或“每个点使用一个智能体”。子智能体工作流比同类单智能体运行消耗更多的令牌，因为每个子智能体执行自己的模型和工具工作。

一个好的子智能体提示应该解释如何划分工作，Codex 是否应该在继续之前等待所有智能体，以及返回什么摘要或输出。

```text
使用并行子智能体检查此分支。生成一个用于安全风险的子智能体，一个用于测试差距的子智能体，一个用于可维护性的子智能体。等待这三个结果，然后通过文件参考按类别总结结果。
```

<a id="choosing-models-and-reasoning"></a>

## 选择模型和推理

不同的智能体需要不同的模型和推理设置。

<ContentModeSwitch group="codex-surface" id="web">

在ChatGPT Work中，从输入框中选择模型和智能级别。可用的智能级别可包括 **光**、**中等**、**高**、**超高** 和 **最大**，具体取决于所选模型。 **超** 仅适用于符合条件的帐户和支持的模型。它使用最大推理，让 ChatGPT 主动将合适的工作委派给子智能体。

在其他推理级别，当您希望并行委派工作时，明确要求子智能体。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

如果您未配置子智能体模型或 `model_reasoning_effort`，则子智能体将继承父智能体的模型和推理工作。如果显式生成请求或 `[agents]` 默认选择没有显式或配置推理工作的模型，则子智能体将使用该模型的默认推理工作。要平衡每个任务的智能、速度和价格，请在提示中请求特定模型或推理工作，在 `config.toml` 中配置 `[agents]` 默认值，或直接在自定义智能体文件中设置 `model` 和 `model_reasoning_effort`。例如，使用 `gpt-5.6-terra` 进行快速扫描，或使用更高效的 `gpt-5.6` 配置进行更苛刻的推理。

对于 Codex 中的大多数任务，从 `gpt-5.6` 开始。当您需要更快、更低成本的选项来减轻子智能体工作量时，请使用 `gpt-5.6-terra`。

<a id="model-choice"></a>

### 模型选择

- **`gpt-5.6`**：对于要求严格的智能体商来说，从这里开始。它最适合需要在更大的背景下进行规划、工具使用、验证和后续工作的模糊、多步骤工作。
- **`gpt-5.6-terra`**：用于注重速度和效率而不是深度的智能体，例如探索、大量读取扫描、大文件审查或处理支持文档。它非常适合将蒸馏结果返回给主智能体的并行工作人员。
- **`gpt-5.6-luna`**：用于快速、范围狭窄的智能体处理清晰、可重复或大批量的工作。

<a id="reasoning-effort-model_reasoning_effort"></a>

### 推理努力 (`model_reasoning_effort`)

- **`ultra`**：当所选模型支持时，用于最深入的推理。
- **`max`**和**`xhigh`**：当所选模型支持这些级别时，用于特别苛刻的推理。
- **`high`**：当智能体需要跟踪复杂逻辑、检查假设或处理边缘情况（例如，审阅者或专注于安全的智能体）时使用。
- **`medium`**：大多数智能体的平衡默认值。
- **`low`**：当任务简单并且速度最重要时使用。

更高的推理工作会增加响应时间和令牌使用，但它可以提高复杂工作的质量。详细信息请参见[模型](../models.zh-CN.md)、[配置基础知识](../config-file/config-basic.zh-CN.md)和[配置参考](../config-file/config-reference.zh-CN.md)。

</ContentModeSwitch>

<a id="orchestration-and-thread-controls"></a>

## 编排和线程控制

ChatGPT 或 Codex 处理跨智能体的编排，包括生成新的子智能体、路由后续指令、等待结果和关闭智能体线程。

当许多智能体正在运行时，Codex 会等待，直到所有请求的结果都可用，然后返回合并的响应。

<ContentModeSwitch group="codex-surface" id="web">

在大多数推理级别，ChatGPT 在直接请求后生成智能体。借助 Ultra，ChatGPT 还可以在并行工作有用时主动进行委派。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

当前本地 Codex 在直接请求或适用的项目或技能指令后释放生成智能体。

</ContentModeSwitch>

要查看它的实际效果，请在您的项目中尝试以下提示：

```text
我想回顾一下当前 PR 的以下几点（此分支与主分支）。每个点生成一个智能体，等待所有智能体，并总结每个点的结果。
1、安全问题
2. 代码质量
3. Bugs
4. Race
5. 测试片状
6. 代码的可维护性
```

<a id="managing-subagents"></a>

## 管理子智能体

<ContentModeSwitch group="codex-surface" id="web">

打开 **子智能体** 可查看只读 **活跃** 和 **完成** 列表。选择一个已完成的子智能体以检查其详细信息和结果。网络侧边栏报告子智能体活动；它不提供控制来停止或引导单个子智能体。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

- 从主线程中显示的活动中打开子智能体线程以检查其工作。
- 直接要求 Codex 引导正在运行的子智能体、停止它或关闭已完成的子智能体线程。



> 插图：Codex 桌面聊天显示两个子智能体并行工作。





> 插图：Codex 桌面子智能体面板，没有活动子智能体且已完成三个审核。



</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

- 在 CLI 中使用 `/agent` 在活动智能体线程之间切换并检查正在进行的线程。
- 直接要求 Codex 引导正在运行的子智能体、停止它或关闭已完成的智能体线程。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

- 当后台智能体面板可用时，将其展开以检查状态、停止活动子智能体或打开子智能体线程。
- 直接要求 Codex 引导正在运行的子智能体、停止它或关闭已完成的子智能体线程。

</ContentModeSwitch>

<a id="approvals-and-sandbox-controls"></a>

## 批准和沙箱控制

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

子智能体继承您当前的沙箱策略。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT Work 在其托管环境中运行子智能体，并且不公开本地 Codex 沙箱或批准模式控件。子智能体使用父聊天可用的工具。网站和连接器权限仍然特定于工具。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

子智能体继承在输入框下选择的权限模式。在要求 Codex 委派工作之前，请选择父轮的权限模式。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在交互式 CLI 会话中，即使您正在查看主线程，审批请求也可能会从不活动的智能体线程中出现。批准覆盖显示源线程标签，您可以在批准、拒绝或应答请求之前按 `o` 打开该线程。

在非交互式流程中，或者每当运行无法显示新的批准时，需要新批准的操作就会失败，并且 Codex 会将错误显示回父工作流程。

Codex 在生成子级时也会重新应用父级回合的实时运行时覆盖。这包括您在会话期间以交互方式设置的沙箱和批准选项，例如 `/permissions` 更改或 `--yolo`，即使所选的自定义智能体文件设置不同的默认值也是如此。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

子智能体继承在输入框下选择的权限模式。在要求 Codex 委派工作之前，请选择父轮的权限模式。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

您还可以覆盖单个 [定制智能体](#custom-agents) 的沙箱配置，例如明确将其标记为在只读模式下工作。

<a id="custom-agents"></a>

## 定制智能体

Codex 附带内置智能体：

- `default`：通用后备智能体。
- `worker`：以执行为中心的智能体，用于实施和修复。
- `explorer`：大量读取的代码库探索智能体。

要定义您自己的自定义智能体，请在 `~/.codex/agents/`（针对个人智能体）或 `.codex/agents/`（针对项目范围的智能体）下添加独立 TOML 文件。

每个文件定义一个自定义智能体。 Codex 加载这些文件作为生成会话的配置层，因此自定义智能体可以覆盖与普通 Codex 会话配置相同的设置。这可能比专门的智能体清单更重，并且随着创作和共享的成熟，这种格式可能会发展。

每个独立的自定义智能体文件必须定义：

- `name`
- `description`
- `developer_instructions`

如果自定义智能体文件设置 `model` 或 `model_reasoning_effort`，则文件中的值优先。在应用文件之前，Codex 从显式生成值解析每个设置，然后是相应的 `[agents]` 默认值，然后是父值。如果显式生成请求或 `[agents]` 默认选择模型并且两者均不提供推理工作，则 Codex 将使用该模型的默认工作。仅设置 `model` 的自定义智能体文件保留了先前解决的工作。如果所选模型不支持该工作或者您想要不同的模型，也请在文件中设置 `model_reasoning_effort`。当自定义智能体文件省略其他会话设置（例如 `sandbox_mode`、`mcp_servers` 和 `skills.config`）时，它们将从父级继承。

<a id="global-settings"></a>

### 全局设置

全局子智能体设置仍然位于 [配置](../config-file/config-basic.zh-CN.md#configuration-precedence) 中的 `[agents]` 下。

| 字段 | 类型 | 必需 | 用途 |
| ------------------------------------------- | ------- | :------: | ------------------------------------------------------------------- |
| `agents.enabled` | 布尔值 | 否 | 启用或禁用多智能体工具。                                |
| `agents.max_concurrent_threads_per_session` | 数量 | 否 | 并发打开生成智能体线程的上限，不包括主线程。 |
| `agents.default_subagent_model` | 字符串 | 否 | 设置生成智能体的默认模型。                           |
| `agents.default_subagent_reasoning_effort` | 字符串 | 否 | 设置生成智能体的默认推理工作。                |
| `agents.interrupt_message` | 布尔值 | 否 | 当智能体轮次中断时记录模型可见消息。   |

**注意事项：**

- `agents.enabled` 默认为 `true`。将其设置为 `false` 以禁用多智能体工具。
- 当您未设置 `agents.max_concurrent_threads_per_session` 时，Codex 将选择默认值。现有配置可以继续使用 `agents.max_threads` 作为旧别名。
- 显式生成值覆盖 `agents.default_subagent_model` 和 `agents.default_subagent_reasoning_effort`。
- `agents.interrupt_message` 默认为 `true`。将其设置为 `false` 以从智能体上下文中忽略模型可见的中断消息。
- 如果自定义智能体名称与内置智能体（例如 `explorer`）匹配，则您的自定义智能体优先。

<a id="custom-agent-file-schema"></a>

### 自定义智能体文件架构

| 字段 | 类型 | 必需 | 用途 |
| ------------------------ | ------ | :------: | --------------------------------------------------------------- |
| `name` | 字符串 | 是 | 智能体名称 Codex 在生成或引用此智能体时使用。 |
| `description` | 字符串 | 是 | Codex 何时应使用此智能体的面向人的指导。     |
| `developer_instructions` | 字符串 | 是 | 定义智能体行为的核心指令。             |

您还可以在自定义智能体文件中包含其他受支持的 `config.toml` 密钥，例如 `model`、`model_reasoning_effort`、`sandbox_mode`、`mcp_servers` 和 `skills.config`。

Codex 通过其 `name` 字段标识自定义智能体。将文件名与智能体名称相匹配是最简单的约定，但 `name` 字段才是事实来源。

<a id="example-custom-agents"></a>

### 自定义智能体示例

最好的海关智能体都是狭隘的、固执己见的。为每一项工作提供明确的工作、与该工作相匹配的工具使用界面以及防止其漂移到相邻工作的说明。

<a id="example-1-pr-review"></a>

#### 示例1：公关审核

此模式将审核分为三个重点自定义智能体：

- `pr_explorer` 映射代码库并收集证据。
- `reviewer` 寻找正确性、安全性和测试风险。
- `docs_researcher` 通过专用的 MCP 服务器检查框架或 API 文档。

项目配置（`.codex/config.toml`）：

```toml
[agents]
max_concurrent_threads_per_session = 8
```

`.codex/agents/pr-explorer.toml`：

```toml
name = "pr_explorer"
description = "Read-only codebase explorer for gathering evidence before changes are proposed."
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Stay in exploration mode.
Trace the real execution path, cite files and symbols, and avoid proposing fixes unless the parent agent asks for them.
Prefer fast search and targeted file reads over broad scans.
"""
```

`.codex/agents/reviewer.toml`：

```toml
name = "reviewer"
description = "PR reviewer focused on correctness, security, and missing tests."
model = "gpt-5.6-terra"
model_reasoning_effort = "high"
sandbox_mode = "read-only"
developer_instructions = """
Review code like an owner.
Prioritize correctness, security, behavior regressions, and missing test coverage.
Lead with concrete findings, include reproduction steps when possible, and avoid style-only comments unless they hide a real bug.
"""
```

`.codex/agents/docs-researcher.toml`：

```toml
name = "docs_researcher"
description = "Documentation specialist that uses the docs MCP server to verify APIs and framework behavior."
model = "gpt-5.6-luna"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Use the docs MCP server to confirm APIs, options, and version-specific behavior.
Return concise answers with links or exact references when available.
Do not make code changes.
"""

[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
```

此设置适用于以下提示：

```text
对照主分支检查此分支。让 pr_explorer 映射受影响的代码路径，审阅者发现真正的风险，并让 docs_researcher 验证补丁所依赖的框架 API。
```

<a id="example-2-frontend-integration-debugging"></a>

#### 示例2：前端集成调试

此模式对于 UI 回归、不稳定的浏览器流程或跨应用程序代码和正在运行的产品的集成错误非常有用。

项目配置（`.codex/config.toml`）：

```toml
[agents]
max_concurrent_threads_per_session = 6
```

`.codex/agents/code-mapper.toml`：

```toml
name = "code_mapper"
description = "Read-only codebase explorer for locating the relevant frontend and backend code paths."
model = "gpt-5.6-luna"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Map the code that owns the failing UI flow.
Identify entry points, state transitions, and likely files before the worker starts editing.
"""
```

`.codex/agents/browser-debugger.toml`：

```toml
name = "browser_debugger"
description = "UI debugger that uses browser tooling to reproduce issues and capture evidence."
model = "gpt-5.6-terra"
model_reasoning_effort = "high"
sandbox_mode = "workspace-write"
developer_instructions = """
Reproduce the issue in the browser, capture exact steps, and report what the UI actually does.
Use browser tooling for screenshots, console output, and network evidence.
Do not edit application code.
"""

[mcp_servers.chrome_devtools]
url = "http://localhost:3000/mcp"
startup_timeout_sec = 20
```

`.codex/agents/ui-fixer.toml`：

```toml
name = "ui_fixer"
description = "Implementation-focused agent for small, targeted fixes after the issue is understood."
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
developer_instructions = """
Own the fix once the issue is reproduced.
Make the smallest defensible change, keep unrelated files untouched, and validate only the behavior you changed.
"""

[[skills.config]]
path = "/Users/me/.agents/skills/docs-editor/SKILL.md"
enabled = false
```

此设置适用于以下提示：

```text
调查设置模式无法保存的原因。让 browser_debugger 重现它，让 code_mapper 跟踪负责的代码路径，让 ui_fixer 在故障模式明确后实施最小的修复。
```

</ContentModeSwitch>