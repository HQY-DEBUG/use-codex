> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/cli/bulk-scans.md)。

<a id="run-bulk-security-scans"></a>

# 批量安全扫描

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 `npx @openai/codex-security bulk-scan` 在一项活动中审查仓库。从您的个人 GitHub 帐户或组织中发现仓库，或提供将每个仓库固定到精确 Git 版本的 CSV。

`@openai/codex-security` 包是公开的。运行扫描需要 Codex Security 访问权限。按照[CLI 快速入门](../cli.zh-CN.md)安装CLI并登录。

<a id="choose-a-repository-source"></a>

## 选择仓库源

| 来源 | 何时使用 |
| ---------------- | --------------------------------------------------------------------------------------- |
| GitHub 发现 | 从您的个人 GitHub 帐户或组织中以交互方式选择仓库。 |
| CSV 库存 | 针对精确的仓库修订运行可重复的自动化活动。                |

这两个工作流程都会保存进度、保留每个仓库的结果，并让您在中断后恢复活动。

<a id="discover-github-repositories"></a>

## 发现 GitHub 仓库

使用 GitHub CLI 登录：

```bash
gh auth login
```

启动交互式批量扫描：

```bash
npx @openai/codex-security bulk-scan
```

CLI 将指导您完成以下步骤：

1. 选择您的个人 GitHub 帐户或组织。
2. 查看过去 90 天内活动的仓库。
3. 搜索仓库列表并选择要扫描的仓库。
4. 选择扫描结果的目录。
5. 查看选定的仓库并确认活动。

发现不包括存档的仓库和分支。 CLI 在`中记录每个选定仓库的确切默认分支提交<output-directory>/repositories.csv`。在您确认选择之前，不会开始扫描。

要使用 GitHub Enterprise Server，请首先登录到您的 GitHub 主机：

```bash
gh auth login --hostname github.example.com
```

启动仓库发现时设置 `GH_HOST`：

```bash
GH_HOST=github.example.com npx @openai/codex-security bulk-scan
```

交互式发现需要终端。对于 CI、容器或准备好的仓库列表，请改用 CSV 库存。

<a id="create-a-repository-csv"></a>

## 创建仓库 CSV

为每个仓库和固定修订创建一个包含一行的 CSV：

```csv
id,repository,revision,scope,mode,prompt
payments,https://github.com/example/payments.git,0123456789abcdef0123456789abcdef01234567,services/api,standard,Review payment authorization and refunds.
identity,https://github.com/example/identity.git,fedcba9876543210fedcba9876543210fedcba98,,deep,Review session and identity boundaries.
```

CSV 支持这些列：

| 列 | 必需 | 描述 |
| ------------ | -------- | ---------------------------------------------------------------------------------------------------------- |
| `id` | 是 | 唯一仓库标识符。使用字母、数字、句点、连字符或下划线。                      |
| `repository` | 是 | HTTPS URL、SSH URL 或本地仓库路径。从 CSV 目录解析相对路径。               |
| `revision` | 是 | 完整的 40 或 64 字符 Git 提交 SHA。不支持分支名称、标签和缩短的提交哈希。 |
| `scope` | 否 | 要扫描的仓库相对目录。省略该值以扫描完整仓库。                       |
| `mode` | 否 | `standard` 或 `deep`。省略该值以使用命令的选定模式。                                   |
| `prompt` | 否 | 扫描特定于此仓库的指令。                                                             |

要查找本地仓库的完整提交 SHA，请运行：

```bash
git -C /path/to/repository rev-parse HEAD
```

<a id="run-a-campaign-from-csv"></a>

## 从 CSV 运行营销活动

在仓库外部传递 CSV 和私有输出目录：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4
```

`--workers` 控制并发仓库扫描，默认为 `4`。它没有设置每次深度扫描中独立的标准扫描工作人员的数量；通过 [`[deep_scan]`](/codex/security/cli/reference#configure-deep-scans) 配置这些限制。使用 `--mode deep` 为没有自己的 `mode` 的行选择深度扫描。每个 CSV 行仍然可以选择自己的扫描模式和仓库范围。

设置 `[deep_scan].max_time_hours` 以限制活动中每次深度扫描的工作执行。 `--max-time-hours` 标志适用于 `scan`，而不适用于 `bulk-scan`。

CLI 签出每个固定修订版本，扫描选定的目标，记录结果，并删除临时仓库签出。仅当仓库的扫描完全覆盖并且所有必需的结果工件都存在时，仓库才算完成。

<a id="share-security-context-and-instructions"></a>

## 共享安全上下文和说明

使用 `--knowledge-base` 将架构文档、威胁模型或安全策略添加到每次扫描中。对更多文件或目录重复该标志：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --knowledge-base /path/to/architecture.md \
  --knowledge-base /path/to/security-policies
```

要添加共享扫描说明或在每次扫描后运行后续操作，请提供提示文件：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --scan-prompt-file scan-instructions.md \
  --post-scan-prompt-file follow-up.md
