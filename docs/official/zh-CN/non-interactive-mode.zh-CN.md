> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/non-interactive-mode.md)。

<a id="non-interactive-mode"></a>

# 非交互模式

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

非交互式模式允许您从脚本（例如，持续集成 (CI) 作业）运行 Codex，而无需打开交互式 TUI。您可以使用 `codex exec` 调用它。

有关标志级别的详细信息，请参阅 [`codex exec`](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-codex-exec)。

<a id="when-to-use-codex-exec"></a>

## 何时使用 `codex exec`

当您希望 Codex 执行以下操作时，请使用 `codex exec`：

- 作为管道的一部分运行（CI、预合并检查、计划作业）。
- 生成可以通过管道传输到其他工具的输出（例如，生成发行说明或摘要）。
- 自然地适合 CLI 工作流程，将命令输出链接到 Codex 并将 Codex 输出传递给其他工具。
- 使用明确的预设沙箱和审批设置运行。

<a id="basic-usage"></a>

## 基本用法

将任务提示作为单个参数传递：

```bash
codex exec "summarize the repository structure and list the top 5 risky areas"
```

当 `codex exec` 运行时，Codex 将进度流式传输到 `stderr`，并仅将最终智能体消息打印到 `stdout`。这使得重定向或管道传输最终结果变得简单：

```bash
codex exec "generate release notes for the last 10 commits" | tee release-notes.md
```

当您不想将会话部署文件保留到磁盘时，请使用 `--ephemeral`：

```bash
codex exec --ephemeral "triage this repository and suggest next steps"
```

如果标准输入是通过管道传输的，并且您还提供了提示参数，则 Codex 会将提示视为指令，并将管道传输的内容视为附加上下文。

这使您可以使用一个命令生成输入并将其直接传递给 Codex：

```bash
curl -s https://jsonplaceholder.typicode.com/comments \
  | codex exec "format the top 20 items into a markdown table" \
  > table.md
```

