> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/cli/faq.md)。

<a id="codex-security-cli-faq"></a>

# Security CLI 常见问题

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

查找有关从终端扫描仓库和管理安全结果的常见问题的答案。对于安装和首次扫描，请从 [CLI 快速入门](../cli.zh-CN.md) 开始。

<a id="repository-scans"></a>

## 仓库扫描

<a id="who-can-use-the-cli"></a>

### 谁可以使用 CLI

`@openai/codex-security` 包是公开的。

运行扫描需要 Codex Security 访问权限。为获得最佳结果，请使用经过 [网络可信访问](https://chatgpt.com/cyber) 验证的帐户。

<a id="why-does-a-scan-use-an-api-key-after-sign-in"></a>

### 为什么登录后扫描使用 API 密钥

当您的环境包括 `OPENAI_API_KEY` 或 `CODEX_API_KEY` 时，无需交互式终端的扫描以及 JSON 和 JSONL 扫描默认使用环境 API 密钥，即使在成功 ChatGPT 或访问令牌登录后也是如此。带有文本输出的交互式扫描会要求您选择何时也可以使用 ChatGPT 登录。试运行不会提示或加载凭据。

要使用您存储的凭据进行扫描，请明确选择它们：

```bash
npx @openai/codex-security scan . --auth chatgpt
```

需要 `OPENAI_API_KEY` 或 `CODEX_API_KEY` 的 API 密钥：

```bash
npx @openai/codex-security scan . --auth api-key
```

要将存储的凭据设置为自动默认凭据，请运行 `unset OPENAI_API_KEY CODEX_API_KEY`。有关所有支持的身份验证模式，请参阅 [CLI 参考](reference.zh-CN.md#select-scan-authentication)。

<a id="how-does-bulk-repository-scanning-work"></a>

### 批量仓库扫描如何工作

使用 GitHub CLI 登录：

```bash
gh auth login
```

从 GitHub 帐户或组织中发现并选择仓库：

```bash
npx @openai/codex-security bulk-scan
```

对于准备好的列表，请提供仓库 CSV 和输出目录：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4
```

请参阅 [运行批量安全扫描](bulk-scans.zh-CN.md) 了解 GitHub 发现、CSV 格式、活动结果和可用选项。

<a id="can-an-interrupted-bulk-scan-resume"></a>

### 中断的批量扫描可以恢复吗

是的。对原始 CSV 和输出目录运行相同的批量扫描命令。 Codex Security 跳过已完成的仓库。

添加 `--max-attempts 3` 以重试临时仓库或扫描错误：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4 \
  --max-attempts 3
```

覆盖 `partial` 或 `unknown` 的完整扫描将保留其结果，并导致活动以代码 `2` 退出。即使使用 `--max-attempts`，也不会重试。

<a id="how-can-a-scan-use-architecture-and-security-policies"></a>

### 扫描如何使用架构和安全策略

使用 `--knowledge-base` 传递架构文档、威胁模型或安全策略：

```bash
npx @openai/codex-security scan . \
  --knowledge-base /path/to/architecture.md \
  --knowledge-base /path/to/security-policies
```

Codex Security 使用这些文档作为当前扫描的上下文。有关支持的文件类型和目录行为，请参阅 [添加安全上下文](reference.zh-CN.md#add-security-context)。

<a id="findings-and-coverage"></a>

## 调查结果和覆盖范围

<a id="where-can-teams-find-earlier-scan-results"></a>

### 团队在哪里可以找到早期的扫描结果

列出仓库的已保存扫描：

```bash
npx @openai/codex-security scans list /path/to/repository
```

使用结果中的扫描 ID 来检查其结果：

```bash
npx @openai/codex-security scans show SCAN_ID
```

每次完成的扫描都会将其报告、结果、覆盖范围和支持工件保存在一起。完整布局请参见 [扫描伪影](reference.zh-CN.md#scan-artifacts)。

要检查保存的扫描和工作器事件，请运行 `scans logs SCAN_ID`。这些日志未经编辑，可以包含源代码或凭据。

<a id="what-if-the-cli-cant-save-scan-history"></a>

### 如果 CLI 无法保存扫描历史记录怎么办

Codex Security 将扫描历史记录保存在工作台数据库中。如果默认状态目录不可写，请选择仓库外部的私有目录：

```bash
export CODEX_SECURITY_STATE_DIR=/path/outside/repository/codex-security-state
```

<a id="how-do-scans-distinguish-new-and-known-findings"></a>

### 扫描如何区分新发现和已知发现

列出仓库所有扫描的开放结果：

```bash
npx @openai/codex-security findings list /path/to/repository
```

该列表列出了最新扫描中确认的发现结果以及扫描未确认的早期开放发现结果。

比较两次扫描的结果：

```bash
npx @openai/codex-security scans compare PREVIOUS_SCAN_ID CURRENT_SCAN_ID
```

比较会自动按根本原因匹配结果，重用已保存的匹配项，并识别新的、持续存在的、重新打开的、已解决的和未知的结果。仅当后续扫描覆盖其原始目标和受影响的路径且没有覆盖间隙时，结果才算已解决。

<a id="how-does-false-positive-feedback-work"></a>

### 假阳性反馈如何发挥作用

检查保存的扫描以查找事件 ID：

```bash
npx @openai/codex-security scans show SCAN_ID
```

记录为什么该发现不适用：

```bash
npx @openai/codex-security findings false-positive FINDING_OCCURRENCE_ID \
  --reason "The framework escapes this input before it reaches the query"
```

未来对同一仓库的扫描会收到该解释作为上下文。他们仍然独立检查当前源、控件和可达性。解除不会抑制规则、路径或漏洞类别。

有关命令的详细信息，请参见 [研究结果参考](reference.zh-CN.md#codex-security-findings)。

<a id="why-can-repeat-scans-return-different-findings"></a>

### 为什么重复扫描会返回不同的结果

即使扫描配置相同，人工智能辅助扫描也会有所不同。首先重新运行基线扫描：

```bash
npx @openai/codex-security scans rerun BASELINE_SCAN_ID
```

重新运行保留原始扫描配置并需要相同的插件版本。如果安装的插件已更改，该命令将停止。

将基线与新扫描进行比较：

```bash
npx @openai/codex-security scans compare BASELINE_SCAN_ID REPEAT_SCAN_ID
```

当缺少上下文可能导致变化时，提供共享架构和安全指导。匹配可以识别运行中相同的潜在发现，但它并不使扫描具有确定性。直接重新检查任何消失的重要发现。

<a id="how-can-a-team-confirm-that-a-fix-worked"></a>

### 团队如何确认修复是否有效

应用修复后，重新运行原始扫描：

```bash
npx @openai/codex-security scans rerun BEFORE_SCAN_ID
```

将原始结果与新扫描结果进行比较：

```bash
npx @openai/codex-security scans compare BEFORE_SCAN_ID AFTER_SCAN_ID
```

确认新扫描覆盖了原始目标和受影响的路径，没有覆盖间隙。然后直接根据当前检出目录重新检查原始发现：

```bash
npx @openai/codex-security validate /path/to/original/findings.json \
  "Recheck the SQL injection in src/orders.ts:42 against the current code"
```

仅缺少发现或扫描比较并不能证明修复有效。

<a id="what-does-incomplete-coverage-mean"></a>

### 覆盖不完全是什么意思

覆盖范围可以是 `complete`、`partial` 或 `unknown`。在将扫描视为审核证据之前，审核 `coverage.json` 中排除的路径、延迟的曲面和未解决的问题。

即使没有严重性策略，部分或未知覆盖范围的扫描也会返回退出代码 `2`。他们仍然保留任何可用的调查结果和报道。当稍后的扫描未覆盖该结果的原始路径时，无法确定较早的结果不再存在。

<a id="automation-and-cost"></a>

## 自动化和成本

<a id="how-do-deep-scan-time-limits-work"></a>

### 深度扫描时间限制如何运作

开始深度扫描时设置工作人员截止日期：

```bash
npx @openai/codex-security scan . --mode deep --max-time-hours 1.5
```

默认为 `96` 小时。使用 `96` 以内的任何正值，包括分数。在截止日期前，Codex Security 停止未完成的工作人员，保留已完成的标准扫描结果，并将其汇总到最终报告中。如果没有工作人员完成源代码审查，报告会记录部分覆盖率，并且 CLI 返回退出代码 `2`。

对于持久设置或批量营销活动，请在 [深度扫描配置](reference.zh-CN.md#configure-deep-scans) 中的 `[deep_scan]` 下设置 `max_time_hours`。

<a id="how-do-scan-cost-limits-work"></a>

### 扫描成本限制如何运作

在开始扫描之前设置以美元为单位的估计成本限制：

```bash
npx @openai/codex-security scan . --max-cost 5
```

该限制是一个估计，而不是硬性支出上限。已经进行中的请求可以在其之上完成。如果在 Codex Security 聚合已完成的工作器结果后深度扫描达到限制，CLI 将保存部分覆盖的已完成报告，并以代码 `2` 退出。否则，它会保留任何可用的部分输出。

<a id="can-scans-check-commits-and-pull-requests"></a>

### 可以扫描检查提交和拉取请求

为暂存和未暂存的更改安装预提交安全检查：

```bash
npx @openai/codex-security install-hook
```

对于拉取请求检查，扫描已提交的更改并设置严重性阈值：

```bash
npx @openai/codex-security scan . \
  --diff origin/main \
  --fail-on-severity high
```

当完整扫描发现问题达到或超过所选严重性时，会返回退出代码 `1`。请参阅 [在 CI 中运行扫描](ci.zh-CN.md) 了解完整的 GitHub 操作工作流程、工件处理和 SARIF 导出。

<a id="can-another-application-run-scans-directly"></a>

### 其他应用程序可以直接运行扫描吗

是的。使用 [Security TypeScript SDK](../sdk.zh-CN.md) 开始扫描、选择目标、检查结果和覆盖范围、跟踪进度以及从应用程序或开发人员工具应用成本控制。