```

CLI 在共享扫描指令之后附加每个仓库的 CSV `prompt`。成功扫描以及覆盖不完整或错误的扫描后，后续指令将在同一经过身份验证的会话中运行，但在取消或扫描达到其成本限制后则不会运行。提示文件路径从当前目录解析。

<a id="choose-a-model-and-reasoning-effort"></a>

## 选择模型和推理工作

默认情况下，批量扫描使用 `gpt-5.6-sol` 和 `xhigh` 推理工作。要为 CSV 营销活动选择其他模型和工作：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4 \
  --model gpt-5.6-terra \
  --effort high
```

在交互式仓库发现过程中，相同的选项也起作用：

```bash
npx @openai/codex-security bulk-scan --model gpt-5.6-terra --effort high
```

支持的工作量级别为 `minimal`、`low`、`medium`、`high` 和 `xhigh`。

要使用 OpenRouter 或 Fireworks，请分别设置 `OPENROUTER_API_KEY` 或 `FIREWORKS_API_KEY`，并指定 `--provider` 和 `--model`。有关凭据和示例，请参阅 [OpenRouter 或 Fireworks 设置](reference.zh-CN.md#use-openrouter-or-fireworks) 或 [亚马逊基岩设置](reference.zh-CN.md#use-amazon-bedrock)。

<a id="review-campaign-results"></a>

## 查看活动结果

输出目录包含固定的活动、仅附加结果分类帐以及每个仓库和尝试的单独工件：

```text
security-scans/
├── manifest.json
├── results.jsonl
├── checkouts/
└── artifacts/
    ├── payments/
    │   └── attempt-1/
    │       ├── scan-manifest.json
    │       ├── findings.json
    │       ├── coverage.json
    │       └── report.md
    └── identity/
        └── attempt-1/
            ├── scan-manifest.json
            ├── findings.json
            ├── coverage.json
            └── report.md
```

- `manifest.json` 记录活动中的仓库、固定修订、范围、扫描模式以及共享或特定于仓库的指令。
- `results.jsonl` 记录每个仓库尝试、其状态、工件目录以及任何可用的成本或错误详细信息。
- `report.md` 为一次仓库尝试提供了一份可读的报告。
- `findings.json` 和 `coverage.json` 记录了该尝试的结果并审查了范围。

当您需要可移植结果时，导出一份已完成的仓库扫描：

```bash
npx @openai/codex-security export \
  /path/outside/repositories/security-scans/artifacts/payments/attempt-1 \
  --export-format sarif \
  --output /path/outside/repositories/payments.sarif
```

结果可以包含源代码摘录和漏洞详细信息。将输出目录保持私有，位于扫描仓库之外，并遵守适当的保留策略。

<a id="resume-a-campaign"></a>

## 恢复活动

使用相同的 CSV 和输出目录运行原始命令：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4
```

CLI 恢复未完成的仓库扫描并跳过已完成的扫描。不会重试覆盖不完整的扫描。他们的结果仍然可用，并且命令以代码 `2` 退出。

不要更改仓库清单或扫描现有输出目录的后续说明。 CLI 检查固定清单并拒绝不同的活动。当您更改仓库、修订版、范围、扫描模式或共享或特定于仓库的指令时，请使用新的输出目录。

<a id="retry-repository-errors"></a>

## 重试仓库错误

在临时签出或扫描错误后，使用 `--max-attempts` 重试仓库：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4 \
  --max-attempts 3
```

默认为每个仓库尝试一次。每次尝试都会收到自己的收据和工件目录。重试包括检出错误、扫描失败和缺少所需的工件。不会重试覆盖不完整的已完成扫描。

批量扫描使用这些退出代码：

| 退出代码 | 含义 |
| --------- | --------------------------------------------------------------------------------------------------------------------- |
| `0` | 每个仓库均已成功完成。                                                                              |
| `2` | 仓库无法完成、扫描覆盖不完整或者命令遇到输入或运行时错误。 |
| `130` | Ctrl-C 中断活动。                                                                                      |
| `143` | SIGTERM 终止了该活动。                                                                                      |

<a id="run-bulk-scans-in-docker"></a>

## 在 Docker 中运行批量扫描

[Codex Security 仓库](https://github.com/openai/codex-security) 包括一个强化的 Compose 配置，用于 Linux Docker 主机上的自动化 CSV 活动。主机必须支持非特权用户命名空间创建。

将仓库 CSV、扫描结果和登录状态安装在持久目录中。通过环境或秘密管理器提供 OpenAI 凭证。对于私有GitHub仓库，以相同的方式提供`GH_TOKEN`或`GITHUB_TOKEN`。

使用已安装的 CSV 和输出目录运行图像：

```bash
docker compose run --rm codex-security \
  bulk-scan /input/repositories.csv \
  --output-dir /output \
  --workers 4
```

使用相同的已安装 CSV 和输出目录来恢复活动。对于 GitHub Enterprise Server，将 `CODEX_SECURITY_GIT_HOST` 设置为您的 GitHub 主机。

对于每个可用标志，请参阅 [批量扫描命令参考](reference.zh-CN.md#codex-security-bulk-scan)。有关扫描覆盖范围和结果的常见问题，请参阅 [CLI 常见问题解答](faq.zh-CN.md)。