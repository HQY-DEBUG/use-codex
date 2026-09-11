> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/sdk.md)。

<a id="codex-security-typescript-sdk"></a>

# Security TypeScript SDK

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Codex Security TypeScript SDK 对应用程序或开发人员工具中的仓库和代码更改运行安全扫描。 SDK 返回键入的结果、覆盖范围详细信息以及扫描工件的路径。对于较长的扫描，它支持飞行前检查、成本限制、进度回调和取消。

SDK 使用 ECMAScript 模块 (ESM) 并使用 Node.js 22（22.13.0 或更高版本）、24 或 26 在服务器端运行。扫描还需要 Python 3.10 或更高版本。 Python 3.10 还需要 `tomli` 包。

Codex Security SDK 是 [在 GitHub 上公开可用](https://github.com/openai/codex-security)。运行扫描需要 Codex Security 访问权限。对于通用编码智能体，请参见 [Codex SDK使用指南](../codex-sdk.zh-CN.md)。有关终端和 CI 工作流程，请参阅 [Codex Security CLI 快速入门](cli.zh-CN.md)。

<a id="set-up-the-sdk"></a>

## 设置SDK

安装SDK：

```bash
npm install @openai/codex-security
```

开始扫描之前，请设置 `OPENAI_API_KEY` 或 `CODEX_API_KEY`，使用现有的文件支持的 Codex 登录或 [配置另一个提供者](#configure-the-runtime-and-credentials)。 Amazon Bedrock 使用 AWS 凭证； OpenRouter 和 Fireworks 使用特定于提供商的 API 密钥和配置。

为获得最佳结果，请使用经过 [网络可信访问](https://chatgpt.com/cyber) 验证的帐户。登录或提供 API 密钥并不授予可信访问权限。

<a id="run-a-scan"></a>

## 运行扫描

仅扫描您信任并有权评估的仓库。 SDK 使用您的本地操作系统权限运行，并且永远不会暂停以等待批准。扫描进程可以继承您的环境，因此在开始之前删除不相关的凭据。参见 [本地扫描权限](cli/reference.zh-CN.md#local-scan-permissions)。

创建一个 `CodexSecurity` 客户端，运行标准仓库扫描，并在工作完成后关闭客户端。传递 `outputDir` 以选择封闭的 Git 工作树之外的私有结果目录。

如果省略 `outputDir`，Codex Security 会将结果保存在其自己的持久状态目录中。结果可以包括源代码摘录和漏洞详细信息，因此请选择适当的权限和保留策略。

```ts


const security = new CodexSecurity();

try {
  const result = await security.run("/path/to/repository", {
    outputDir: "/path/outside/repository/results",
  });

  console.log(result.reportPath);
  console.log(result.coverage.completeness);
  console.log(result.findings.findings.length);
} finally {
  await security.close();
}
```

`run` 开始扫描，等待完成，验证密封的工件，并返回 `ScanResult`。 `close`释放隔离运行时，支持重复调用。

<a id="check-inputs-with-preflight"></a>

## 通过预检检查输入

在开始扫描之前，使用 `preflight` 检查仓库、目标、模式、知识库文档、输出位置和 Codex 配置：

```ts
const plan = await security.preflight("/path/to/repository", {
  target: ["services/billing", "packages/auth"],
  knowledgeBasePaths: ["/path/to/architecture.md"],
  outputDir: "/path/outside/repository/results",
});

console.log(plan.repository);
console.log(plan.target.kind);
console.log(plan.mode);
console.log(plan.outputDir);
```

预检使 Codex 运行时和凭证保持不变。它还将插件和 Python 发现留给扫描本身。这使得预检对于在长时间运行或凭证操作之前检查用户输入非常有用。

要预览现有结果目录的存档，请设置 `archiveExisting: true`：

```ts
const plan = await security.preflight("/path/to/repository", {
  outputDir: "/path/outside/repository/results",
  archiveExisting: true,
});

console.log(plan.archiveDir);
```

返回的`archiveDir`预览存档命名。最终路径可能有所不同，因为 `run` 生成其自己唯一的目的地。使用`onOutputArchived`捕获实际存档路径：

```ts
await security.run("/path/to/repository", {
  outputDir: "/path/outside/repository/results",
  archiveExisting: true,
  onOutputArchived(archiveDir) {
    console.log("Archived results:", archiveDir);
  },
});
```

扫描会存档早期结果并从空输出目录开始。

<a id="choose-a-scan-target"></a>

## 选择扫描目标

SDK 支持仓库、路径、提交差异和工作树目标。默认目标是完整的仓库。

<a id="scan-selected-paths"></a>

### 扫描选定的路径

传递仓库内的路径数组：

```ts
const result = await security.run("/path/to/repository", {
  target: ["services/billing", "packages/auth"],
});
```

路径可以标识文件或目录。 SDK 解析仓库内的每个路径并删除重复项。

<a id="scan-committed-changes"></a>

### 扫描提交的更改

使用 `DiffTarget.refs` 扫描两个本地可用的 Git 修订版之间提交的更改：

```ts


const target = DiffTarget.refs({
  base: "origin/main",
  head: "HEAD",
});

const result = await security.run("/path/to/repository", { target });
```

磁头默认为`HEAD`。 Diff 目标要求仓库参数是 Git 工作树根。

<a id="scan-the-working-tree"></a>

### 扫描工作树

使用 `DiffTarget.workingTree` 根据基本修订扫描已暂存和未暂存的更改：

```ts
const target = DiffTarget.workingTree({ base: "HEAD" });
const result = await security.run("/path/to/repository", { target });
```

底座默认为 `HEAD`。在开始差异或工作树扫描之前获取选定的修订版本。

<a id="select-deep-mode"></a>

### 选择深度模式

为需要更广泛审查的仓库或路径扫描设置 `mode: "deep"`：

```ts
const result = await security.run("/path/to/repository", {
  target: ["services/billing"],
  mode: "deep",
  workers: 2,
  subagents: 0,
  stopAfterNoNew: 3,
  maxDiscoveryRuns: 10,
  maxTimeHours: 1.5,
});
```

深度模式支持仓库和路径目标。使用标准模式进行差异和工作树扫描。可选设置控制并发独立标准扫描工作人员、每个工作人员的子智能体、连续完成的工作人员扫描（没有新发现）以及工作人员运行的总数和持续时间。他们需要 `mode: "deep"`。

`maxTimeHours` 默认为 `96`，并接受最大 `96` 的正数，包括小数小时。在截止日期前，Codex Security 停止未完成的工作人员，保留已完成的扫描结果，并将其汇总到最终报告中。在将限时扫描视为完全覆盖的证据之前，请先查看 `result.coverage.completeness`。

<a id="add-a-security-knowledge-base"></a>

### 添加安全知识库

通过`knowledgeBasePaths`传递架构文档、威胁模型或安全策略：

```ts
const result = await security.run("/path/to/repository", {
  knowledgeBasePaths: [
    "/path/to/architecture.md",
    "/path/to/security-policies",
  ],
});
```

SDK 接受文件或目录并递归搜索目录。支持的文档格式为 `.md`、`.markdown`、`.txt`、`.pdf` 和 `.docx`。 SDK 拒绝链接的输入路径，跳过链接的目录条目，并将提取的文档内容保留在保存的扫描结果之外。

<a id="add-scan-and-follow-up-instructions"></a>

### 添加扫描和后续说明

使用 `scanPrompt` 聚焦扫描，使用 `postScanPrompt` 请求后续：

```ts
const result = await security.run("/path/to/repository", {
  scanPrompt: "Focus on tenant isolation and authorization checks.",
  postScanPrompt: "Write confirmed findings to post-scan-summary.md.",
});
```

如果后续失败，SDK会保留已完成的扫描，并通过`onWarning`上报错误。它会恢复后续更改的任何已完成的扫描工件。

<a id="set-a-scan-budget"></a>

### 设置扫描预算

设置 `maxCostUsd` 在估计模型成本超过限制时停止扫描。使用 `onCost` 跟踪扫描运行时的成本：

```ts
const result = await security.run("/path/to/repository", {
  maxCostUsd: 5,
  onCost(cost) {
    console.log(cost.estimatedUsd);
  },
});

console.log(result.cost?.estimatedUsd);
```

该限制估计了支出，但不是硬性上限，因此已经进行中的请求可能会略高于该限制。 Codex Security汇总完成的worker结果后，如果深度扫描达到限制，`run`返回`coverage.completeness`设置为`"partial"`的结果，并通过`onWarning`上报预算警告。

如果扫描无法产生完整的部分结果，`run` 会抛出 `ScanCostLimitExceededError` 并保留任何可用的输出。

<a id="work-with-scan-results"></a>

## 处理扫描结果

`ScanResult` 公开结构化文档、扫描元数据和工件路径：

| 属性 | 内容 |
| -------------------- | ---------------------------------------------------------------------------------- |
| `manifest` | 密封的扫描清单，包括目标、范围、生产者和工件记录。 |
| `findings` | 当前扫描的结果。阅读 `findings.findings` 中的查找对象。     |
| `repositoryFindings` | 当扫描历史记录可用时，打开仓库扫描的结果。             |
| `coverage` | 审查了使用界面、排除、推迟的工作、未解决的问题和完整性。    |
| `scanDir` | 扫描目录。                                                                |
| `threadId` | 扫描的 Codex 线程标识符。                                          |
| `turnResult` | 对话轮次状态、响应和可用使用元数据。                               |
| `cost` | 估计模型和Token成本，或 `null`（不可用时）。                        |
| `reportPath` | `report.md` 的路径。                                                           |
| `manifestPath` | `scan-manifest.json` 的路径。                                                  |
| `findingsPath` | `findings.json` 的路径。                                                       |
| `coveragePath` | `coverage.json` 的路径。                                                       |
| `artifactsDir` | 支持工件目录。                                                |
| `sarifPath` | 生成的 SARIF 路径，或者当 SARIF 不存在时为 `null`。                          |
| `pluginVersion` | 扫描制作者录制的版本。                                         |

要在以后的扫描中需要相同的插件，请传递 `expectedPluginVersion: result.pluginVersion`。如果安装的插件版本不同，SDK 将拒绝扫描。

直接使用结构化发现和覆盖范围：

```ts
for (const finding of result.findings.findings) {
  const location = finding.locations[0];
  if (location === undefined) continue;

  console.log(
    finding.severity.level,
    `${location.path}:${location.startLine}`,
    finding.title
  );
}

for (const deferred of result.coverage.deferred) {
  console.log(deferred.id, deferred.reason);
}
```

结果可以包括可选的 `codeEvidence`、`rootCause`、`validation`、`attackPath`、`remediationTests` 和 `preventiveControls` 字段。

对于仓库范围内的发现，`confirmedInLatestScan` 将最新扫描中看到的发现与仍处于开放状态的早期发现区分开来：

```ts
for (const finding of result.repositoryFindings ?? []) {
  console.log(finding.title, finding.confirmedInLatestScan);
}
```

覆盖范围完整性为 `complete`、`partial` 或 `unknown`。在使用扫描作为安全决策的证据之前，请查看延迟的使用界面、排除和未解决的问题。

`result.toJSON()` 在一个 JSON 就绪对象中返回清单、仓库和当前扫描结果、覆盖范围、扫描和线程标识符、`reportPath`、`artifactsDir`、`sarifPath`、成本和转向元数据。

<a id="track-or-cancel-a-scan"></a>

## 跟踪或取消扫描

传递 `ScanOptions` 回调来报告扫描启动、工作进程和连接重试：

```ts
const result = await security.run("/path/to/repository", {
  outputDir: "/path/outside/repository/results",
  onScanStarted() {
    console.log("Scan started");
  },
  onProgress(progress) {
    console.log(progress.phase, progress.filesCompleted, progress.filesTotal);
  },
  onWorkerStatus(status) {
    console.log(status.kind, status);
  },
  onSessionEvent(session) {
    console.log(session.threadId, session.worker, session.event["type"]);
  },
  onReconnect(attempt, maxAttempts) {
    console.log(`Reconnect attempt ${attempt} of ${maxAttempts}`);
  },
  onObserverError(observer, error) {
    console.error(`${observer} failed`, error);
  },
});

console.log(result.reportPath);
```

当取消来自请求、作业控制器或超时时，传递 `AbortSignal`：

```ts


const controller = new AbortController();

try {
  const scan = security.run("/path/to/repository", {
    outputDir: "/path/outside/repository/results",
    signal: controller.signal,
  });

  controller.abort();
  await scan;
} catch (error) {
  if (error instanceof ScanInterruptedError) {
    console.error(error.scanDir);
  } else {
    throw error;
  }
}
```

中断的扫描可能会在 `scanDir` 中留下部分输出。当结果需要调查时保留该目录。

显示扫描设置进度的应用程序还可以使用 `ScanOptions` 生命周期回调：

| 回调 | 当 | 时调用
| ----------------------------------- | ---------------------------------------------------- |
| `onAuthentication(authentication)` | 扫描选择其身份验证方法。          |
| `onOutputArchived(archiveDir)` | 现有结果移至存档目录。      |
| `onOutputDirReady(scanDir)` | 私有扫描目录已准备就绪。                 |
| `onScanStarted()` | 扫描设置完成并开始执行。           |
| `onTrustedAccessStatus(status)` | 可信访问状态变为可用。             |
| `onReconnect(attempt, maxAttempts)` | SDK 重试断开连接的扫描流。          |
| `onActivity(activity)` | 命令、工具、推理步骤或消息更新。 |
| `onProgress(progress)` | 扫描阶段或已查看的文件计数发生变化。       |
| `onWorkerStatus(status)` | 工作人员预检或调度状态更改。         |
| `onSessionEvent(session)` | 扫描或工作会话发出事件。             |
| `onCost(cost)` | 提供更新的估计扫描成本。         |
| `onWarning(warning)` | 扫描报告警告。                          |
| `onObserverError(observer, error)` | 另一个扫描生命周期回调引发错误。     |

可信访问状态为 `granted`、`not_granted` 或 `unknown`。丢失或未知的访问也会触发 `onWarning`。

`onSessionEvent` 接收未经编辑且可能包含源代码或凭据的事件。在将它们发送到共享日志或其他服务之前对其进行过滤。

<a id="configure-the-runtime-and-credentials"></a>

## 配置运行时和凭据

当您需要特定插件、解释器或 Codex 设置时传递运行时配置：

```ts
const security = new CodexSecurity({
  pluginPath: "/path/to/codex-security-plugin",
  pythonPath: "/path/to/python",
  codexOverrides: {
    model: "gpt-5.6-terra",
    model_reasoning_effort: "high",
  },
});
```

`pluginPath` 接受插件目录或 ZIP。 `pythonPath` 选择插件解释器。 `codexOverrides` 将支持的值合并到独立的 Codex 配置中。默认情况下，扫描使用 `gpt-5.6-sol` 进行超高推理工作。在 `codexOverrides` 中设置 `model` 和 `model_reasoning_effort` 以使用不同的模型或推理工作。要使用[亚马逊基岩](cli/reference.zh-CN.md#use-amazon-bedrock)，请在`codexOverrides`中设置`model_provider`和`model`。

`codexOverrides` 无法限制扫描的文件系统访问或更改其批准策略。参见 [本地扫描权限](cli/reference.zh-CN.md#local-scan-permissions)。

对于 OpenRouter 或 Fireworks，还需在 `codexOverrides` 中提供匹配的 API 密钥和完整的提供程序配置。例如，设置`OPENROUTER_API_KEY`并配置OpenRouter：

```ts
const security = new CodexSecurity({
  codexOverrides: {
    model: "anthropic/claude-sonnet-4.5",
    model_provider: "openrouter",
    model_providers: {
      openrouter: {
        name: "OpenRouter",
        base_url: "https://openrouter.ai/api/v1",
        env_key: "OPENROUTER_API_KEY",
        wire_api: "responses",
      },
    },
  },
});
```

对于 Fireworks，将两个 `openrouter` 键更改为 `fireworks`，将 `name` 设置为 `Fireworks AI`，将 `env_key` 设置为 `FIREWORKS_API_KEY`，使用 `https://api.fireworks.ai/inference/v1` 作为 `base_url`，然后选择 Fireworks 模型。

客户端还公开支持的身份验证方法：

| 方法 | 目的 |
| -------------------------- | ----------------------------------------------------------- |
| `loginApiKey(apiKey)` | 使用 API 密钥对隔离运行时进行身份验证。          |
| `loginChatGPT()` | 启动浏览器登录流程并返回登录句柄。     |
| `loginChatGPTDeviceCode()` | 启动设备代码登录流程并返回登录句柄。 |
| `account()` | 返回当前认证状态。                    |
| `logout()` | 清除隔离身份验证。                              |

登录句柄提供 `waitForInstructions`、`authUrl`、`verificationUrl`、`userCode`、`wait` 和 `cancel`，以便应用程序可以呈现并完成所选的登录流程。 SDK 可以重复使用文件支持的 Codex 登录。 API 密钥非常适合 CI 和服务器端自动化。

当 API 密钥和存储的登录都可用时，SDK 默认使用 API 密钥。要改用 ChatGPT 登录，请选择它进行扫描：

```ts
const result = await security.run("/path/to/repository", {
  auth: "chatgpt",
});
```

将 `auth: "api-key"` 设置为需要环境 API 密钥。 `preflight` 接受相同的 `auth` 选项。

<a id="handle-scan-errors"></a>

## 处理扫描错误

捕获与您的应用程序可以执行的操作相匹配的导出错误类：

| 错误 | 含义 |
| -------------------------------- | ------------------------------------------------------------------ |
| `AuthenticationRequiredError` | 扫描需要受支持的凭据。                               |
| `ConfigurationError` | Codex 配置或覆盖不合适。                  |
| `InvalidTargetError` | 仓库、路径、模式或 Git 目标不合适。           |
| `OutputDirectoryError` | 输出位置或其权限不合适。             |
| `OutputInsideProtectedRootError` | 输出目录位于扫描的仓库或工作树内。 |
| `PluginPythonUnavailableError` | 可用的Python解释器不可用。                        |
| `PluginBootstrapError` | 插件运行时无法启动。                                |
| `ScanCostLimitExceededError` | 扫描超出了其估计成本限制。                        |
| `IncompleteScanError` | 扫描在产生所需结果之前结束。               |
| `ContractValidationError` | 已完成的扫描返回结构化合同错误。             |
| `ScanInterruptedError` | 中断停止了扫描，可能留下了部分输出。 |

继续使用 [CLI 快速入门](cli.zh-CN.md)、[CI指南](cli/ci.zh-CN.md) 或 [CLI 参考](cli/reference.zh-CN.md)。