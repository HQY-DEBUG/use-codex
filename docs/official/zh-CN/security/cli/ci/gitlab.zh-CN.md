> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../../en/security/cli/ci/gitlab.md)。

<a id="run-codex-security-in-gitlab-cicd"></a>

# GitLab CI/CD 安全扫描

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

在 GitLab CI/CD 中运行 Codex Security 以扫描已提交的更改和受保护的分支，将结果发布到 GitLab 安全性，并可选择在草稿合并请求中提出经过验证的修复。

该工作流程将扫描凭据与仓库写入访问分开。生成的更改在合并之前始终需要人工审核。

从仅扫描报告开始。仅在检查项目的运行程序、结果和凭证边界后才启用修复。

<a id="before-you-begin"></a>

## 开始之前

您需要：

- 具有受信任运行程序的 GitLab 项目，支持 Codex 沙箱的用户命名空间。
- GitLab 项目中的维护者或所有者角色，以便您可以配置 [项目 CI/CD 变量](https://docs.gitlab.com/ci/variables/) 和受保护的资源。
- 具有 Codex Security 访问权限的 OpenAI API 密钥。使用平台 API 密钥的组织可以 [请求网络可信访问](https://openai.com/form/enterprise-trusted-access-for-cyber/)。使用ChatGPT认证的个人可以使用[个人可信访问流程](https://chatgpt.com/cyber)。某些帐户或仓库需要此访问权限才能进行完整仓库扫描。
- GitLab Ultimate 19.2 或更高版本（适用于 [SARIF 2.1.0 摄取](https://docs.gitlab.com/user/application_security/detect/sarif/)）。
- 完整的 Git 历史记录，以便合并请求作业可以计算合并基础。

管道映像安装 Node.js 26、Python 3、Git、`rg` 和固定的 Codex Security CLI。自动修复还需要现有的回归测试和运行程序，该运行程序可以在没有受保护凭据的情况下运行仓库控制的命令。

<a id="start-with-a-scan-only-pipeline"></a>

## 从仅扫描管道开始

创建一个名为 `CODEX_SECURITY_API_KEY` 的屏蔽、隐藏、受保护的 GitLab CI/CD 变量。使用具有 Codex Security 访问权限的 OpenAI 平台 API 密钥，并将其环境范围设置为 `codex-security/openai`。参见 [环境范围的 CI/CD 变量](https://docs.gitlab.com/ci/environments/#limit-the-environment-scope-of-a-cicd-variable)。

首先将此最小管道添加到测试项目中。它扫描符合条件的受保护合并请求中提交的更改，从成功的报告作业中发布 SARIF，并在单独的门中恢复扫描结果：

```yaml
stages:
  - security_scan
  - security_gate

.codex-security-merge-request:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_SOURCE_PROJECT_ID == $CI_PROJECT_ID && $CI_MERGE_REQUEST_SOURCE_BRANCH_PROTECTED == "true" && $CI_MERGE_REQUEST_TARGET_BRANCH_PROTECTED == "true"'

codex-security:
  extends: .codex-security-merge-request
  stage: security_scan
  image: node:26-bookworm-slim
  environment:
    name: codex-security/openai
    action: access
  variables:
    GIT_DEPTH: "0"
  before_script:
    - npm install --prefix /tmp/codex-security-cli --ignore-scripts --no-audit --no-fund @openai/codex-security@0.1.20
  script:
    - |
      set -eu
      test -n "${CODEX_SECURITY_API_KEY:-}"

      CODEX_SECURITY_BIN="/tmp/codex-security-cli/node_modules/.bin/codex-security"
      RESULTS_DIR="/tmp/codex-security-results-$CI_JOB_ID"
      ARTIFACT_DIR="codex-security-artifacts"
      BASE_REVISION="$(git merge-base \
        "$CI_MERGE_REQUEST_DIFF_BASE_SHA" "$CI_COMMIT_SHA")"
      install -d -m 700 "$RESULTS_DIR" "$ARTIFACT_DIR/results"

      codex_security_api_key="$CODEX_SECURITY_API_KEY"
      unset CODEX_SECURITY_API_KEY
      set +e
      OPENAI_API_KEY="$codex_security_api_key" \
        "$CODEX_SECURITY_BIN" scan . \
          --diff "$BASE_REVISION" \
          --head "$CI_COMMIT_SHA" \
          --auth api-key \
          --output-dir "$RESULTS_DIR" \
          --json
      scan_exit="$?"
      set -e
      unset codex_security_api_key

      case "$scan_exit" in
        0|1|2) ;;
        *) exit "$scan_exit" ;;
      esac

      "$CODEX_SECURITY_BIN" export "$RESULTS_DIR" \
        --export-format sarif \
        --source-root "$CI_PROJECT_DIR" \
        --output "$ARTIFACT_DIR/results.sarif"
      test -s "$ARTIFACT_DIR/results.sarif"
      cp -R "$RESULTS_DIR"/. "$ARTIFACT_DIR/results/"
      printf '%s\n' "$scan_exit" > "$ARTIFACT_DIR/scan-exit-code.txt"
      exit 0
  artifacts:
    when: always
    access: maintainer
    expire_in: 7 days
    paths:
      - codex-security-artifacts/
    reports:
      sarif: codex-security-artifacts/results.sarif

codex-security-gate:
  extends: .codex-security-merge-request
  stage: security_gate
  image: alpine:3.20
  needs:
    - job: codex-security
      artifacts: true
  script:
    - exit "$(cat codex-security-artifacts/scan-exit-code.txt)"
```



> 插图：GitLab 管道扫描提交的差异，发布 SARIF 报告，并在策略门中恢复扫描结果



在运行秘密承载作业之前，请检查对 `.gitlab-ci.yml` 的每项更改。这个最小的示例故意省略了完整扫描和修复。

<a id="adopt-the-production-pipeline"></a>

## 采用生产流水线

1. [下载完整的 GitLab 管道](https://learn.chatgpt.com/docs/security/cli/ci/gitlab.yml) 并将其另存为 `.gitlab-ci.yml` 在仓库根目录中。如果您的仓库已有管道，请将示例的阶段、隐藏模板和作业合并到现有文件中。
2. 保留现有的构建、测试和部署阶段。如果项目使用 `workflow: rules`，请确认它允许您要扫描的管道事件。

该示例添加了 `security_scan`、`security_remediation`、`security_publish` 和 `security_gate` 级。仅扫描报告仅需要 `CODEX_SECURITY_API_KEY`。

默认情况下，扫描作业仅针对受保护分支之间的同一项目合并请求运行。设置 `CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH=true` 扫描受保护的默认分支推送和手动管道。设置 `CODEX_SECURITY_SCHEDULED_DEEP_SCAN=true` 并配置明确的时间和成本预算，以在受保护的默认分支上启用计划的深度扫描。

合并请求管道仅在以下情况下才能访问受保护的变量和运行器：

- 您可以保护同一项目中的源分支和目标分支。
- 项目[允许合并请求管道访问受保护的变量和运行程序](https://docs.gitlab.com/ci/pipelines/merge_request_pipelines/#control-access-to-protected-variables-and-runners)。
- 启动管道的用户可以推送或合并到目标分支。

分叉管道和不受保护的合并请求不会收到扫描凭据。在运行秘密承载作业之前，请检查对 `.gitlab-ci.yml` 的每项更改。屏蔽和隐藏变量并不能使不受信任的 CI 代码变得安全。

<a id="run-a-scan-and-review-findings"></a>

## 运行扫描并查看结果

创建合格的受保护合并请求或在受保护的默认分支上运行管道。在运行付费的全仓库扫描之前，先从一个小的差异开始。

打开 `codex-security` 作业并确认其工件包括：

- `scan-manifest.json`
- `findings.json`
- `coverage.json`
- `results.sarif`
- `scan-exit-code.txt`

然后打开管道 **安全性** 选项卡，查看摄入警告，并确认查找标识符、严重性级别和源位置。默认分支扫描还会创建项目漏洞记录。合并请求结果显示在管道安全选项卡或合并请求安全小部件中，但不会创建项目范围的漏洞记录。

限制工件访问，因为扫描结果可能包含易受攻击的源代码片段、证据和补救详细信息。

<a id="choose-a-scan-profile"></a>

## 选择扫描配置文件

管道从触发器中选择一个配置文件：

| 触发 | 目标 | 模式 | 努力 |
| ---------------------------------------------- | --------------- | ---------- | ------- |
| 受保护的同一项目合并请求 | 提交的差异 | `standard` | `low` |
| 选择加入受保护的默认分支推送或手动 | 完整仓库 | `standard` | `high` |
| 受保护的默认分支上的选择加入计划 | 完整仓库 | `deep` | `xhigh` |

合并请求扫描将反馈集中在已提交的更改上。默认分支扫描会检查集成仓库。预定的深度扫描提供更广泛的定期覆盖范围。完成的差异扫描仅适用于该更改，并不表明整个仓库是干净的。

该工作流在仓库外部安装 CLI 并通过绝对路径运行它。其试运行预检使用进程范围的 API 密钥，但不会启动付费扫描或验证 API 身份验证、Codex Security 访问、配额或模型可用性。

工作流将工作树和范围 `OPENAI_API_KEY` 之外的扫描状态和结果写入扫描进程。 CLI 接收一个小的、显式的环境，而不是继承每个 GitLab 变量。对于差异扫描，工作流程计算合并基础并将扫描绑定到已审查的基础和头部修订。

该示例引脚 `@openai/codex-security` 至 `0.1.20`。在更改 pin 之前重新测试身份验证、工件、SARIF 摄取和策略门控。

<a id="separate-reporting-from-policy-enforcement"></a>

## 报告与政策执行分开

GitLab 从成功的报告作业中提取 SARIF。管道首先发布报告，并在单独的 `codex-security-gate` 作业中恢复扫描仪的退出状态。

报告作业接受退出代码 `0` 和 `1` 的结果。仅当扫描清单证明扫描已完成、覆盖范围明确为 `partial` 并且存在非空 SARIF 报告时，它才接受退出代码 `2`。其他运行时、配置或导出失败仍处于阻塞状态。

最后一个门保留这些扫描仪退出代码：

| 退出 | 含义 |
| ---- | --------------------------------------------------------------------------- |
| `0` | 扫描已完成，覆盖完整，并通过了其策略。            |
| `1` | 扫描已完成并发现问题等于或高于配置的阈值。 |
| `2` | 扫描覆盖不完整或者存在输入或运行时错误。              |

该示例在您校准部分覆盖范围时暂时允许退出 `2`。当不完全覆盖必须堵塞管道时，请取消该津贴。

修复和发布在最终政策关卡之前运行。即使门后来使管道失败，合格的发现也可以生成经过验证的草稿合并请求。

<a id="enable-verified-remediation"></a>

## 启用经过验证的补救措施

自动修复是可选的，并且仅针对受保护的默认分支管道运行。 Codex 修复过程和仓库控制的验证命令不会收到 GitLab 项目访问令牌或运行程序注入的凭据。

安全合约由三个部分组成：仓库控制的命令永远不会接收 OpenAI 或 GitLab 凭证，只有发布作业接收仓库写入访问权限，并且每个生成的更改都保持草稿状态，直到有人审查并合并它。

工作流程：

1. 需要完整的扫描覆盖范围和 `high`- 或 `critical`- 严重性发现。
2. 在修补之前确认配置的回归测试失败。
3. 生成重点补丁并拒绝对 CI、凭据、二进制文件或其他受保护文件的更改。
4. 在没有 OpenAI、GitLab、注册表、部署或作业令牌凭据的情况下运行回归测试。
5. 使用 `verify-fix` 返回 `fixed`、`still_vulnerable` 或 `inconclusive`。仅当 `verify-fix` 返回 `fixed` 并且验证过程使补丁保持不变时，作业才会发布补丁。

设置这些受保护的变量以启用修复：

- 将 `CODEX_SECURITY_ENABLE_REMEDIATION` 设置为 `true`。
- 将 `CODEX_SECURITY_VERIFICATION_COMMAND` 设置为现有回归测试，该测试在修复之前退出 `1`，之后退出 `0`。
- （可选）将 `CODEX_SECURITY_SETUP_COMMAND` 设置为非交互式依赖性设置命令。

选择执行底层安全不变式的回归测试，而不是特定的实现。对生成的测试和源代码更改应用相同的审查。

<details>
  <summary>高级：仓库命令隔离</summary>

`validate`、`patch` 和 `verify-fix` 命令接收进程范围的 `CODEX_API_KEY`。仓库控制的设置和测试命令作为单独的非特权用户在跟踪的源文件的可写副本中运行。该副本有意排除 Git 元数据、子模块内容和下载的工件。需要 `.git` 或子模块的设置和测试命令必须在单独设计的无凭据作业中运行。

只有 root 拥有的 Codex 步骤才能访问规范签出或 GitLab 的相邻文件变量目录。副本的干净环境仅包含 `PATH`、`HOME`、`LANG`、`CI` 和 `CI_PROJECT_DIR`。如果命令需要另一个非秘密值，请在检查该命令后将其添加到允许列表中。如果您的运行程序无法更改用户，请在启用修复之前将验证移至单独的无凭据作业中。

</details>

<a id="publish-a-draft-merge-request"></a>

## 发布草稿合并请求

创建具有开发人员角色以及 `api` 和 `write_repository` 范围的 [GitLab 项目访问令牌](https://docs.gitlab.com/user/project/settings/project_access_tokens/#create-a-project-access-token)。将其存储为受保护、屏蔽、隐藏的 `GITLAB_REMEDIATION_TOKEN`，仅适用于 `codex-security/publish` 环境。

设置 `CODEX_SECURITY_CREATE_MR=true` 以启用发布。还将非秘密 `CODEX_SECURITY_MR_TEST_COMMAND` 设置为每个生成的修复分支都必须通过的特定于项目的安全回归测试。保持此变量不受保护，以便生成的不受保护的合并请求可以读取该命令。发布工作流程：

- 接收仓库写入令牌，但没有 OpenAI 凭证。
- 创建一个“codex-security/fix-”<finding-hash>` 分支。
- 打开草稿合并请求并重用现有的打开草稿而不是创建副本。
- 在仅跟踪副本中以非特权用户身份运行未受保护的修复分支的回归测试，而无需受保护的凭据。
- 永远不会自动合并生成的更改。

不要用 `CI_JOB_TOKEN` 替换项目访问令牌。它无法执行所需的合并请求创建操作。在合并之前审查提议的补丁、验证证据和发现结果。

<a id="configure-optional-variables"></a>

## 配置可选变量

仅配置您启用的功能所需的变量：

| 变量 | 需要时 | 默认或用途 |
| ----------------------------------------- | --------------------------------- | ----------------------------------------------------------- |
| `CODEX_SECURITY_API_KEY` | 每次扫描 | 受保护、屏蔽、隐藏；范围至 `codex-security/openai` |
| `CODEX_SECURITY_VERSION` | CLI 升级 | 固定到 `0.1.20`；更改前重新测试 |
| `CODEX_SECURITY_FULL_SCAN_DEFAULT_BRANCH` | 默认分支完整扫描 | 显式选择加入；默认关闭 |
| `CODEX_SECURITY_SCHEDULED_DEEP_SCAN` | 预定深度扫描 | 显式选择加入；默认关闭 |
| `CODEX_SECURITY_DEEP_MAX_TIME_HOURS` | 计划深度扫描 | 所需时间预算大于 `0` 且小于 `8` |
| `CODEX_SECURITY_DEEP_MAX_COST` | 预定的深度扫描 | 所需的估计美元成本护栏大于 `0` |
| `CODEX_SECURITY_ENABLE_REMEDIATION` | 补丁生成 | 受保护的选择加入；默认关闭 |
| `CODEX_SECURITY_VERIFICATION_COMMAND` | 补丁生成 | 受保护的回归测试 |
| `CODEX_SECURITY_SETUP_COMMAND` | 可选修复设置 | 受保护的依赖项安装 |
| `CODEX_SECURITY_REMEDIATION_EFFORT` | 可选修复调整 | `high` |
| `CODEX_SECURITY_MAX_CHANGED_FILES` | 可选贴片尺寸限制 | `8`；允许范围 `1` 至 `20` |
| `CODEX_SECURITY_CREATE_MR` | 草稿合并请求创建 | 受保护的选择加入；默认关闭 |
| `GITLAB_REMEDIATION_TOKEN` | 草稿合并请求创建 | 范围为 `codex-security/publish` | 的开发者项目令牌
| `CODEX_SECURITY_GITLAB_INTERNAL_URL` | 可选的自托管发布 | GitLab 可从运行器到达的原点 |
| `CODEX_SECURITY_MR_TEST_COMMAND` | 草稿合并请求发布 | 必需的非秘密、特定于项目的回归测试 |
| `CODEX_SECURITY_MR_SETUP_COMMAND` | 可选修复分支设置 | 非秘密依赖项设置 |

GitLab 提供 `CI_*` 变量。该管道管理`CODEX_SECURITY_BIN`、`CODEX_SECURITY_EFFORT`、`CODEX_SECURITY_MODE`、`CODEX_SECURITY_STATE_DIR`和`CODEX_SECURITY_TARGET`；不要将它们配置为项目变量。对于差异扫描，CLI 从规范化的基础和头部修订中派生规范的目标身份。

<a id="tune-enforcement-and-cost"></a>

## 调整执行和成本

使用集中差异扫描来获取合并请求反馈，使用标准仓库扫描来获取默认分支，并使用计划的深度扫描来获取更广泛的覆盖范围。默认情况下，两个完整仓库配置文件均处于关闭状态。预定深度扫描还需要 `CODEX_SECURITY_DEEP_MAX_TIME_HOURS` 和 `CODEX_SECURITY_DEEP_MAX_COST`；将 CLI 时间预算保持在作业的八小时超时以下。衡量代表在制定预算之前运行。将 `--max-cost` 视为估计成本护栏，而不是硬性计费上限。

从仅报告扫描开始。在您的团队审查了代表性调查结果、覆盖范围、成本和运行时间后添加 `--fail-on-severity`。有关严重性策略和退出代码详细信息，请参阅 [在 CI 中运行 Codex Security](../ci.zh-CN.md)。

当作业失败时：

- 丢失的扫描工件表明存在配置或运行程序问题。
- 部分覆盖的现有工件需要审查 `coverage.json`。
- 缺少 GitLab 结果需要检查 SARIF 报告作业是否成功以及 GitLab 是否接受该报告。
- 跳过的修复需要检查受保护的分支、完整的覆盖范围、查找严重性、验证命令和选择加入变量。
- 发布错误需要检查项目Token的角色、范围和环境限制。

对于每个命令、标志和工件，请参阅 [Codex Security CLI 参考](../reference.zh-CN.md)。