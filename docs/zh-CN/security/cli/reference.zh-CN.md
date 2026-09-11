> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/cli/reference.md)。

<a id="codex-security-cli-reference"></a>

# Security CLI 命令参考

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用此参考来检查支持的 `codex-security` 命令、标志、输出格式和退出行为。对于引导式首次扫描，请从 [CLI 快速入门](../cli.zh-CN.md) 开始。

`@openai/codex-security` 包是公开的。运行扫描需要 Codex Security 访问权限。扫描使用您的本地权限，并且不会暂停以等待批准。在开始之前，请查看 [本地扫描权限](#local-scan-permissions)。

使用 `npx @openai/codex-security` 运行 CLI。

<a id="command-overview"></a>

## 命令概述

```text
用法：codex-security [--version] <命令> [选项]
```

CLI 提供以下命令：

| 命令 | 用途 |
| ----------------------------- | ----------------------------------------------------- |
| `codex-security scan` | 运行 Codex Security 扫描。                            |
| `codex-security install-hook` | 安装 Git 预提交安全扫描。               |
| `codex-security bulk-scan` | 发现仓库并运行可恢复批量扫描。   |
| `codex-security scans` | 列出、检查、比较和检索保存的扫描日志。 |
| `codex-security findings` | 查看并更新保存的安全结果。            |
| `codex-security export` | 将完成的结果导出为 CSV、JSON 或 SARIF。     |
| `codex-security publish` | 将完成的扫描结果发布到 Linear。            |
| `codex-security validate` | 检查一项或多项候选安全结果。        |
| `codex-security patch` | 修补一个或多个安全问题。                    |
| `codex-security login` | 登录、存储凭据或检查登录状态。  |
| `codex-security logout` | 删除存储的签到。                            |
| `codex-security info` | 显示只读 SDK 和捆绑插件元数据。       |

CLI 还提供以下集成命令：

| 命令 | 用途 |
| ---------------------------- | ------------------------------------- |
| `codex-security completions` | 生成 shell 完成脚本。    |
| `codex-security mcp` | 将 CLI 注册为 MCP 服务器。    |
| `codex-security skills` | 将Codex Security技能同步给特工。 |

列出所有可用命令：

```bash
npx @openai/codex-security --help
```

将 `--help` 添加到命令中以检查其参数和选项：

```bash
npx @openai/codex-security scan --help
```

`codex-security --version` 打印安装的版本并退出。 `codex-security info --json` 报告 SDK 和捆绑插件版本。这两个命令都不需要 Python。

<a id="discover-commands-and-connect-agents"></a>

### 发现命令并连接智能体

打印智能体可读的命令清单：

```bash
npx @openai/codex-security --llms
```

检查 JSON 格式的扫描参数架构：

```bash
npx @openai/codex-security scan --schema --format json
```

为 Bash 生成 shell 补全：

```bash
npx @openai/codex-security completions bash
```

对于这些外壳，将 `bash` 替换为 `zsh` 或 `fish`。

扫描结果支持`--format toon|json|yaml|jsonl`和`--full-output`。这个框架级别`--format`与`--export-format`，它选择从已完成的扫描导出的工件的格式。全局命令帮助还列出了`md`，但扫描结果不支持Markdown输出。

将 CLI 注册为 MCP 服务器：

```bash
npx @openai/codex-security mcp add
```

将 Codex Security 技能同步给您的客服人员：

```bash
npx @openai/codex-security skills add
```

MCP 仅公开只读 `info` 元数据命令。扫描、导出、身份验证、验证和修补仍然仅通过 CLI 进行。

<a id="codex-security-scan"></a>

## `codex-security scan`

对仓库、选定的路径、提交的更改或工作树运行扫描。

```text
用法：codex-安全扫描 [-h] [--auth {auto,chatgpt,api-key}]
                           [--provider {openai,openrouter,fireworks,amazon-bedrock}]
                           [--path PATH | --diff BASE | --working-tree]
                           [--head HEAD] [--base BASE]
                           [--knowledge-base PATH] [--scan-prompt-file FILE]
                           [--post-scan-prompt-file FILE]
                           [--mode {standard,deep}] [--workers N]
                           [--subagents N] [--stop-after-no-new N]
                           [--max-discovery-runs N] [--max-time-hours HOURS]
                           [--model MODEL]
                           [--effort {minimal,low,medium,high,xhigh,max}]
                           [--output-dir DIR]
                           [--archive-existing]
                           [--plugin-path PATH] [--python PATH]
                           [--codex KEY=VALUE] [--fail-on-severity LEVEL]
                           [--patch] [--patch-severity {critical,high,medium,low}]
                           [--create-pr]
                           [--max-cost USD] [--dry-run] [--headless] [--verbose]
                           [--json] [--format {toon,json,yaml,jsonl}]
                           [--full-output] [repository]
```

`repository` 默认为当前目录。

<a id="select-scan-authentication"></a>

### 选择扫描认证

使用默认值 `--auth auto` 自动选择凭据。当 ChatGPT 登录和 `OPENAI_API_KEY` 或 `CODEX_API_KEY` 都可用时，带有文本输出的交互式扫描会询问要使用哪个凭据。 CI、JSON 和 JSONL 扫描以及其他没有交互式终端的扫描使用环境 API 密钥。试运行不会提示或加载凭据。

要使用您存储的凭据，请传递 `--auth chatgpt`：

```bash
npx @openai/codex-security scan . --auth chatgpt
```

要使用环境 API 密钥，请传递 `--auth api-key`：

```bash
npx @openai/codex-security scan . --auth api-key
```

要将存储的凭据设置为自动默认值，请运行 `unset OPENAI_API_KEY CODEX_API_KEY`。

<a id="use-openrouter-or-fireworks"></a>

### 使用 OpenRouter 或 Fireworks

选择 OpenRouter 及其 API 密钥和显式模型：

```bash
export OPENROUTER_API_KEY="your-openrouter-api-key"
npx @openai/codex-security scan . \
  --provider openrouter \
  --model anthropic/claude-sonnet-4.5
```

选择 Fireworks 及其 API 密钥和显式模型：

```bash
export FIREWORKS_API_KEY="your-fireworks-api-key"
npx @openai/codex-security scan . \
  --provider fireworks \
  --model accounts/fireworks/models/qwen3-235b-a22b
```

两个提供商还支持 `bulk-scan`。

<a id="use-amazon-bedrock"></a>

### 使用亚马逊基岩

选择带有 `--provider amazon-bedrock` 的 Amazon Bedrock 并指定带有 `--model` 的显式 Bedrock 模型：

```bash
npx @openai/codex-security scan . \
  --provider amazon-bedrock \
  --model openai.gpt-5.6-sol
```

设置 `AWS_REGION` 并使用 `AWS_BEARER_TOKEN_BEDROCK`、标准 AWS 访问密钥、AWS 配置文件、Web 身份、容器凭证或默认 AWS 凭证链进行身份验证。 Bedrock 扫描使用 AWS 凭证，而不是 `--auth`、ChatGPT 登录或 OpenAI API 密钥。 `scan`和`bulk-scan`都支持`--provider`。

<a id="select-the-scan-target"></a>

### 选择扫描目标

为每次扫描选择一种目标类型。

| 参数 | 说明 |
| ------------------------ | ------------------------------------------------------------------------------- |
| `--path PATH` | 扫描相对于仓库的路径。重复该标志以获得更多路径。         |
| `--diff BASE` | 扫描从 `BASE` 到 `--head` 的提交更改。头默认为`HEAD`。    |
| `--head HEAD` | 设置 `--diff` 的头部版本。                                             |
| `--working-tree` | 针对 `--base` 扫描已暂存和未暂存的更改。底座默认为 `HEAD`。 |
| `--base BASE` | 设置 `--working-tree` 的基本版本。                                     |
| `--mode {standard,deep}` | 选择扫描模式。默认值为 `standard`。                                |

`--path`、`--diff` 和 `--working-tree` 是互斥的。 `--head` 需要 `--diff`，`--base` 需要 `--working-tree`。深度模式支持仓库和路径目标。

差异和工作树扫描要求仓库参数是 Git 工作树根。所选参考必须存在于该结帐中。

扫描整个仓库：

```bash
npx @openai/codex-security scan .
```

扫描选定的路径：

```bash
npx @openai/codex-security scan . --path src --path tests
```

扫描提交的更改：

```bash
npx @openai/codex-security scan . --diff origin/main --head HEAD
```

扫描暂存和未暂存的更改：

```bash
npx @openai/codex-security scan . --working-tree --base HEAD
```

对仓库进行更深入的审查：

```bash
npx @openai/codex-security scan . --mode deep
```

<a id="configure-deep-scans"></a>

### 配置深度扫描

将这些选项与 `--mode deep` 一起使用来控制工作线程并发性和运行时：

| 参数 | 说明 |
| ------------------------ | -------------------------------------------------------------------------------------- |
| `--workers N` | 并发独立标准扫描工作人员的限制。默认为 `4`。                |
| `--subagents N` | 每个工作人员可用的子智能体。默认为 `3`。                                   |
| `--stop-after-no-new N` | `N` 连续完成的工作扫描未发现新问题后停止。默认为 `4`。 |
| `--max-discovery-runs N` | 独立标准扫描运行总数的限制。默认为 `40`。                       |
| `--max-time-hours HOURS` | Worker 执行时间限制（以小时为单位）。默认为`96`；接受分数。             |

`--subagents` 接受零或正整数。 `--max-time-hours` 接受不大于 `96` 的正数。其余选项需要正整数。这些选项不适用于标准扫描。

例如，使用两个工作线程，最多允许运行十次，并在 1.5 小时后停止工作线程执行：

```bash
npx @openai/codex-security scan . \
  --mode deep \
  --workers 2 \
  --subagents 0 \
  --stop-after-no-new 3 \
  --max-discovery-runs 10 \
  --max-time-hours 1.5
```

当时间限制到期时，扫描会停止未完成的工作人员，保留已完成的扫描结果，并将它们聚合到最终报告中。如果没有工作人员完成源代码审查，扫描将记录部分覆盖并返回退出代码 `2`。

在 `~/.codex/codex-security/config.toml` 中设置持久默认值，或者在设置 `CODEX_HOME` 时在 `$CODEX_HOME/codex-security/config.toml` 中设置持久默认值：

```toml
[deep_scan]
workers = 2
subagents = 0
stop_after_no_new = 3
max_discovery_runs = 10
max_time_hours = 1.5
```

命令行选项会覆盖这些默认值。 `scan --workers` 在一次深度扫描中控制独立的标准扫描工作人员； `bulk-scan --workers` 控制并发仓库扫描。仅在TOML文件中设置`stop_after_consecutive_errors`；默认值为 `3`。

<a id="add-security-context"></a>

### 添加安全上下文

使用`--knowledge-base PATH`提供架构文档、威胁模型或安全策略。对更多文件或目录重复该选项：

```bash
npx @openai/codex-security scan . \
  --knowledge-base /path/to/architecture.md \
  --knowledge-base /path/to/security-policies
```

支持的文档包括 `.md`、`.markdown`、`.txt`、`.pdf` 和 `.docx` 文件。 CLI 递归搜索目录，拒绝链接的输入路径，跳过链接的目录条目，并将提取的文档内容保留在保存的扫描结果之外。

<a id="add-scan-instructions"></a>

### 添加扫描指令

要添加扫描指令，请提供带有 `--scan-prompt-file` 的文本或 Markdown 文件。在成功扫描和扫描覆盖不完整或错误后，使用 `--post-scan-prompt-file` 在同一经过身份验证的会话中运行后续指令：

```bash
npx @openai/codex-security scan . \
  --scan-prompt-file security-focus.md \
  --post-scan-prompt-file follow-up.md
```

例如，使用扫描提示关注授权边界，并要求后续在扫描目录中写入新的`post-scan-summary.md`。如果后续失败，CLI 会报告警告并保留已完成的扫描。取消后或扫描达到其成本限制后，后续操作不会运行。

<a id="set-output-and-policy-options"></a>

### 设置输出和策略选项

使用这些选项可以保留工件、保留早期结果或创建机器可读的结果。

| 参数 | 说明 |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `--output-dir DIR` | 将扫描工件写入封闭的 Git 工作树之外的私有目录。默认为持久 Codex Security 状态。 |
| `--archive-existing` | 将现有结果移至 `DIR.previous-<timestamp>-<id>` and start with an empty output directory. Requires `--输出目录`。  |
| `--fail-on-severity LEVEL` | 当完成的扫描报告结果等于或高于 `critical`、`high`、`medium` 或 `low` 时，返回出口 `1`。                  |
| `--patch` | 完整扫描后修复并验证选定的结果。                                                                      |
| `--patch-severity LEVEL` | 补丁结果等于或高于 `critical`、`high`、`medium` 或 `low`。默认为 `low`。                                        |
| `--create-pr` | 提交经过验证的补丁文件并打开 GitHub 拉取请求。需要 `--patch`。                                              |
| `--max-cost USD` | 当估计模型成本超过指定的美元金额时停止扫描。                                                  |
| `--dry-run` | 检查仓库、目标、知识库、输出目录和 Codex 配置，而无需启动扫描。             |
| `--headless` | 显示纯文本进度而不是交互式扫描仪表板。                                                          |
| `--verbose` | 将编辑的生命周期、身份验证、进度和成本诊断打印到 stderr。                                          |
| `--json` | 打印清单、调查结果、覆盖范围、路径，并将元数据转换为一个 JSON 文档。                                           |
| `--format FORMAT` | 将完整的扫描结果打印为 `toon`、`json`、`yaml` 或 `jsonl`。                                                        |
| `--full-output` | 使用默认结构化输出格式打印完整结果。                                                        |

成本限制是一个估计值，而不是硬性支出上限。已在处理的请求可能会略高于限制完成。如果 Codex Security 聚合已完成的工作结果后深度扫描达到限制，CLI 会密封可用结果，将覆盖范围标记为 `partial`，并返回退出代码 `2`。否则，它返回 `2` 并将任何可用的部分输出保留在磁盘上。

当您省略 `--output-dir` 时，结果将保留在 `$CODEX_HOME/state/plugins/codex-security/scans/ 下<repository>`. `CODEX_HOME` defaults to `~/.codex`. Set `CODEX_SECURITY_STATE_DIR` to keep results under `$CODEX_SECURITY_STATE_DIR/扫描/<repository>` 相反。这些目录可以包含源代码摘录和漏洞详细信息，因此请相应地管理其权限和保留。

工作台将扫描历史记录保存在 `$CODEX_HOME/state/plugins/codex-security/workbench.sqlite3` 中。设置 `CODEX_SECURITY_STATE_DIR` 也会移动工作台数据库。

输出目录必须位于扫描目录和任何封闭的 Git 工作树之外。扫描可以用 `--archive-existing` 替换现有结果目录。

要在重用输出目录之前保留早期结果：

```bash
npx @openai/codex-security scan . \
  --output-dir /path/outside/repository/results \
  --archive-existing
```

默认情况下，扫描仅报告。添加 `--fail-on-severity` 以评估 CI 中的严重性策略：

```bash
npx @openai/codex-security scan . \
  --diff origin/main \
  --output-dir /path/outside/repository/results \
  --json \
  --fail-on-severity high \
  > /path/outside/repository/codex-security.json
```

空运行检查本地输入，包括知识库文档，无需加载凭据、启动 Codex 或探测插件的 Python 解释器：

```bash
npx @openai/codex-security scan . \
  --output-dir /path/outside/repository/results \
  --dry-run
```

<a id="configure-the-runtime"></a>

### 配置运行时

当您需要显式模型、解释器、插件或 Codex 配置值时，请使用运行时选项。

| 参数 | 说明 |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `--auth {auto,chatgpt,api-key}` | 选择扫描凭据。默认值为 `auto`。                                                      |
| `--provider {openai,openrouter,fireworks,amazon-bedrock}` | 选择推理提供程序。默认值为 `openai`。                                                  |
| `--model MODEL` | 选择模型。默认值为 `gpt-5.6-sol`。 OpenRouter、Fireworks 和 Amazon Bedrock 需要。  |
| `--effort {minimal,low,medium,high,xhigh,max}` | 选择模型的推理工作量。默认值为 `xhigh`。                                             |
| `--plugin-path PATH` | 使用 Codex Security 插件目录或 ZIP 覆盖捆绑的插件。                             |
| `--python PATH` | 选择插件运行时的 Python 解释器。                                                    |
| `--codex KEY=VALUE` | 覆盖隔离的 Codex 配置值。值使用 TOML 语法。重复该标志以获得更多值。 |

要选择不同的模型和推理工作而不编写 TOML：

```bash
npx @openai/codex-security scan . --model gpt-5.6-terra --effort high
```

引用通过 `--codex` 传递的字符串值，以便 TOML 解析器接收一个字符串：

```bash
npx @openai/codex-security scan . --codex 'model="gpt-5.6-terra"'
```

<a id="codex-security-install-hook"></a>

## `codex-security install-hook`

为当前仓库安装 Git 预提交安全检查：

```bash
npx @openai/codex-security install-hook
```

检查会在每次提交之前扫描暂存和未暂存的更改，并阻止高严重性发现或扫描错误。它尊重 `core.hooksPath` 并且不会替换现有的预提交脚本。需要时设置不同的严重性阈值：

```bash
npx @openai/codex-security install-hook . --fail-on-severity medium
```

<a id="codex-security-bulk-scan"></a>

## `codex-security bulk-scan`

发现并扫描 GitHub 仓库，或从仓库 CSV 运行可恢复扫描：

有关 GitHub 发现、CSV 库存、活动结果和容器化扫描的完整指南，请参阅 [运行批量安全扫描](bulk-scans.zh-CN.md)。

```text
用法： codex-security 批量扫描 [input] [--output-dir DIR]
                                [--workers N] [--mode {standard,deep}]
                                [--provider {openai,openrouter,fireworks,amazon-bedrock}]
                                [--model MODEL]
                                [--effort {minimal,low,medium,high,xhigh,max}]
                                [--knowledge-base PATH]
                                [--scan-prompt-file FILE]
                                [--post-scan-prompt-file FILE]
                                [--max-attempts N] [--plugin-path PATH]
                                [--python PATH] [--codex KEY=VALUE]
```

运行不带参数的 `npx @openai/codex-security bulk-scan` 以交互方式选择仓库。此流程需要 GitHub CLI 登录。

要在交互式发现期间选择模型和推理工作：

```bash
npx @openai/codex-security bulk-scan --model gpt-5.6-terra --effort high
```

对于准备好的仓库列表，请提供 CSV 和 `--output-dir`：

```bash
npx @openai/codex-security bulk-scan repositories.csv \
  --output-dir /path/outside/repositories/security-scans \
  --workers 4
```

CSV 需要 `id`、`repository` 和 `revision` 列。修订必须是完整的提交哈希。可选的 `scope`、`mode` 和 `prompt` 列配置单独的仓库：

```csv
id,repository,revision,scope,mode,prompt
service,https://github.com/example/service.git,0123456789abcdef0123456789abcdef01234567,src,standard,Review authorization boundaries.
```

使用 `--knowledge-base PATH` 在每个仓库之间共享安全文档。使用`--scan-prompt-file FILE`添加共享扫描指令； CSV `prompt` 列在共享提示之后添加特定于仓库的说明。 `--post-scan-prompt-file FILE` 在每次扫描后运行后续指令，包括覆盖不完整或错误的扫描。取消后或扫描达到其成本限制后，它不会运行。

`--workers` 限制同时仓库扫描，默认为 `4`。 `--mode` 默认为 `standard`，`--max-attempts` 默认为 `1`。设置 `--max-attempts` 以重试仓库或扫描错误。不会重试覆盖不完整的已完成扫描。他们的结果仍然可用，并且命令返回退出代码 `2`。

再次运行相同的命令以从现有输出目录恢复。 CLI 会跳过已完成的扫描，包括覆盖不完整的扫描。

对于容器化活动，请参阅 [在 Docker 中运行批量扫描](bulk-scans.zh-CN.md#run-bulk-scans-in-docker)。

<a id="codex-security-scans"></a>

## `codex-security scans`

<a id="find-saved-scans"></a>

### 查找保存的扫描

列出当前目录的已保存扫描：

```bash
npx @openai/codex-security scans
```

列出不同仓库的扫描：

```bash
npx @openai/codex-security scans list /path/to/repository
```

查找存储在特定输出目录下的扫描：

```bash
npx @openai/codex-security scans list --scan-root /path/outside/repository/results
```

<a id="inspect-or-repeat-a-scan"></a>

### 检查或重复扫描

显示保存的扫描结果和配置：

```bash
npx @openai/codex-security scans show SCAN_ID
```

添加 `--show-linked-findings` 以包括从早期扫描中查找链接。

使用其原始配置针对当前结帐重新运行扫描：

```bash
npx @openai/codex-security scans rerun SCAN_ID
```

重新运行需要原始扫描记录的插件版本。如果安装的版本不同，该命令将停止，而不是使用不同的插件运行。

<a id="inspect-saved-scan-logs"></a>

### 检查保存的扫描日志

读取扫描及其工作人员的完整已保存会话事件。这些日志未经编辑，可能包含源代码或凭据，因此在共享之前请先查看它们：

```bash
npx @openai/codex-security scans logs SCAN_ID
```

添加 `--json` 以获得包含完整信息的机器格式结果。

<a id="match-and-compare-findings"></a>

### 匹配和比较结果

比较两次扫描以查找新的、持续存在的、重新打开的、已解决的和未知的发现：

```bash
npx @openai/codex-security scans compare PREVIOUS_SCAN_ID CURRENT_SCAN_ID
```

比较会自动匹配具有相同根本原因的结果并重复使用已保存的匹配项。要显式保存匹配项，请使用 `scans match`：

```bash
npx @openai/codex-security scans match PREVIOUS_SCAN_ID CURRENT_SCAN_ID
```

当后来的扫描覆盖不完整或未覆盖发现物的原始位置时，发现物是未知的。当您需要重新计算现有匹配时，将 `--force` 添加到 `match`。

要匹配当前仓库的所有已完成扫描，包括来自其他签出的扫描：

```bash
npx @openai/codex-security scans match --all
```

即使重新运行相同的配置，扫描结果也可能会有所不同。匹配和比较追踪变化；它们不会使结果具有确定性或证明漏洞不再存在。使用 `validate` 根据当前代码重新检查安全关键发现。

<a id="codex-security-findings"></a>

## `codex-security findings`

列出当前仓库扫描中未发现的结果：

```bash
npx @openai/codex-security findings list
```

传递仓库路径以检查另一个检出目录：

```bash
npx @openai/codex-security findings list /path/to/repository
```

添加 `--json` 以进行结构化输出。该列表列出了最新扫描中看到的发现以及该扫描中未确认的早期发现。

请注意，早期的发现在解决或被驳回之前一直保持开放状态（最新扫描中的缺席并不被解释为问题已修复的证据）。

要将审查结果记录为误报：

```text
用法：法典安全调查结果误报 OCCURRENCE_ID
                       --reason REASON
```

检查保存的扫描以识别发现的情况：

```bash
npx @openai/codex-security scans show SCAN_ID
```

记录误报的具体解释：

```bash
npx @openai/codex-security findings false-positive FINDING_OCCURRENCE_ID \
  --reason "The framework escapes this input before it reaches the query"
```

原因不能为空。 Codex Security 保存仓库的决策并将其作为未来扫描的上下文。每次扫描都会独立地重新检查当前源、控件和可达性。先前的决定不会抑制规则、路径或漏洞类别。

<a id="codex-security-export"></a>

## `codex-security export`

从已完成的密封扫描中导出 CSV、JSON 或 SARIF。导出会在写入输出之前验证扫描工件，并保持 Codex 运行时和凭证不变。

```text
用法：codex-security 导出 [--export-format {csv,json,sarif}]
                             [--output FILE|-] [--source-root PATH]
                             [--python PATH] scan_dir
```

`scan_dir` 是完成的扫描目录。

| 参数 | 说明 |
| ---------------------------------- | ------------------------------------------------------------------------------------------- |
| `--export-format {csv,json,sarif}` | 选择导出格式。默认值为 `sarif`。                                           |
| `--output FILE\|-` | 将所选格式写入文件或标准输出。默认为当前目录中的文件。 |
| `--source-root PATH` | 使用仓库签出将源代码行指纹添加到 SARIF。                          |
| `--python PATH` | 为捆绑导出器选择 Python 解释器。                                     |

`--source-root` 仅适用于 `--export-format sarif`。 JSON 保留密封的调查结果文档。 CSV 包含可移植查找列，不包括本地工作台分类状态。

如果没有 `--output`，CLI 将在当前工作目录中将 SARIF 写入 `results.sarif`、将 JSON 写入 `findings.json`、将 CSV 写入 `findings.csv`。导出可以包含源代码摘录和漏洞详细信息。在仓库外部运行命令或通过扫描结帐外部的私有路径传递 `--output`。

将 SARIF 写入文件：

```bash
npx @openai/codex-security export /path/to/scan \
  --export-format sarif \
  --source-root /path/to/repository \
  --output /path/outside/repository/exports/results.sarif
```

将 SARIF 写入标准输出：

```bash
npx @openai/codex-security export /path/to/scan \
  --export-format sarif \
  --source-root . \
  --output -
```

将结果导出为 JSON：

```bash
npx @openai/codex-security export /path/to/scan \
  --export-format json \
  --output /path/outside/repository/exports/findings.json
```

将结果导出为 CSV：

```bash
npx @openai/codex-security export /path/to/scan \
  --export-format csv \
  --output /path/outside/repository/exports/findings.csv
```

<a id="codex-security-publish-scan"></a>

## `codex-security publish scan`

将完整扫描的每个发现发布到 Linear：

```text
用法：codex-security发布扫描[SCAN_DIR]——线性
                                   [--linear-team TEAM_ID]
                                   [--project PROJECT_ID]
                                   [--linear-api-key KEY]
                                   [--linear-assignee EMAIL_OR_USER_ID]
                                   [--dry-run] [--json]
```

`SCAN_DIR` 必须包含完整的密封扫描件。在交互式终端中省略它可以从本地扫描历史记录中选择已完成的扫描。创建问题还需要扫描及其结果存在于本地扫描历史记录中。空运行可验证密封的工件，而无需进行持久性检查。

| 参数 | 说明 |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--to linear` | 发布到线性。这个参数是必需的。                                                                                                                    |
| `--linear-team TEAM_ID` | 选择线性团队。省略时使用 `CODEX_SECURITY_LINEAR_TEAM`；其中之一是必需的。                                                                 |
| `--project PROJECT_ID` | 选择线性项目。省略时使用 `CODEX_SECURITY_LINEAR_PROJECT`。如果两者均未设置，则会直接在团队中创建问题。                          |
| `--linear-api-key KEY` | 使用 Linear 个人 API 密钥直接发布。省略时使用 `CODEX_SECURITY_LINEAR_API_KEY`。                                                         |
| `--linear-assignee EMAIL_OR_USER_ID` | 通过电子邮件地址或线性用户 ID 分配创建的问题。需要 `--linear-api-key` 或 `CODEX_SECURITY_LINEAR_API_KEY`。省略时，问题仍处于未分配状态。 |
| `--dry-run` | 准备问题有效负载，无需启动 Codex、联系 Linear、创建问题或写入发布状态。                                                 |
| `--json` | 将结构化发布结果写入标准输出。 stderr 仍取得进展。                                                                                      |

线性问题描述和试运行输出可以包括源代码片段和漏洞详细信息。仅发布到授权的 Linear 团队或项目，并将保存的输出视为敏感输出。

每个非试运行调用都会尝试为每个发现创建一个新问题。再次发布相同的扫描不会匹配、更新或重复使用现有问题。如果某些结果失败，该命令将保留成功创建的问题并返回退出代码 `2`。对于 `--json`，请在重试之前检查 `created` 和 `failed` 结果以避免重复。

发布前预览问题有效负载：

```bash
npx @openai/codex-security publish scan /path/to/completed-scan \
  --to linear \
  --linear-team TEAM_ID \
  --dry-run \
  --json
```

<a id="publish-with-the-connected-linear-app"></a>

### 使用连接的 Linear 应用程序进行发布

如果没有 Linear API 密钥，该命令将使用您现有的配置和连接的 Linear 应用程序启动 Codex。在发布之前登录并连接 Linear 到您的 Codex 帐户：

```bash
npx @openai/codex-security login
npx @openai/codex-security publish scan /path/to/completed-scan \
  --to linear \
  --linear-team TEAM_ID \
  --project PROJECT_ID
```

<a id="publish-with-a-linear-api-key"></a>

### 使用 Linear API 密钥发布

提供 `--linear-api-key` 或 `CODEX_SECURITY_LINEAR_API_KEY` 直接通过 Linear API 发布，并且不会启动 Codex。除非您选择受让人，否则直接发布会留下未分配的问题：

```bash
export CODEX_SECURITY_LINEAR_API_KEY=YOUR_LINEAR_PERSONAL_API_KEY
npx @openai/codex-security publish scan /path/to/completed-scan \
  --to linear \
  --linear-team TEAM_ID \
  --linear-assignee teammate@example.com
```

命令行值会覆盖其匹配的环境变量。对于 API 密钥，优先选择 `CODEX_SECURITY_LINEAR_API_KEY` 而不是 `--linear-api-key`，因为命令行参数可能出现在 shell 历史记录和进程列表中。

<a id="codex-security-validate-and-codex-security-patch"></a>

## `codex-security validate` 和 `codex-security patch`

检查候选结果是否有效：

```bash
npx @openai/codex-security validate findings.json \
  "Possible SQL injection in src/query.ts:42"
```

使用捆绑的修复技能生成修复：

```bash
npx @openai/codex-security patch findings.json \
  "Missing authorization check in src/routes.ts:18"
```

每个位置参数接受文字文本或文件路径。这些输入使用当前目录。修复后或以后的扫描不再报告问题时，使用 `validate` 重新检查结果。仅比较扫描并不能证明修复有效。

使用 `--effort` 为任一命令选择推理工作：

```bash
npx @openai/codex-security validate "Possible SQL injection" --effort high
```

<a id="patch-findings-after-a-scan"></a>

### 扫描后修补结果

使用 `scan --patch` 修复完整扫描后的结果。这需要 `@openai/codex-security` 0.1.15 或更高版本。默认严重性阈值是 `low`。此命令选择重要且关键的发现：

```bash
npx @openai/codex-security scan . --patch --patch-severity high --json
```

已验证且已修复的结果不会触发 `--fail-on-severity`。

<a id="patch-saved-findings"></a>

### 修补保存的结果

传递结果或事件 ID 以修补其原始仓库，或从保存的扫描中选择结果：

```bash
npx @openai/codex-security patch OCCURRENCE_ID
npx @openai/codex-security patch --scan SCAN_ID --severity high --json
npx @openai/codex-security patch --scan latest --severity medium
```

`--scan latest` 选择当前仓库最新完成的扫描。保存查找命令支持`--json`；文字文本和文件输入则不然。

添加 `--create-pr` 以仅提交经过验证的补丁文件并使用 GitHub CLI 打开拉取请求：

```bash
npx @openai/codex-security patch --scan SCAN_ID --severity high --create-pr
```

如果推送或拉取请求失败，请从同一仓库运行打印的 `patch --resume-pr BRANCH` 命令来重试。

<a id="patch-linear-issues"></a>

### 修补线性问题

设置 `CODEX_SECURITY_LINEAR_API_KEY` 或 `LINEAR_API_KEY` 作为个人 API 密钥，或设置 `LINEAR_ACCESS_TOKEN` 作为 OAuth 令牌。最好使用环境变量而不是 `--linear-api-key KEY`，以将密钥保留在 shell 历史记录之外。

通过 ID 或 URL 导入问题。重复 `--linear-issue` 以选择多个问题：

```bash
npx @openai/codex-security patch --linear-issue SEC-123 --linear-issue SEC-124
```

使用 `--linear-project` 选择项目的未解决问题。添加 `--linear-filter` 以缩小选择范围：

```bash
npx @openai/codex-security patch --linear-project "Security backlog" \
  --linear-filter '{"labels":{"name":{"eq":"security"}}}'
```

CLI 会排除已完成和已取消的问题，除非过滤器设置 `state`。它不会改变线性问题。

<a id="codex-security-login-logout-and-info"></a>

## `codex-security login`、`logout` 和 `info`

交互式登录：

```bash
npx @openai/codex-security login
```

在远程或无头计算机上使用设备身份验证：

```bash
npx @openai/codex-security login --device-auth
```

检查当前登录情况：

```bash
npx @openai/codex-security login status
```

删除存储的登录信息：

```bash
npx @openai/codex-security logout
```

通过将 API 密钥传递到 stdin 来存储它：

```bash
printenv OPENAI_API_KEY | npx @openai/codex-security login --with-api-key
```

存储企业访问令牌：

```bash
printenv CODEX_ACCESS_TOKEN | npx @openai/codex-security login --with-access-token
```

检查只读 SDK 和捆绑插件元数据：

```bash
npx @openai/codex-security info --json
```

当您将 CLI 公开为 MCP 服务器时，`info` 是唯一可用的命令。扫描、导出、发布、登录、验证和修补仍然仅通过 CLI 进行。

<a id="read-scan-output"></a>

## 读取扫描输出

默认情况下，扫描会将进度、完成摘要和错误发送到 stderr，而不将完整的扫描结果写入 stdout。请求 `--json`、`--format` 或 `--full-output` 将结构化扫描结果发送到 stdout。

交互式终端显示实时仪表板，其中包含当前扫描阶段、审查的文件、活动、令牌使用情况和估计成本。 CI 和重定向输出使用纯文本进度。添加 `--headless` 以在交互式终端中使用纯文本进度：

```bash
npx @openai/codex-security scan . --headless
```

仪表板还显示实时会话详细信息。它们未经编辑，可以包含源代码或凭据。在分享之前先回顾一下它们。

<a id="verbose-diagnostics"></a>

### 详细诊断

添加 `--verbose` 以将编辑后的生命周期、身份验证、进度和成本诊断打印到 stderr：

```bash
npx @openai/codex-security scan . --verbose
```

设置 `CODEX_SECURITY_LOG_LEVEL=debug` 以启用相同的诊断而无需该标志。当 `CODEX_SECURITY_LOG_LEVEL` 未设置时，`LOG_LEVEL=debug` 也会启用诊断。

<a id="completion-summary"></a>

### 完成总结

完成的扫描会将其打开的仓库发现计数、严重性细分、覆盖范围、经过的时间、报告路径和结果目录写入 stderr。它包括Token使用情况和预计成本（如果可用）：

```text
  REPORT    /path/to/scan/report.md

  结果 4（3 项确认了本次扫描；1 项之前发现；1 项严重，2 项严重，1 项提供信息）
  覆盖范围完整
  ELAPSED   1s
  TOKENS 1,250 个输入，200 个缓存，30 个输出
  RESULTS   /path/to/scan
```

信息调查结果计入汇总总数。严重性策略仅评估当前扫描中的 `critical`、`high`、`medium` 和 `low` 结果，而不评估仓库总数中显示的早期结果。

<a id="json-output"></a>

### JSON 输出

`scan --json` 将一份完整的 JSON 文档写入标准输出。它的顶层形状是：

```text
manifest
repositoryFindings
findings
coverage
scanDir
threadId
reportPath
artifactsDir
sarifPath
cost
turn
  id
  status
  durationMs
  finalResponse
  usage
```

当 [修补](#patch-findings-after-a-scan) 时，JSON 输出还包括补丁结果和任何创建的拉取请求。

进度、完成摘要、存档通知和错误保留在 stderr 上。当严重性策略返回退出 `1` 或不完整覆盖返回退出 `2` 时，已完成的扫描仍会打印完整的 JSON 结果。

`codex-security scan --json` 发出一份 JSON 文档。 `codex exec --json` 发出 JSON Lines 事件流。使用与您运行的命令匹配的输出格式。

<a id="scan-artifacts"></a>

## 扫描伪影

完整的扫描将可读报告和结构化工件放在一起：

```text
<scan-directory>/
├── scan-manifest.json
├── findings.json
├── coverage.json
├── report.md
├── artifacts/
└── exports/
    └── results.sarif       # when produced
```

结构化文件服务于不同的工作：

| 文件 | 内容 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `scan-manifest.json` | 扫描身份、状态、目标、范围、生产者和密封工件记录。                                                    |
| `findings.json` | 查找标识符、严重性、置信度、分类、位置、证据、验证、数据流、可达性和补救措施。 |
| `coverage.json` | 审查了使用界面、排除、推迟的工作、未解决的问题和覆盖范围的完整性。                                        |
| `report.md` | 可读扫描报告。                                                                                                           |
| `artifacts/` | 支持扫描工件。                                                                                                      |
| `exports/results.sarif` | 扫描期间生成的 SARIF（如果存在）。                                                                                  |

覆盖完整性具有三个值：

- `complete`：扫描记录其选定范围的完整覆盖范围。
- `partial`：扫描记录延期工作或其他覆盖范围限制。
- `unknown`：扫描报告覆盖完整性未知。

在使用覆盖范围作为安全决策的证据之前，请检查延迟的使用界面、明确的排除和悬而未决的问题。

<a id="exit-codes-and-signals"></a>

## 退出代码和信号

CLI 使用以下退出代码：

| 退出 | 条件 |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `0` | 扫描完成且覆盖完整并通过了其严重性策略，批量扫描或发布完成而没有失败，或者另一个命令成功。                  |
| `1` | 已完成的扫描会报告等于或高于配置的严重性的发现结果。                                                                                                       |
| `2` | CLI 发现输入、运行时或导出错误，扫描覆盖范围不完整，批量扫描仓库有错误，或者出版物有一个或多个失败的结果。 |
| `130` | Ctrl-C 中断扫描或发布。                                                                                                                                     |
| `143` | SIGTERM 终止扫描或发布。                                                                                                                                     |

即使没有严重性策略，任何具有 `partial` 或 `unknown` 覆盖范围的扫描都会返回 `2`。当您请求结构化输出时，已完成的扫描和部分发布仍会将可用结果写入标准输出。 CLI 在中断或运行时错误后打印任何部分输出的位置。

<a id="local-scan-permissions"></a>

## 本地扫描权限

CLI 和 SDK 扫描使用您的本地操作系统权限运行。每次扫描都使用 `codex_security_scan` 文件系统配置文件并将 `approvalPolicy` 设置为 `"never"`。该配置文件允许读取本地文件系统并写入工作区根目录和选定的扫描状态目录。扫描不会停止请求交互式批准。

通过 CLI `--codex` 或 SDK `codexOverrides` 提供的设置（包括 `approval_policy`、`sandbox_mode` 和文件系统权限）无法替换或限制这些扫描控件。主机和网络限制仍然适用。

扫描和工作台进程可以继承您的环境，包括不相关的 API 令牌和云凭据。仅扫描您信任且有权评估的仓库，并仅提供扫描所需的凭据。

<a id="authentication-and-prerequisites"></a>

## 身份验证和先决条件

设置 `OPENAI_API_KEY` 或 `CODEX_API_KEY`，使用 `npx @openai/codex-security login` 登录，或使用现有的文件支持的 Codex 登录。对于 OpenRouter 或 Fireworks，设置提供商的 API 密钥并选择模型。对于 Amazon Bedrock，请改用 Bedrock API 密钥或标准 AWS 凭证链。

有关凭证选择，请参阅 [选择扫描认证](#select-scan-authentication)。

对于 CI，请将 API 密钥范围限制在扫描步骤并使用可信工作流程。

CLI 需要 Node.js 22（22.13.0 或更高版本）、24 或 26。扫描、批量扫描、导出、扫描历史记录和保存的结果也需要 Python 3.10 或更高版本。 Python 3.10 还需要 `tomli`。将 `--python` 与 `scan`、`bulk-scan` 或 `export` 一起使用，或为任何 Python 支持的命令设置 `PYTHON`。

继续使用 [CLI 快速入门](../cli.zh-CN.md)、[批量扫描指南](bulk-scans.zh-CN.md)、[CLI 常见问题解答](faq.zh-CN.md)、[CI指南](ci.zh-CN.md) 或 [TypeScript SDK 指南](../sdk.zh-CN.md)。

<a id="plain-text-aliases"></a>

### 纯文本别名

- --输出文件|-