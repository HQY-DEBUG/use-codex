> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/cli.md)。

<a id="codex-security-cli-quickstart"></a>

# Security CLI 快速开始

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex Security 帮助安全和工程团队发现、确认和修复漏洞。使用其命令行界面 (CLI) 扫描您拥有或有权评估的仓库，随时间审查发现的结果，并在更改发生之前进行检查。

`@openai/codex-security` 包是公开的。运行扫描需要 Codex Security 访问权限。对于 Codex 中的交互式扫描，请从 [Codex Security 插件快速入门](plugin.zh-CN.md) 开始。对于连接的 GitHub 仓库，请参阅 [Codex Security 云设置](setup.zh-CN.md)。

<a id="check-the-prerequisites"></a>

## 检查先决条件

CLI 需要 Node.js 22（22.13.0 或更高版本）、24 或 26。扫描、批量扫描、导出、扫描历史记录和保存的结果也需要 Python 3.10 或更高版本。有关更多详细信息，请参阅 [身份验证和先决条件](cli/reference.zh-CN.md#authentication-and-prerequisites)。

<a id="set-up-and-verify-the-cli"></a>

## 设置并验证 CLI

使用 `npx` 运行 CLI 并检查其版本：

```bash
npx @openai/codex-security --version
```

要查看包版本及其捆绑插件的版本，请运行：

```bash
npx @openai/codex-security info --json
```

有关封装更改，请参阅 [CLI 和 SDK 版本](https://github.com/openai/codex-security/releases)。

列出可用的命令：

```bash
npx @openai/codex-security --help
```

另请参见 [CLI 参考](cli/reference.zh-CN.md)。

<a id="sign-in"></a>

## 登录

对于本地使用，请使用您的 ChatGPT 帐户登录：

```bash
npx @openai/codex-security login
```

在远程或无头计算机上，使用设备身份验证：

```bash
npx @openai/codex-security login --device-auth
```

对于 CI 和其他自动化工作流程，设置 OpenAI API 密钥：

```bash
export OPENAI_API_KEY="<your-api-key>"
```

有关 AWS 凭证，请参阅 [亚马逊基岩设置](cli/reference.zh-CN.md#use-amazon-bedrock)。对于[OpenRouter 或 Fireworks](cli/reference.zh-CN.md#use-openrouter-or-fireworks)，设置提供商的API密钥并选择带有`--provider`和`--model`的模型。

要在设置了 API 密钥的情况下使用 ChatGPT 登录，请明确选择它：

```bash
npx @openai/codex-security scan . --auth chatgpt
```

如需环境 API 密钥，请选择 API 密钥身份验证：

```bash
npx @openai/codex-security scan . --auth api-key
```

根据您的帐户和仓库，完整仓库扫描可能还需要 [网络可信访问](https://chatgpt.com/cyber)。

<a id="prepare-a-scan"></a>

## 准备扫描

选择您信任并有权评估的仓库。扫描使用您的本地操作系统权限，并且不会暂停以等待批准。扫描进程可以继承您的环境，因此在开始之前删除不相关的凭据。参见 [本地扫描权限](cli/reference.zh-CN.md#local-scan-permissions)。

选择仓库外部的目录来存放扫描结果：

```bash
REPOSITORY=/path/to/repository
SCAN_DIR=/path/outside/repository/codex-security-results
```

如果省略 `--output-dir`，Codex Security 会将结果保存在其自己的持久状态目录中。结果可以包括源代码摘录和漏洞详细信息，因此请选择私有位置和适当的保留策略。

如果默认状态目录不可写，请选择扫描仓库外部的可写目录：

```bash
export CODEX_SECURITY_STATE_DIR=/path/outside/repository/codex-security-state
```

在开始扫描之前检查仓库、目标和输出目录：

```bash
npx @openai/codex-security scan "$REPOSITORY" --output-dir "$SCAN_DIR" --dry-run
```

试运行会检查本地输入，包括任何 `--knowledge-base` 路径，而无需启动 Codex、加载凭据或探测插件的 Python 解释器。

<a id="run-your-first-scan"></a>

## 运行您的第一次扫描

运行标准扫描并将其结果保存在选定的目录中：

```bash
npx @openai/codex-security scan "$REPOSITORY" --output-dir "$SCAN_DIR"
```

交互式终端显示实时扫描仪表板。添加 `--headless` 以显示普通进度线。 CI 和没有交互式会话的终端会自动使用普通进度。

仪表板还显示实时会话详细信息。这些可能包含源代码或凭据，因此在共享之前请先查看它们。

默认情况下，CLI 将扫描进度及其完成摘要写入 stderr。它不会将完整的扫描结果打印到标准输出。完成的扫描会打印如下摘要：

```text
  REPORT    /path/outside/repository/codex-security-results/report.md

  结果 2（2 个确认了此扫描；0 个先前发现；1 个高，1 个中）
  覆盖范围完整
  ELAPSED   42s
  RESULTS   /path/outside/repository/codex-security-results
```

令牌使用情况和估计成本会在可用时显示。要将完整结果打印为机器可读的 JSON，请显式请求结构化输出：

```bash
npx @openai/codex-security scan "$REPOSITORY" --output-dir "$SCAN_DIR" --json
```

默认情况下，扫描仅报告，因此结果仍可供本地审查。当您准备好 [在 CI 中运行扫描](cli/ci.zh-CN.md) 时，您可能需要添加严重性阈值。

<a id="choose-a-model-and-reasoning-effort"></a>

## 选择模型和推理工作

默认情况下，扫描使用 `gpt-5.6-sol` 和 `xhigh` 推理工作。当任务需要时选择不同的模型和工作量：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --model gpt-5.6-terra \
  --effort high
```

支持的工作量级别为 `minimal`、`low`、`medium`、`high`、`xhigh` 和 `max`。

<a id="review-the-results"></a>

## 查看结果

打开 `report.md` 即可读取结果。扫描目录还包含自动化使用的结构化文件：

```text
codex-security-results/
├── scan-manifest.json
├── findings.json
├── coverage.json
├── report.md
├── artifacts/
└── exports/
    └── results.sarif       # when produced
```

- `scan-manifest.json`记录目标、范围、生产者和密封工件。
- `findings.json` 记录每个发现的严重性、置信度、位置、证据和补救措施。
- `coverage.json` 记录已审查的使用界面、排除、推迟的工作、未解决的问题和覆盖范围的完整性。

覆盖范围可以是 `complete`、`partial` 或 `unknown`。在将扫描视为审查证据之前，请阅读任何推迟的区域或未解决的问题。 [CLI 参考](cli/reference.zh-CN.md#scan-artifacts) 描述了完整的工件和输出合同。

<a id="review-and-patch-findings"></a>

## 审查和修补结果

在对结果进行完整的交互式扫描后，CLI 会提供一个结果浏览器。审查证据并选择要修复的发现。您可以在 Codex 桌面应用程序中找到保存的任务。

要在不使用浏览器的情况下修补重要且关键的发现：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --patch --patch-severity high --json
```

添加 `--create-pr` 以提交经过验证的补丁并打开 GitHub 拉取请求。

您还可以修补保存的结果或导入线性问题。请参阅 [`validate` 和 `patch` 参考](cli/reference.zh-CN.md#codex-security-validate-and-codex-security-patch)。

<a id="choose-the-next-scan"></a>

## 选择下一次扫描

当仓库包含单独的服务或包时，使用路径扫描：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --path services/billing \
  --path packages/auth
```

查看基础修订版和 `HEAD` 之间已提交的更改：

```bash
npx @openai/codex-security scan "$REPOSITORY" --diff origin/main --head HEAD
```

针对 `HEAD` 检查已暂存和未暂存的更改：

```bash
npx @openai/codex-security scan "$REPOSITORY" --working-tree --base HEAD
```

差异和工作树扫描期望仓库参数是 Git 工作树根。在开始差异扫描之前获取选定的修订版本。

当仓库或路径需要更广泛的审查时，请使用深度模式：

```bash
npx @openai/codex-security scan "$REPOSITORY" --mode deep
```

要控制工作人员、子智能体以及扫描何时停止：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --mode deep \
  --workers 2 \
  --subagents 0 \
  --stop-after-no-new 3 \
  --max-discovery-runs 10 \
  --max-time-hours 1.5
```

这些选项需要深度模式，该模式支持仓库和路径目标，而不是差异或工作树扫描。在这里，`--workers` 在一次扫描中控制独立的标准扫描工作人员； `bulk-scan --workers` 控制并发仓库扫描。 `--max-time-hours` 接受最大 `96` 的正数，包括小数小时。在极限情况下，扫描会停止未完成的工作人员，保留已完成的扫描结果，并将它们聚合到最终报告中。

<a id="add-architecture-and-security-context"></a>

## 添加架构和安全上下文

提供架构文档、威胁模型或安全策略作为扫描上下文。这有助于 Codex Security 根据您的系统实际工作方式评估结果：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --knowledge-base /path/to/architecture.md \
  --knowledge-base /path/to/security-policies
```

<a id="add-custom-scan-instructions"></a>

## 添加自定义扫描说明

添加将扫描重点放在您的安全优先级上的说明。使用第二个文件进行后续说明：

```bash
npx @openai/codex-security scan "$REPOSITORY" \
  --scan-prompt-file /path/to/scan.md \
  --post-scan-prompt-file /path/to/follow-up.md
```

成功扫描和覆盖不完整或错误的扫描后，后续操作在同一经过身份验证的会话中运行。如果后续失败，CLI 会报告警告并保留已完成的扫描。取消或扫描达到成本限制后，它不会运行。这两个选项也适用于 `bulk-scan`； CSV `prompt` 列添加特定于仓库的说明。

<a id="set-a-scan-budget"></a>

## 设置扫描预算

当估计模型成本超过美元限制时，使用 `--max-cost` 停止扫描：

```bash
npx @openai/codex-security scan "$REPOSITORY" --max-cost 5
```

已在处理的请求可能会略高于限制完成。如果在 Codex Security 聚合已完成的工作器结果后深度扫描达到限制，CLI 将保存已完成的报告，将其覆盖范围标记为 `partial`，并返回退出代码 `2`。如果扫描无法生成完整的报告，则任何可用的部分输出都会保留在磁盘上。

<a id="scan-changes-before-each-commit"></a>

## 每次提交前扫描更改

为您的仓库安装 Git 预提交安全检查：

```bash
npx @openai/codex-security install-hook
```

检查在每次提交之前扫描暂存和未暂存的更改。它可以阻止高严重性的发现和扫描错误，而无需替换现有的预提交脚本。

<a id="scan-repositories-in-bulk"></a>

## 批量扫描仓库

在发现仓库之前登录 GitHub：

```bash
gh auth login
```

从您的 GitHub 帐户或组织中发现并选择仓库：

```bash
npx @openai/codex-security bulk-scan
```

交互流程不包括存档仓库和分叉。它会要求您在扫描之前确认所选的仓库。

要扫描准备好的仓库列表，请提供 CSV 和输出目录：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4
```

再次运行相同的命令以恢复现有的批量扫描。 Codex Security 跳过已完成的仓库。当您想要重试临时仓库或扫描错误时，请添加 `--max-attempts 3`。

有关 GitHub 发现、CSV 准备、活动结果和 Docker 设置，请参阅 [运行批量安全扫描](cli/bulk-scans.zh-CN.md)。

<a id="run-bulk-scans-in-docker"></a>

## 在 Docker 中运行批量扫描

如果您的访问权限包括 Codex Security Docker 映像，请在 Linux Docker 主机上使用提供的强化 Compose 配置和安全配置文件。主机必须支持非特权用户命名空间创建。提供仓库 CSV，将结果和登录状态保存在持久安装的目录中，并通过您的环境或秘密管理器提供凭据：

```bash
docker compose run --rm codex-security \
  bulk-scan /input/repositories.csv \
  --output-dir /output \
  --workers 4
```

容器在没有交互式提示的情况下运行批量扫描。当您想要以交互方式发现仓库时，请使用 Docker 外部的 CLI。对于私有仓库，请通过您的环境或秘密管理器提供 `GH_TOKEN` 或 `GITHUB_TOKEN`。 [登录要求](#sign-in)，包括帐户和仓库访问权限，也适用于容器化扫描。

<a id="revisit-a-saved-scan"></a>

## 重新访问保存的扫描

列出仓库中保存的扫描：

```bash
npx @openai/codex-security scans list "$REPOSITORY"
```

从结果中复制扫描 ID 以检查其结果和配置：

```bash
npx @openai/codex-security scans show SCAN_ID
```

要检查扫描及其工作人员保存的事件：

```bash
npx @openai/codex-security scans logs SCAN_ID
```

保存的日志未经过编辑，并且可以包含源代码或凭据。在分享之前先回顾一下它们。

列出仓库扫描中未发现的结果：

```bash
npx @openai/codex-security findings list "$REPOSITORY"
```

如果最新的扫描未能证实较早的发现，则该发现将保持开放状态。

要将已审核的结果标记为误报，请解释该结果不适用的原因：

```bash
npx @openai/codex-security findings false-positive FINDING_OCCURRENCE_ID \
  --reason "The route already checks permissions"
```

稍后的扫描会考虑该解释，但仍重新检查当前代码。

使用其原始配置对当前结帐运行相同的扫描：

```bash
npx @openai/codex-security scans rerun SCAN_ID
```

比较两次扫描以查找新的、持续存在的、重新打开的、已解决的或未知的发现：

```bash
npx @openai/codex-security scans compare PREVIOUS_SCAN_ID CURRENT_SCAN_ID
```

比较会根据根本原因自动匹配结果并重复使用已保存的匹配项。

有关批量扫描 CSV 格式、扫描历史过滤器和命令选项，请参阅 [CLI 参考](cli/reference.zh-CN.md)。

继续适合您目标的工作流程：

- [运行批量安全扫描](cli/bulk-scans.zh-CN.md) 用于发现 GitHub 仓库或扫描固定的 CSV 库存。
- [阅读 CLI 常见问题解答](cli/faq.zh-CN.md) 有关扫描历史记录、误报反馈、覆盖范围和修复验证的答案。
- [在 CI 中运行扫描](cli/ci.zh-CN.md) 用于审查拉取请求、保留结果并设置严重性策略。
- [使用 CLI 参考](cli/reference.zh-CN.md) 检查每个标志、输出格式、工件和退出代码。
- [集成 TypeScript SDK](sdk.zh-CN.md) 从应用程序或开发人员工具运行扫描。