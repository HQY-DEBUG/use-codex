> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/plugin/code-changes.md)。

<a id="review-code-changes-for-security"></a>

# 代码变更安全审查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

运行安全变更审查以查找 Git 支持的变更集中的回归。 Codex 审查每个更改的类源文件及其直接支持代码。它不会将审查扩展到完整的仓库审核。

要扫描整个仓库而不是特定更改，请参阅 [运行安全扫描](scans.zh-CN.md)。

<a id="run-a-manual-review"></a>

## 运行手动审核

在桌面应用程序中，打开 **安全性**，选择 **扫描**，然后选择 **+ 扫描**。选择仓库，然后选择 **变化**。查看未提交的更改、单个提交或基础和头部修订。 **深度扫描** 不可用于更改扫描。

您还可以要求 Codex 检查对话中未提交的更改：

```text
使用 $codex-security:security-diff-scan 查看我当前未提交的安全回归更改。
```

对于提交或分支范围，请在需要时指定两个修订：

```text
使用 $codex-security:security-diff-scan 查看从 origin/main 到 HEAD 的更改以进行安全回归。重点关注身份验证、授权、输入处理、文件系统访问、网络请求和机密。
```

当本地结帐中提供了基础版本和头部版本时，您还可以命名拉取请求。

<a id="confirm-the-change-in-setup"></a>

## 确认设置更改

<WorkflowSteps>

1. 选择**变化**。
2. 确认已签出的仓库、当前分支和最新提交。
3. 在 **审查变更** 下，选择：
   - `Uncommitted changes` 表示当前工作树。
   - 单次提交审核的最新提交。
   - 分支或拉取请求范围的基础和头部修订。
4. 确认摘要描述了您想要查看的更改。
5. 选择**开始扫描**。

</WorkflowSteps>

Codex 不会检查另一个分支或切换选定的工作树。如果请求的修订在本地不可用，请在审阅之前获取它或提供本地可用的基础和头部。

<a id="act-on-findings"></a>

## 根据调查结果采取行动

查看结果后，[修复并验证已接受的发现](fix-findings.zh-CN.md) 或 [导出并跟踪结果](export-findings.zh-CN.md)。

<a id="automate-reviews-in-cicd"></a>

## 在 CI/CD 中自动进行审核

如果您有权访问测试版独立 CLI，请参阅 [在 CI 中运行 Codex Security](../cli/ci.zh-CN.md) 了解结构化 JSON、严重性策略和 SARIF 上传。继续本节，通过 `codex exec` 调用已安装的插件技能。

当运行者无需交互即可调用 Codex CLI 时，在 CI 中运行 `$codex-security:security-diff-scan`。首先，在不暴露扫描凭据的情况下安装 CLI：

```bash
npm install --global @openai/codex
```

在 CLI 中安装 Codex Security 插件：

```bash
codex plugin add codex-security@openai-curated
```

安装命令使用公共 Codex CLI 插件市场。在依赖 CI 中的特定插件版本或功能之前，请检查 [插件变更日志](changelog.zh-CN.md)。

接下来，从 CI 秘密存储中提供 OpenAI API 密钥 `CODEX_SECURITY_API_KEY`。仅公开扫描的凭据：

```bash
CODEX_API_KEY="$CODEX_SECURITY_API_KEY" codex exec \
  --sandbox workspace-write \
  "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."
```

可写沙箱允许扫描创建临时工件。提示仍然要求Codex保持源签出不变。

