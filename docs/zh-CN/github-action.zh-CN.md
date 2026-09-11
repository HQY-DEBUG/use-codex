> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/github-action.md)。

<a id="codex-github-action"></a>

# GitHub Action

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Codex GitHub 操作 (`openai/codex-action@v1`) 在 CI/CD 作业中运行 Codex、应用补丁或从 GitHub 操作工作流程发布评论。该操作将安装 Codex CLI，在您提供 API 密钥时启动响应 API 代理，并在您指定的权限下运行 `codex exec`。

当您想要执行以下操作时，请采取行动：

- 自动执行 Codex 对拉取请求或发布的反馈，而无需亲自管理 CLI。
- 作为 CI 管道的一部分，Codex 驱动的质量检查的门发生变化。
- 从工作流程文件运行可重复的 Codex 任务（代码审查、发布准备、迁移）。

有关 CI 示例，请参阅 [非交互模式](non-interactive-mode.zh-CN.md) 并探索 [openai/codex-action 仓库](https://github.com/openai/codex-action) 中的源代码。

<a id="prerequisites"></a>

## 先决条件

- 将您的 OpenAI 密钥存储为 GitHub 密钥（例如 `OPENAI_API_KEY`）并在工作流程中引用它。
- 在 Linux 或 macOS 运行器上运行作业。对于 Windows，设置 `safety-strategy: unsafe`。
- 在调用操作之前检查您的代码，以便 Codex 可以读取仓库内容。
- 决定您要运行哪个提示。您可以通过 `prompt` 提供内联文本，或使用 `prompt-file` 指向仓库中提交的文件。

<a id="example-workflow"></a>

## 工作流程示例

下面的示例工作流程审查新的拉取请求，捕获 Codex 的响应，并将其发布回 PR。

```yaml
name: Codex pull request review
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  codex:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      final_message: ${{ steps.run_codex.outputs.final-message }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: refs/pull/${{ github.event.pull_request.number }}/merge
          fetch-depth: 0
          persist-credentials: false

      - name: Run Codex
        id: run_codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt-file: .github/codex/prompts/review.md
          output-file: codex-output.md

  post_feedback:
    runs-on: ubuntu-latest
    needs: codex
    if: needs.codex.outputs.final_message != ''
    permissions:
      issues: write
      pull-requests: write
    steps:
      - name: Post Codex feedback
        uses: actions/github-script@v7
        with:
          github-token: ${{ github.token }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number,
              body: process.env.CODEX_FINAL_MESSAGE,
            });
        env:
          CODEX_FINAL_MESSAGE: ${{ needs.codex.outputs.final_message }}
```

将 `.github/codex/prompts/review.md` 替换为您自己的提示文件，或使用 `prompt` 输入作为内联文本。该示例还将最终的 Codex 消息写入 `codex-output.md` 以供以后检查或工件上传。

<a id="configure-codex-exec"></a>

## 配置`codex exec`

通过设置映射到 `codex exec` 选项的操作输入来微调 Codex 的运行方式：

- `prompt` 或 `prompt-file`（选择一项）：内联指令或 Markdown 的仓库路径或任务文本。考虑将提示存储在 `.github/codex/prompts/` 中。
- `codex-args`：额外的 CLI 标志。提供 JSON 数组（例如 `["--ephemeral"]`）或 shell 字符串 (`--profile ci`) 以配置会话、配置文件或 MCP 设置。
- `model` 和 `effort`：选择您想要的 Codex 智能体配置；默认值留空。
- `sandbox`：将沙箱模式（`workspace-write`、`read-only`、`danger-full-access`）与Codex运行期间所需的权限相匹配。
- `output-file`：将最终的 Codex 消息保存到磁盘，以便后续步骤可以上传或比较它。
- `codex-version`：固定特定的 CLI 版本。留空以使用最新发布的版本。
- `codex-home`：如果要跨步骤重复使用配置文件或 MCP 设置，请指向共享的 Codex 主目录。

<a id="manage-privileges"></a>

## 管理权限

Codex 对 GitHub 托管的运行器具有广泛的访问权限，除非您对其进行限制。使用这些输入来控制曝光：

- `safety-strategy`（默认`drop-sudo`）在运行Codex之前删除`sudo`。这对于工作来说是不可逆转的，并且可以保护内存中的秘密。在 Windows 上，您必须设置 `safety-strategy: unsafe`。
- `unprivileged-user` 将 `safety-strategy: unprivileged-user` 与 `codex-user` 配对，将 Codex 作为特定帐户运行。确保用户可以读取和写入仓库签出（有关所有权修复，请参阅 [`unprivileged-user` 示例](https://github.com/openai/codex-action/blob/main/examples/unprivileged-user.yml)）。
- `read-only` 阻止 Codex 更改文件或使用网络，但它仍然以提升的权限运行。不要仅依靠 `read-only` 来保护机密。
- `sandbox` 限制 Codex 本身内的文件系统和网络访问。选择仍能让任务完成的最窄选项。
- `allow-users` 和 `allow-bots` 限制谁可以触发工作流。默认情况下，只有具有写入权限的用户才能运行该操作；明确列出额外的受信任帐户或将该字段留空以实现默认行为。

<a id="capture-outputs"></a>

## 捕获输出

该操作通过 `final-message` 输出发出最后一条 Codex 消息。将其映射到作业输出（如上所示）或在后续步骤中直接处理它。如果您希望从跑步者那里收集完整的成绩单，请将 `output-file` 与上传的工件功能结合起来。当您需要结构化数据时，请通过 `--output-schema` 传递 `codex-args` 以强制执行 JSON 形状。

<a id="security-checklist"></a>

## 安全检查表

- 限制谁可以启动工作流程。优先选择受信任的事件或明确的批准，而不是允许每个人针对您的仓库运行 Codex。
- 清理来自拉取请求、提交消息或问题正文的提示输入，以避免提示注入。在将 HTML 注释或隐藏文本输入到 Codex 之前，请先检查其内容。
- 通过将 `safety-strategy` 保留在 `drop-sudo` 上或将 Codex 移至非特权用户来保护您的 `OPENAI_API_KEY`。切勿在多租户运行器上将操作保留在 `unsafe` 模式下。
- 将 Codex 作为作业的最后一步运行，以便后续步骤不会继承任何意外的状态更改。
- 如果您怀疑代理日志或操作输出暴露了秘密材料，请立即轮换密钥。

<a id="troubleshooting"></a>

## 故障排除

- **您设置提示和提示文件**：删除重复的输入，以便您只提供一个源。
- **response-api-proxy 未写入服务器信息**：确认API密钥存在且有效；仅当您提供 `openai-api-key` 时，代理才会启动。
- **预期删除 `sudo`，但 `sudo` 成功**：确保之前的步骤没有恢复 `sudo`，并且运行程序操作系统是 Linux 或 macOS。使用新作业重新运行。
- **`drop-sudo`之后的权限错误**：在操作运行之前授予写入访问权限（例如使用 `chmod -R g+rwX "$GITHUB_WORKSPACE"` 或使用非特权用户模式）。
- **未经授权的触发被阻止**：如果需要允许默认写入协作者之外的服务帐户，请调整 `allow-users` 或 `allow-bots` 输入。