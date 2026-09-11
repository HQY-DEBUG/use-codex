> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/cli/ci.md)。

<a id="run-codex-security-in-ci"></a>

# 在 CI 中运行安全扫描

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

在 CI 中运行 Codex Security CLI 以查看拉取请求或合并请求中的确切更改，保留结果和覆盖范围，并可选择在选定的严重性下使检查失败。从咨询结果开始，检查扫描质量和运行时间，然后添加适合您的仓库的严重性策略。

安装公共 `@openai/codex-security` 包。运行扫描仍然需要 Codex Security 访问权限。

本指南包括 GitHub 操作和 GitLab CI/CD 的示例。相同的扫描和导出命令适用于其他 CI 系统。

<a id="prepare-the-workflow"></a>

## 准备工作流程

将 OpenAI API 密钥存储在 CI 提供商的秘密存储中，名称为 `CODEX_SECURITY_API_KEY`。

将此秘密直接映射到扫描步骤的 `OPENAI_API_KEY` 环境变量。将凭证范围保持在扫描进程范围内，并使用 `--auth api-key` 显式选择它。

仅针对您信任的仓库和拉取请求运行工作流程。扫描使用运行者的本地权限，并且不会暂停以等待批准。扫描进程可以继承作业环境，因此请将不相关的令牌和云凭据排除在外。

跑步者需要：

- Node.js 22（22.13.0 或更高版本）、24 或 26。
- Python 3.10 或更高版本。
- 已发布的 `@openai/codex-security` 包，安装在仓库签出之外。
- 拉取请求或合并请求头和基础历史记录，以便 Git 可以计算合并基础。

<a id="add-the-github-actions-workflow"></a>

## 添加 GitHub 操作工作流程

