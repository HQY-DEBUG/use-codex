> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/plugin/export-findings.md)。

<a id="export-and-track-security-findings"></a>

# 导出与跟踪发现

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用完整的 Codex Security 扫描进行以下任一切换：

- **出口** 创建可移植的 JSON、CSV 或 SARIF 文件。
- **跟踪调查结果** 将选定的调查结果准备为 Linear、GitHub 或 Jira 问题，或作为一份私人草稿 GitHub 安全报告。 Codex 在写入之前会检查重复项并等待您的批准。

两个工作流程都不会改变密封的扫描包。

可用的工件链接和导出格式取决于您的 Codex 使用界面和安装的插件版本。在自动化中使用格式之前检查 [插件变更日志](changelog.zh-CN.md)。

<a id="export-a-portable-artifact"></a>

## 导出便携式工件

在桌面应用程序中，从 **安全性** > **扫描** 打开已完成的扫描。使用其可用的工件链接检查 `report.md`、`findings.json`、`scan-manifest.json`、`coverage.json` 或 SARIF 报告（如果存在）。

要创建另一种受支持的格式，请要求 Codex 从已完成的扫描中导出结果，而不修改其密封包：

```text
将 [completed 扫描目录 ] 中的结果导出为 [JSON、CSV 或 SARIF]。请勿修改密封的扫描包或上传其内容。
```

选择适合您目的地的格式：

| 格式 | 用于 |
| ------ | ----------------------------------------------------------------- |
| JSON | 保留工具和脚本的密封结构化结果。    |
| CSV | 在电子表格中查看结果和当前本地分类状态。  |
| SARIF | 将结果发送到支持 SARIF 交换格式的工具。 |

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="完成的 Codex Security 扫描显示真实的覆盖范围、结果、清单、Markdown 和 SARIF 工件"
    lightSrc={exportFindingsFormats.src}
    darkSrc={exportFindingsFormatsDark.src}
    maxHeight="360px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
打开已完成扫描的覆盖范围、结果、扫描清单、Markdown 报告或 SARIF 工件。
  </figcaption>
</figure>

选择 **降价报告** 在您配置的外部编辑器中打开 `report.md`。编辑器取决于您的系统设置；下面的示例显示了生成的报告内容。

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="生成的安全报告示例显示扫描范围、威胁模型和经过验证的结果"
    lightSrc={exportFindingsReport.src}
    darkSrc={exportFindingsReportDark.src}
    maxHeight="600px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
查看生成的 Markdown 报告中的扫描范围、威胁模型、经过验证的结果和详细报告链接。
  </figcaption>
</figure>

使用返回的工件路径。如果其他工具需要完整的扫描上下文，请将原始 `scan-manifest.json`、`findings.json` 和 `coverage.json` 保留在一起。导出不会将结果上传到代码扫描服务。

<a id="track-selected-findings"></a>

## 跟踪选定的发现

运行 `$codex-security:track-findings`，其中包含一项经过验证的结果或来自同一密封扫描的明确选择的批次（最多 25 个结果）。每次运行使用一个提供者和一个目的地。一份私人草案 GitHub 安全咨询仅接受一项调查结果。

要准备线性问题，请发送：

```text
使用 $codex-security:track-findings 准备查找 [查找 ID]
[completed scan directory] for the Linear team [team] and project [project, if
任意]。检查重复项并显示确切的问题标题、正文、元数据、
和目的地。在我批准该有效负载之前，请勿创建或更新任何内容。
```

要准备 GitHub 问题，请发送：

```text
使用 $codex-security:track-findings 准备查找 [查找 ID]
[completed scan directory] for GitHub repository [owner/repository]. Check open
并关闭重复问题并向我显示确切的问题标题、正文、
元数据、仓库可见性和经过身份验证的传输。不要创建或
更新任何内容，直到我批准该有效负载。
```

要准备 Jira 问题，请发送：

```text
使用 $codex-security:track-findings 准备查找 [查找 ID]
[completed scan directory] for Jira project [project key] as [issue type].
检查重复项并向我显示确切的问题摘要、描述、
元数据和目的地。在我批准之前不要创建或更新任何内容
该有效负载。
```

Jira 跟踪需要 Codex 中的 Atlassian Rovo 插件。重用问题需要读取权限；创建或更新需要读取和写入访问权限。

要准备私人草稿 GitHub 安全通报，请发送：

```text
使用 $codex-security:track-findings 准备查找 [查找 ID]
[completed scan directory] as a private draft GitHub Security Advisory in
[owner/repository]. Verify the sealed source revision, repository, affected
路径、包元数据和重复状态。显示确切的建议
有效负载、经过身份验证的 GitHub CLI 身份和泄露警告。不
创建任何内容，直到我批准该有效负载。
```

草案建议需要密封的 `git_revision` 扫描、经过验证的公共规范源仓库和管理员访问权限中的一项发现。该工作流程不会批量、更新、发布或关闭建议。当来源不满足这些要求时，请使用经批准的私人发行目的地。

<a id="review-the-proposed-write"></a>

## 检查建议的写入

<WorkflowSteps>

1. 确认发现的 ID 和指纹来自预期的密封扫描。
2. 确认提供商、确切的 Linear 团队、GitHub 仓库、Jira 项目或咨询仓库以及实时目标可见性。
3. 查看重复结果：`create`、`reuse`、`update` 或 `blocked`。
4. 阅读完整的建议标题、正文、源位置和提供者元数据。删除目的地不应公开的漏洞利用细节或内部证据。
5. 仅批准确切的有效负载。更改的目的地、可见性、结果集或正文需要新的预览。

</WorkflowSteps>

敏感发现应发送至私人目的地。在内部或公共 GitHub 仓库中创建问题需要明确的可见性警告并批准完整内容。将咨询描述草案视为最终公开，并在批准之前删除凭证、私人证据和不必要的利用细节。

审查并批准 Codex 对话中的外部操作。批准不会在安全工作台中创建单独的问题或咨询屏幕。

<a id="verify-the-tracked-item"></a>

## 验证跟踪的项目

在您批准建议的写入后，Codex 会重新检查密封的源、目的地、访问和重复状态。对于一批，它一次处理一个结果，并在第一个不确定结果处停止。只有在 Codex 读回确切的问题并验证其绑定标识符和内容后，创建、更新或重用才完成。

将返回的规范问题或咨询 URL 与您的分类记录一起保留。当所有者接受该项目进行修复后，继续 [修复并验证发现的结果](fix-findings.zh-CN.md)。