扫描将其输出写入“$TMPDIR/codex-security-scans/”<repository>/<scan-id>/`:

| 文件 | 内容 |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `report.md` | 完整扫描目录的主要可读入口点。                                                                                              |
| `调查结果/<slug>/` | 根据要求提供详细的漏洞报告和支持概念验证文件。                                                                     |
| `hardening/` | 结构加固指南和支持建议（根据要求）。                                                                                   |
| `findings.json` | 具有稳定标识符、严重性、置信度、源位置和补救措施的结果。提供经批准的内部安全工作流程或下游工具。 |
| `scan-manifest.json` | 密封的扫描收据，其中包含已审核的目标、修订和工件哈希值。                                                                             |
| `coverage.json` | 审查和推迟使用界面、排除和覆盖完整性。                                                                                    |

[`findings.json` 架构](https://github.com/openai/plugins/blob/main/plugins/codex-security/schemas/findings.schema.json) 定义了完整的结构。该架构包括以下字段：

| 字段 | 类型 | 描述 |
| ------------------------- | ------ | ---------------------------------------------------------------------- |
| `documentType` | 字符串 | 将文档标识为 `codex-security.findings`。                  |
| `schemaVersion` | 字符串 | 标识结果架构版本。                                |
| `scanId` | 字符串 | 标识产生结果的扫描。                        |
| `findings` | 数组 | 包含零个或多个查找对象。                                 |
| `findings[].findingId` | 字符串 | 从发现指纹派生的稳定发现标识符。        |
| `findings[].occurrenceId` | 字符串 | 标识特定扫描中发现的情况。          |
| `findings[].ruleId` | 字符串 | 标识漏洞系列。                                   |
| `findings[].identity` | 对象 | 包含语义锚点和可选的同级实例标识符。 |
| `findings[].fingerprints` | 对象 | 包含指纹算法和主指纹。            |
| `findings[].title` | 字符串 | 提供空头调查标题。                                      |
| `findings[].summary` | 字符串 | 总结漏洞及其影响。                           |
| `findings[].severity` | 对象 | 包含严重性级别和可选评分详细信息。              |
| `findings[].confidence` | 对象 | 包含置信水平和基本原理。                           |
| `findings[].taxonomy` | 对象 | 包含漏洞类别和 CWE 标识符。               |
| `findings[].locations` | 数组 | 列出受影响的文件、行号和位置角色。                |
| `findings[].remediation` | 字符串 | 描述建议的修复。                                         |
| `findings[].provenance` | 对象 | 标识结果的来源。                                  |

例如，此命令为每个结果打印一个制表符分隔的行：

```bash
jq -r '
  .findings[] |
  [.findingId, .severity.level, .confidence.level, .locations[0].path, .locations[0].startLine, .title] |
  @tsv
' findings.json
```

这些示例假设使用 Node.js 和 `npm`、Git、Python 3、`jq` 以及提供商的命令行工具的可信 Linux 运行程序。 `npm` 全局包前缀必须是可写的。

选择适合您的 CI 提供商的示例：

扫描结果可能包括敏感漏洞详细信息。将工件保密，仅在审查受众、内容和所需批准后才发布调查结果。

<Tabs
  id="codex-security-ci-examples"
  param="ci"
  defaultTab="github"
  tabs={[
    { id: "github", label: "GitHub 行动" },
    { id: "gitlab", label: "GitLab CI/CD" },
    { id: "azure", label: "Azure管道" },
    { id: "jenkins", label: "詹金斯" },
  ]}
>
  


```yaml
name: Codex Security review

on:
  pull_request:

jobs:
  security-review:
    if: github.event.pull_request.head.repo.full_name == github.repository
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.pull_request.head.sha }}
          fetch-depth: 0
          persist-credentials: false

      - name: Install Codex Security
        env:
          CODEX_HOME: ${{ runner.temp }}/codex-home
        run: |
          npm install --global @openai/codex
          codex plugin add codex-security@openai-curated

      - name: Review code changes
        env:
          CODEX_SECURITY_API_KEY: ${{ secrets.CODEX_SECURITY_API_KEY }}
          CODEX_HOME: ${{ runner.temp }}/codex-home
          TMPDIR: ${{ runner.temp }}/codex-security
          BASE_SHA: ${{ github.event.pull_request.base.sha }}
          HEAD_REVISION: ${{ github.event.pull_request.head.sha }}
        run: |
          BASE_REVISION="$(git merge-base "$BASE_SHA" "$HEAD_REVISION")"
          CODEX_API_KEY="$CODEX_SECURITY_API_KEY" codex exec \
            --sandbox workspace-write \
            "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."

      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: codex-security-review
          path: ${{ runner.temp }}/codex-security/codex-security-scans
```

  


  


创建一个屏蔽的 `CODEX_SECURITY_API_KEY` CI/CD 变量，并在共享结果之前私下检查扫描工件。

```yaml
codex-security-review:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_SOURCE_PROJECT_ID == $CI_PROJECT_ID'
  variables:
    GIT_DEPTH: "0"
  script:
    - |
      codex_security_api_key="$CODEX_SECURITY_API_KEY"
      unset CODEX_SECURITY_API_KEY
      export CODEX_HOME="/tmp/codex-home-$CI_JOB_ID"
      export TMPDIR="/tmp/codex-security-$CI_JOB_ID"
      export BASE_REVISION="$CI_MERGE_REQUEST_DIFF_BASE_SHA"
      export HEAD_REVISION="${CI_MERGE_REQUEST_SOURCE_BRANCH_SHA:-$CI_COMMIT_SHA}"
      npm install --global @openai/codex
      codex plugin add codex-security@openai-curated
      CODEX_API_KEY="$codex_security_api_key" codex exec \
        --sandbox workspace-write \
        "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."
  after_script:
    - |
      unset CODEX_SECURITY_API_KEY
      scan_root="/tmp/codex-security-$CI_JOB_ID/codex-security-scans"
      if [ -d "$scan_root" ]; then
        tar -czf codex-security-artifacts.tar.gz -C "$scan_root" .
      fi
  artifacts:
    when: always
    paths:
      - codex-security-artifacts.tar.gz