有关更高级的 stdin 管道模式，请参阅 [高级标准输入管道](#advanced-stdin-piping)。

<a id="permissions-and-safety"></a>

## 权限和安全

默认情况下，`codex exec` 在只读沙箱中运行。在自动化中，设置工作流程所需的最少权限：

- 允许编辑：`codex exec --sandboxworkspace-write "<task>"`
- 允许更广泛的访问：`codex exec --sandboxanger-full-access ”<task>"`

仅在受控环境（例如，隔离的 CI 运行程序或容器）中使用 `danger-full-access`。

Codex 将 `codex exec --full-auto` 保留为已弃用的兼容性标志并打印警告。首选新脚本中的显式 `--sandbox workspace-write` 标志。

当您需要不加载 `$CODEX_HOME/config.toml` 的运行时，请使用 `--ignore-user-config`；当您需要为受控自动化环境跳过用户和项目 execpolicy `.rules` 文件时，请使用 `--ignore-rules`。

如果您使用 `required = true` 配置已启用的 MCP 服务器并且它无法初始化，则 `codex exec` 将出现错误并退出，而不是在没有该服务器的情况下继续。

<a id="make-output-machine-readable"></a>

## 使输出机器可读

要在脚本中使用 Codex 输出，请使用 JSON Lines 输出：

```bash
codex exec --json "summarize the repo structure" | jq
```

当您启用 `--json` 时，`stdout` 会变成 JSON Lines (JSONL) 流，因此您可以捕获 Codex 在运行时发出的每个事件。事件类型包括`thread.started`、`turn.started`、`turn.completed`、`turn.failed`、`item.*`和`error`。

项目类型包括智能体消息、推理、命令执行、文件更改、MCP 工具调用、Web 搜索和计划更新。

JSON 流示例（每行都是一个 JSON 对象）：

```jsonl
{"type":"thread.started","thread_id":"0199a213-81c0-7800-8aa1-bbab2a035a53"}
{"type":"turn.started"}
{"type":"item.started","item":{"id":"item_1","type":"command_execution","command":"bash -lc ls","status":"in_progress"}}
{"type":"item.completed","item":{"id":"item_3","type":"agent_message","text":"Repo contains docs, sdk, and examples directories."}}
{"type":"turn.completed","usage":{"input_tokens":24763,"cached_input_tokens":24448,"output_tokens":122,"reasoning_output_tokens":0}}
```

如果您只需要最终消息，请使用“-o”将其写入文件<path>`/`--输出最后一条消息<path>`. This writes the final message to the file and still prints it to `stdout` (see [`codex exec`](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-codex-exec) 了解详细信息）。

<a id="create-structured-outputs-with-a-schema"></a>

## 使用架构创建结构化输出

如果下游步骤需要结构化数据，请使用 `--output-schema` 请求符合 JSON 架构的最终响应。这对于需要稳定字段（例如作业摘要、风险报告或发布元数据）的自动化工作流程非常有用。

`schema.json`

```json
{
  "type": "object",
  "properties": {
    "project_name": { "type": "string" },
    "programming_languages": {
      "type": "array",
      "items": { "type": "string" }
    }
  },
  "required": ["project_name", "programming_languages"],
  "additionalProperties": false
}
```

使用该架构运行 Codex 并将最终 JSON 响应写入磁盘：

```bash
codex exec "Extract project metadata" \
  --output-schema ./schema.json \
  -o ./project-metadata.json
```

最终输出示例（标准输出）：

```json
{
  "project_name": "Codex CLI",
  "programming_languages": ["Rust", "TypeScript", "Shell"]
}
```

<a id="authenticate-in-automation"></a>

## 自动化验证

`codex exec` 默认重用保存的 CLI 身份验证。在 CI 中，显式提供凭据是很常见的：

如果您的可信云或 CI 运行时已收到短期工作负载令牌，请使用 [工作负载身份联合](enterprise/workload-identity.zh-CN.md) 而不是存储 OpenAI 凭证。

<a id="use-api-key-auth"></a>

### 使用 API 密钥身份验证

对于 GitHub 操作，请使用 [Codex GitHub 动作](github-action.zh-CN.md) 而不是自行安装和验证 CLI。该操作旨在通过安装 Codex、启动响应 API 代理并使用可配置的安全策略运行 Codex 来减少 API 密钥暴露。

不要在签出或运行仓库控制代码的工作流中将 `OPENAI_API_KEY` 或 `CODEX_API_KEY` 设置为作业级环境变量。构建脚本、测试、依赖生命周期挂钩或同一作业中的受损操作可以读取这些环境变量。

对于其他自动化环境，仅为需要它的 Codex 调用设置 `CODEX_API_KEY`，并确保同一进程环境中没有不受信任的代码运行。

要在单次运行中使用不同的 API 密钥，请内联设置 `CODEX_API_KEY`：

```bash
CODEX_API_KEY=<api-key> codex exec --json "triage open bug reports"
```

您可以将 `CODEX_API_KEY` 与 `codex exec`、`codex review`、TypeScript SDK 和 `codex exec-server --remote` 结合使用。

<ToggleSection title="在 CI/CD 中使用 ChatGPT 管理的身份验证（高级）">
如果您需要使用 Codex 用户帐户而不是 API 密钥运行 CI/CD 作业，例如企业团队对受信任的运行者使用 ChatGPT 管理的 Codex 访问权限或需要 ChatGPT/Codex 速率限制而不是 API 密钥使用的用户，请阅读此内容。

API 密钥是自动化的正确默认值，因为它们更易于配置和轮换。仅当您特别需要以 Codex 帐户运行时才使用此路径。

将 `~/.codex/auth.json` 视为密码：它包含访问令牌。不要提交、粘贴到票证中或在聊天中分享。

请勿将此工作流程用于公共或开源仓库。如果运行程序上没有 `codex login` 选项，请通过安全存储为 `auth.json` 提供种子，在运行程序上运行 Codex，以便 Codex 就地刷新它，并在运行之间保留更新的文件。

参见 [在 CI/CD 中维护 Codex 帐户身份验证（高级）](https://learn.chatgpt.com/docs/auth/ci-cd-auth)。

</ToggleSection>

<a id="resume-a-non-interactive-session"></a>

## 恢复非交互式会话

如果需要继续之前的运行（例如，两阶段管道），请使用 `resume` 子命令：

```bash
codex exec "review the change for race conditions"
codex exec resume --last "fix the race conditions you found"
```

您还可以使用“codex execresume”定位特定会话 ID<SESSION_ID>`.

<a id="git-repository-required"></a>

## 需要 Git 仓库

Codex 需要在 Git 仓库内运行命令以防止破坏性更改。如果您确定环境安全，请使用 `codex exec --skip-git-repo-check` 覆盖此检查。

<a id="common-automation-patterns"></a>

## 常见的自动化模式

<a id="example-autofix-ci-failures-in-github-actions"></a>

### 示例：GitHub 操作中的自动修复 CI 失败

对于 GitHub 操作工作流程，请使用 [`openai/codex-action`](https://github.com/openai/codex-action)，而不是安装 Codex 并将 API 密钥传递给 shell 步骤。该操作启动 OpenAI API 密钥的安全代理。

当 CI 工作流程失败时，您可以使用 Codex 自动提出修复建议。模式是：

1. 当主 CI 工作流程完成但出现错误时，触发后续工作流程。
2. 仅使用仓库读取权限检查失败的提交。
3. 在 Codex 之前运行设置命令，而不会将您的 OpenAI API 密钥暴露给这些步骤。
4. 运行 Codex GitHub 操作。
5. 将 Codex 的本地更改保存为补丁工件。
6. 在单独的作业中，应用补丁并打开拉取请求。

下面的 Codex 作业只有 `contents: read`。 Codex 运行后，它仅将 diff 序列化为工件。 `open_pr` 作业接收仓库写入权限，但不接收 `OPENAI_API_KEY`。

该示例假设有一个 Node.js 项目。调整设置和测试命令以匹配您的堆栈。

有关更深入的安全检查表，请参阅 [Codex GitHub 行动安全指导](https://github.com/openai/codex-action/blob/main/docs/security.md)。

```yaml
name: Codex auto-fix on CI failure

on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]

jobs:
  generate_fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      has_patch: ${{ steps.diff.outputs.has_patch }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0
          persist-credentials: false

      - uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: |
          if [ -f package-lock.json ]; then npm ci; fi

      - name: Run Codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt: |
            The CI workflow "${{ github.event.workflow_run.name }}" failed for commit
            ${{ github.event.workflow_run.head_sha }}.

            Run `npm test --silent` to reproduce the failure. Identify the minimal
            change needed to make the tests pass, implement only that change, and
            run `npm test --silent` again.

            Do not refactor unrelated files.

      - name: Create patch artifact
        id: diff
        run: |
          git add -N .
          git diff --binary HEAD > codex.patch
          if [ -s codex.patch ]; then
            echo "has_patch=true" >> "$GITHUB_OUTPUT"
          else
            echo "has_patch=false" >> "$GITHUB_OUTPUT"
          fi

      - name: Upload patch artifact
        if: steps.diff.outputs.has_patch == 'true'
        uses: actions/upload-artifact@v4
        with:
          name: codex-fix-patch
          path: codex.patch
          if-no-files-found: error

  open_pr:
    runs-on: ubuntu-latest
    needs: generate_fix
    if: needs.generate_fix.outputs.has_patch == 'true'
    permissions:
      contents: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0

      - uses: actions/download-artifact@v4
        with:
          name: codex-fix-patch

      - name: Apply Codex patch
        run: git apply --index codex.patch

      - name: Open pull request
        env:
          GH_TOKEN: ${{ github.token }}
          FAILED_HEAD_BRANCH: ${{ github.event.workflow_run.head_branch }}
          FAILED_HEAD_SHA: ${{ github.event.workflow_run.head_sha }}
          RUN_ID: ${{ github.event.workflow_run.run_id }}
        run: |
          branch="codex/auto-fix-$RUN_ID"

          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git switch -c "$branch"
          git commit -m "Auto-fix failing CI via Codex"
          git push origin "$branch"

          {
            echo "Codex generated this patch after CI failed for \`$FAILED_HEAD_SHA\`."
            echo
            echo "Review the changes before merging."
          } > pr-body.md

          gh pr create \
            --base "$FAILED_HEAD_BRANCH" \
            --head "$branch" \
            --title "Auto-fix failing CI via Codex" \
            --body-file pr-body.md
```

<a id="advanced-stdin-piping"></a>

## 高级标准输入管道

当另一个命令为 Codex 生成输入时，根据指令应来自的位置选择标准输入模式。当您已经知道指令并希望将管道输出作为上下文传递时，请使用提示加标准输入。当 stdin 应成为完整提示符时，请使用 `codex exec -`。

<a id="use-prompt-plus-stdin"></a>

### 使用提示加标准输入

当另一个命令已经生成您想要 Codex 检查的数据时，Prompt-plus-stdin 非常有用。在此模式下，您可以自己编写指令，并将输出作为上下文通过管道传输，这使其非常适合围绕命令输出、日志和生成的数据构建的 CLI 工作流程。

```bash
npm test 2>&1 \
  | codex exec "summarize the failing tests and propose the smallest likely fix" \
  | tee test-summary.md
```

<ToggleSection title="更多提示加标准输入示例">

<a id="summarize-logs"></a>

### 总结日志

```bash
tail -n 200 app.log \
  | codex exec "identify the likely root cause, cite the most important errors, and suggest the next three debugging steps" \
  > log-triage.md
```

<a id="inspect-tls-or-http-issues"></a>

### 检查 TLS 或 HTTP 问题

```bash
curl -vv https://api.example.com/health 2>&1 \
  | codex exec "explain the TLS or HTTP failure and suggest the most likely fix" \
  > tls-debug.md
```

<a id="prepare-a-slack-ready-update"></a>

### 准备 Slack 就绪更新

```bash
gh run view 123456 --log \
  | codex exec "write a concise Slack-ready update on the CI failure, including the likely cause and next step" \
  | pbcopy
```

<a id="draft-a-pull-request-comment-from-ci-logs"></a>

### 从 CI 日志中起草拉取请求评论

```bash
gh run view 123456 --log \
  | codex exec "summarize the failure in 5 bullets for the pull request thread" \
  | gh pr comment 789 --body-file -
```

</ToggleSection>

<a id="use-codex-exec---when-stdin-is-the-prompt"></a>

### 当 stdin 为提示符时使用 `codex exec -`

如果省略提示参数，Codex 从标准输入读取提示。当您想要显式强制该行为时，请使用 `codex exec -`。

当另一个命令或脚本动态生成整个提示时，`-` 哨兵非常有用。当您将提示存储在文件中、使用 shell 脚本组装提示或在将整个提示传递给 Codex 之前将实时命令输出与指令结合起来时，这非常适合。

```bash
cat prompt.txt | codex exec -
```

```bash
printf "Summarize this error log in 3 bullets:\n\n%s\n" "$(tail -n 200 app.log)" \
  | codex exec -
```

```bash
generate_prompt.sh | codex exec - --json > result.jsonl
```