对于私人或内部仓库，请在上传 SARIF 之前启用 [GitHub 代码安全](https://docs.github.com/en/code-security/code-scanning/integrating-with-code-scanning/uploading-a-sarif-file-to-github)。

创建 `.github/workflows/codex-security.yml`。在检查拉取请求之前，请在 `$RUNNER_TEMP/codex-security` 下安装 `@openai/codex-security`，以便可信可执行文件在 `$RUNNER_TEMP/codex-security/node_modules/.bin/codex-security` 上可用：

```yaml
name: Codex Security scan

on:
  pull_request:

jobs:
  codex-security:
    if: github.event.pull_request.head.repo.full_name == github.repository && github.actor != 'dependabot[bot]'
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write
    steps:
      - name: Set up Node.js
        uses: actions/setup-node@820762786026740c76f36085b0efc47a31fe5020 # v7
        with:
          node-version: "26"

      - name: Set up Python
        uses: actions/setup-python@5fda3b95a4ea91299a34e894583c3862153e4b97 # v7
        with:
          python-version: "3.14"

      - name: Install Codex Security
        run: |
          set -euo pipefail
          npm install \
            --prefix "$RUNNER_TEMP/codex-security" \
            --ignore-scripts \
            --no-audit \
            --no-fund \
            @openai/codex-security

      - name: Verify Codex Security
        env:
          CODEX_SECURITY_BIN: ${{ runner.temp }}/codex-security/node_modules/.bin/codex-security
        run: |
          set -euo pipefail
          test -x "$CODEX_SECURITY_BIN"
          "$CODEX_SECURITY_BIN" --version

      - name: Check out the pull request
        uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7
        with:
          ref: ${{ github.event.pull_request.head.sha }}
          fetch-depth: 0
          persist-credentials: false

      - name: Scan the pull request
        env:
          OPENAI_API_KEY: ${{ secrets.CODEX_SECURITY_API_KEY }}
          CODEX_SECURITY_BIN: ${{ runner.temp }}/codex-security/node_modules/.bin/codex-security
          CODEX_SECURITY_STATE_DIR: ${{ runner.temp }}/codex-security-state
          BASE_SHA: ${{ github.event.pull_request.base.sha }}
          HEAD_SHA: ${{ github.event.pull_request.head.sha }}
          SCAN_DIR: ${{ runner.temp }}/codex-security-results
        run: |
          set -euo pipefail
          BASE_REVISION="$(git merge-base "$BASE_SHA" "$HEAD_SHA")"
          "$CODEX_SECURITY_BIN" scan . \
            --diff "$BASE_REVISION" \
            --head "$HEAD_SHA" \
            --auth api-key \
            --output-dir "$SCAN_DIR" \
            --json > "$RUNNER_TEMP/codex-security.json"

      - name: Export SARIF
        id: export-sarif
        if: always()
        env:
          CODEX_SECURITY_BIN: ${{ runner.temp }}/codex-security/node_modules/.bin/codex-security
          SCAN_DIR: ${{ runner.temp }}/codex-security-results
          SARIF_FILE: ${{ runner.temp }}/codex-security.sarif
        run: |
          set -euo pipefail
          if test -f "$SCAN_DIR/scan-manifest.json"; then
            "$CODEX_SECURITY_BIN" export "$SCAN_DIR" \
              --export-format sarif \
              --source-root "$GITHUB_WORKSPACE" \
              --output "$SARIF_FILE"
            echo "available=true" >> "$GITHUB_OUTPUT"
          fi

      - name: Upload SARIF
        if: always() && steps.export-sarif.outputs.available == 'true'
        uses: github/codeql-action/upload-sarif@e4fba868fa4b1b91e1fdab776edc8cfbe6e9fb81 # v4
        with:
          sarif_file: ${{ runner.temp }}/codex-security.sarif
          ref: refs/pull/${{ github.event.pull_request.number }}/head
          sha: ${{ github.event.pull_request.head.sha }}
          category: codex-security

      - name: Preserve scan results
        if: always()
        uses: actions/upload-artifact@043fb46d1a93c77aae656e7c1c64a875d1fc6a0a # v7
        with:
          name: codex-security-results
          path: |
            ${{ runner.temp }}/codex-security-results
            ${{ runner.temp }}/codex-security.json
          if-no-files-found: warn
          retention-days: 7
```

工作流程检查拉取请求头，计算其合并基础，并扫描这些修订之间已提交的更改。完整的历史记录使目标保持准确。 `persist-credentials: false` 将仓库令牌保留在签出的 Git 配置之外。在签出之前安装 CLI 并运行其绝对路径可以使仓库控制的可执行文件远离扫描凭据。 `--auth api-key` 显式选择范围 API 密钥。扫描将其历史记录保存在仓库外部的可写状态目录中。

`--json` 将一份完整的 JSON 文档写入 stdout，因此工作流可以直接保存它。进度、完成摘要和错误保留在 stderr 上。这与 `codex exec --json` 不同，后者发出 JSON Lines 事件流。

导出步骤读取完整的密封扫描并写入 SARIF。它使 Codex 运行时和凭证保持不变。扫描工件可能包含易受攻击的源代码片段、证据和补救详细信息。选择适合您的仓库的访问控制和较短的保留窗口。

<a id="add-the-gitlab-cicd-pipeline"></a>

## 添加 GitLab CI/CD 管道

对于具有受保护的默认分支扫描、选择加入计划深度扫描、单独的 SARIF 策略门控以及可选的已验证草稿合并请求的生产工作流程，请使用 [在 GitLab CI/CD 中运行 Codex Security](ci/gitlab.zh-CN.md)。

GitLab 可以在 GitLab Ultimate 19.2 或更高版本上摄取 [SARIF 2.1.0 报告](https://docs.gitlab.com/ci/yaml/artifacts_reports/#artifactsreportssarif)。在运行管道之前添加屏蔽和隐藏的 `CODEX_SECURITY_API_KEY` CI/CD 变量。

以下最小示例将仅扫描 `security` 作业添加到根 `.gitlab-ci.yml`。将所有现有阶段和作业保留在文件中。它默认扫描合并请求更改。将 `CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH` 设置为 `"true"` 也可以扫描完整的默认分支：

```yaml
variables:
  CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH: "false"

stages:
  - test
  - security

codex-security:
  stage: security
  image: node:26-bookworm-slim
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_SOURCE_PROJECT_ID == $CI_PROJECT_ID'
      variables:
        CODEX_SECURITY_SCAN_SCOPE: "diff"
    - if: '$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH && $CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH == "true"'
      variables:
        CODEX_SECURITY_SCAN_SCOPE: "full"
  variables:
    GIT_DEPTH: "0"
    CODEX_SECURITY_CLI_DIR: "/tmp/codex-security-cli"
  before_script:
    - |
      set -eu
      apt-get update -qq
      apt-get install -y -qq --no-install-recommends \
        ca-certificates \
        git \
        python3 \
        ripgrep
      npm install \
        --prefix "$CODEX_SECURITY_CLI_DIR" \
        --ignore-scripts \
        --no-audit \
        --no-fund \
        @openai/codex-security@0.1.20
      export CODEX_SECURITY_BIN="$CODEX_SECURITY_CLI_DIR/node_modules/.bin/codex-security"
      test -x "$CODEX_SECURITY_BIN"
      "$CODEX_SECURITY_BIN" --version
  script:
    - |
      set -eu
      if test -z "${CODEX_SECURITY_API_KEY:-}"; then
        echo "Set the CODEX_SECURITY_API_KEY CI/CD variable." >&2
        exit 2
      fi

      codex_security_api_key="$CODEX_SECURITY_API_KEY"
      unset CODEX_SECURITY_API_KEY

      case "${CODEX_SECURITY_SCAN_SCOPE:-}" in
        diff)
          BASE_SHA="$CI_MERGE_REQUEST_DIFF_BASE_SHA"
          HEAD_SHA="$CI_COMMIT_SHA"
          BASE_REVISION="$(git merge-base "$BASE_SHA" "$HEAD_SHA")"
          set -- --diff "$BASE_REVISION" --head "$HEAD_SHA"
          echo "Scanning committed changes from $BASE_REVISION to $HEAD_SHA."
          ;;
        full)
          set -- --mode standard
          echo "Scanning the complete default branch at $CI_COMMIT_SHA."
          ;;
        *)
          echo "Unsupported Codex Security scan scope: ${CODEX_SECURITY_SCAN_SCOPE:-unset}" >&2
          exit 2
          ;;
      esac

      export CODEX_SECURITY_STATE_DIR="/tmp/codex-security-state-$CI_JOB_ID"
      SCAN_DIR="/tmp/codex-security-results-$CI_JOB_ID"
      JSON_FILE="/tmp/codex-security-$CI_JOB_ID.json"
      SARIF_FILE="/tmp/codex-security-$CI_JOB_ID.sarif"

      install -d -m 700 "$CODEX_SECURITY_STATE_DIR" "$SCAN_DIR"

      set +e
      OPENAI_API_KEY="$codex_security_api_key" \
        "$CODEX_SECURITY_BIN" scan . \
          "$@" \
          --auth api-key \
          --output-dir "$SCAN_DIR" \
          --json > "$JSON_FILE"
      scan_exit="$?"
      set -e
      unset codex_security_api_key

      install -d -m 700 codex-security-artifacts/results
      cp -R "$SCAN_DIR"/. codex-security-artifacts/results/
      if test -s "$JSON_FILE"; then
        cp "$JSON_FILE" codex-security-artifacts/codex-security.json
      fi
      printf '%s\n' "$scan_exit" > codex-security-artifacts/scan-exit-code.txt

      export_exit=0
      if test -f "$SCAN_DIR/scan-manifest.json"; then
        set +e
        "$CODEX_SECURITY_BIN" export "$SCAN_DIR" \
          --export-format sarif \
          --source-root "$CI_PROJECT_DIR" \
          --output "$SARIF_FILE"
        export_exit="$?"
        set -e
        if test -s "$SARIF_FILE"; then
          cp "$SARIF_FILE" codex-security-artifacts/codex-security.sarif
        fi
      fi

      if test "$scan_exit" -ne 0; then
        exit "$scan_exit"
      fi
      exit "$export_exit"
  artifacts:
    when: always
    access: maintainer
    expire_in: 7 days
    paths:
      - codex-security-artifacts/
    reports:
      sarif: codex-security-artifacts/codex-security.sarif
```

默认情况下，该作业仅针对来自同一项目中分支的合并请求运行，因此分支管道不会接收扫描凭据。在组、项目或管道级别将 `CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH` 设置为 `"true"`，以便在默认分支上运行标准完整扫描。与差异扫描相比，完整扫描需要更长的时间且成本更高。

`GIT_DEPTH: "0"` 提供从 `CI_MERGE_REQUEST_DIFF_BASE_SHA` 和 `CI_COMMIT_SHA` 计算合并基数以进行合并请求扫描所需的历史记录。

该作业在 `/tmp` 下安装 CLI，通过绝对路径运行它，并仅向扫描进程公开 API 密钥。当扫描失败时，`artifacts: when: always` 会保留 SARIF 报告，而 `artifacts:access: maintainer` 会限制对详细扫描结果的访问。

对 `.gitlab-ci.yml` 的更改可能会暴露 CI/CD 变量，因此请在运行作业之前检查管道更改。如果您使用 [保护`CODEX_SECURITY_API_KEY`](https://docs.gitlab.com/ci/pipelines/merge_request_pipelines/#control-access-to-protected-variables-and-runners)，则 GitLab 使其仅适用于受保护分支之间的同一项目合并请求，并且仅当用户可以访问目标分支时可用。

专用的 GitLab 指南将这个最小的工作扩展到本节开头链接的生产工作流程中。

<a id="choose-a-severity-policy"></a>

## 选择严重性策略

这两个示例都是仅报告的，因为它们省略了 `--fail-on-severity`。一旦您准备好让调查结果影响检查，请向扫描命令添加阈值：

```bash
"$CODEX_SECURITY_BIN" scan . \
  --diff origin/main \
  --output-dir /path/outside/repository/results \
  --fail-on-severity high
```

支持的阈值包括 `critical`、`high`、`medium` 和 `low`。阈值包括当前扫描在该严重程度及以上的结果。仓库摘要中显示的早期公开调查结果不会影响该政策。

扫描步骤使用这些退出代码：

| 退出 | 含义 |
| ----- | --------------------------------------------------------------------------------------- |
| `0` | 扫描已完成并完全覆盖，并且所有配置的策略均已通过。            |
| `1` | 已完成的扫描包含等于或高于阈值的结果。                        |
| `2` | CLI 发现输入或运行时错误，或者已完成的扫描覆盖不完整。 |
| `130` | Ctrl-C 中断扫描。                                                            |
| `143` | SIGTERM 终止扫描。                                                            |

即使没有严重性策略，具有 `partial` 或 `unknown` 覆盖范围的扫描也会返回 `2`。 CLI 仍然会记录其可用的调查结果和覆盖范围。在将检查视为结论性检查之前，请检查 `coverage.json` 中的递延区域。

<a id="retry-with-an-existing-result-directory"></a>

## 使用现有结果目录重试

为每个 CI 作业使用新的运行程序目录。对于持久或自托管运行器，请使用 `--archive-existing` 保留早期结果：

```bash
"$CODEX_SECURITY_BIN" scan . \
  --diff origin/main \
  --output-dir /path/outside/repository/results \
  --archive-existing
```

该命令存档早期的结果并从空扫描目录开始。

<a id="troubleshoot-a-ci-scan"></a>

## CI 扫描故障排除

- **未知的 Git 引用或意外的差异：** 获取基础和头部历史记录，计算合并基础，并显式传递两个修订。
- **受保护或非空输出目录：** 选择封闭的 Git 工作树之外的私有目录。当目录已包含结果时使用 `--archive-existing`。
- **缺少凭据：** 确认 `CODEX_SECURITY_API_KEY` 可用于受信任的工作流或管道，并直接映射到扫描进程的 `OPENAI_API_KEY` 环境变量。
- **扫描历史记录错误：** 将 `CODEX_SECURITY_STATE_DIR` 设置为仓库外部的可写目录。
- **Python设置错误：** 确认运行器使用Python 3.10或更高版本。
- **覆盖不完整：** 审查 `coverage.json`，包括延迟使用界面和开放问题，然后使用适当的目标或环境重新运行。
- **SARIF 导出错误：** 确认扫描已完成且完整扫描目录可用。导出在写入 SARIF 之前验证密封的工件。
- **SARIF 上传错误：** 对于 GitHub 操作，请确认您的组织已为仓库启用 GitHub 代码安全性，并且工作流授予 `actions: read`、`contents: read` 和 `security-events: write`。对于 GitLab CI/CD，确认项目使用 GitLab Ultimate 19.2 或更高版本，并且作业通过 `artifacts:reports:sarif` 上传 SARIF 2.1.0 文件。

对于每个命令、标志、工件和输出字段，请参阅 [CLI 参考](reference.zh-CN.md)。有关基于插件的交互式 CI 审核，请参阅 [检查代码更改以确保安全](../plugin/code-changes.zh-CN.md#automate-reviews-in-cicd)。