```

  


  


```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:
  - checkout: self
    fetchDepth: 0

  - bash: |
      set -euo pipefail
      export CODEX_HOME="$AGENT_TEMPDIRECTORY/codex-home"
      npm install --global @openai/codex
      codex plugin add codex-security@openai-curated
    displayName: Install Codex Security

  - bash: |
      set -euo pipefail
      export CODEX_HOME="$AGENT_TEMPDIRECTORY/codex-home"
      export TMPDIR="$AGENT_TEMPDIRECTORY/codex-security"
      export HEAD_REVISION="$SYSTEM_PULLREQUEST_SOURCECOMMITID"
      export BASE_REVISION="$(git merge-base HEAD^1 "$HEAD_REVISION")"
      CODEX_API_KEY="$CODEX_SECURITY_API_KEY" codex exec \
        --sandbox workspace-write \
        "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."
    displayName: Review code changes
    condition: and(succeeded(), ne(variables['System.PullRequest.IsFork'], 'True'))
    env:
      CODEX_SECURITY_API_KEY: $(CODEX_SECURITY_API_KEY)

  - publish: $(Agent.TempDirectory)/codex-security/codex-security-scans
    artifact: codex-security-review
    condition: always()
```

对于 Azure Repos，配置 **构建验证** 分支策略以根据拉取请求运行管道。

  


  


```groovy
pipeline {
  agent { label 'linux' }
  stages {
    stage('Codex Security review') {
      when {
        allOf {
          changeRequest()
          expression { !env.CHANGE_FORK?.trim() }
        }
      }
      steps {
        sh '''#!/usr/bin/env bash
          set -euo pipefail
          export CODEX_HOME="/tmp/codex-home-$BUILD_TAG"
          export TMPDIR="/tmp/codex-security-$BUILD_TAG"
          mkdir -p "$TMPDIR"
          git fetch --no-tags origin "$CHANGE_TARGET"
          target="$(git rev-parse FETCH_HEAD)"
          git fetch --no-tags origin "$CHANGE_BRANCH"
          git rev-parse FETCH_HEAD > "$TMPDIR/head"
          git merge-base "$target" "$(cat "$TMPDIR/head")" > "$TMPDIR/base"
          npm install --global @openai/codex
          codex plugin add codex-security@openai-curated
        '''
        withCredentials([string(credentialsId: 'codex-security-api-key', variable: 'CODEX_SECURITY_API_KEY')]) {
          sh '''#!/usr/bin/env bash
            set +x
            set -euo pipefail
            export CODEX_HOME="/tmp/codex-home-$BUILD_TAG"
            export TMPDIR="/tmp/codex-security-$BUILD_TAG"
            export HEAD_REVISION="$(cat "$TMPDIR/head")"
            export BASE_REVISION="$(cat "$TMPDIR/base")"
            CODEX_API_KEY="$CODEX_SECURITY_API_KEY" codex exec \
              --sandbox workspace-write \
              "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."
          '''
        }
      }
      post {
        always {
          sh '''#!/usr/bin/env bash
            set -euo pipefail
            scan_root="/tmp/codex-security-$BUILD_TAG/codex-security-scans"
            if [ -d "$scan_root" ]; then
              tar -czf codex-security-artifacts.tar.gz -C "$scan_root" .
            fi
          '''
          archiveArtifacts artifacts: 'codex-security-artifacts.tar.gz', allowEmptyArchive: true
        }
      }
    }
  }
}
```

  

</Tabs>

这些示例跳过分叉的拉取请求。仅从受保护的管道定义运行凭据作业，并且仅针对具有扫描凭据的可信贡献者运行凭据作业。存档 `codex-security-scans`，以将结构化调查结果、清单、覆盖范围和 `report.md` 以及任何请求的 `findings/` 或 `hardening/` 输出保存在一起。在将作业作为必要检查之前，首先查看咨询结果并检查覆盖范围和运行时间。

有关 API 密钥处理和沙箱控制，请参阅 [非交互模式](../../non-interactive-mode.zh-CN.md)。如果您的组织允许 [Codex GitHub 动作](../../github-action.zh-CN.md)，则可以在运行时安装 CLI，但您仍必须先安装插件并将操作的 `codex-home` 输入指向同一个 `CODEX_HOME`。