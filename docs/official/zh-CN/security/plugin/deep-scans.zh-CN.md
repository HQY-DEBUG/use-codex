> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/plugin/deep-scans.md)。

<a id="run-a-deep-security-scan"></a>

# 深度安全扫描

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

当您需要更彻底的检查并且可以允许更长的运行时间时，运行深度扫描。深度扫描可以更广泛地搜索仓库，并且可以减少运行之间的差异。

从 [标准扫描](scans.zh-CN.md) 开始检查您的范围和结果。当您需要更彻底的评估时，请使用深度扫描。

<a id="choose-between-standard-and-deep-scans"></a>

## 在标准扫描和深度扫描之间进行选择

|                         | 标准扫描 | 深度扫描 |
| ----------------------- | -------------------------------------------------- | ----------------------------------------------------- |
| 最适合 | 首次运行和例行仓库或文件夹审查 | 标准扫描后更彻底的审查 |
| 可变性 | 标准型 | 降低型 |
| 范围 | 仓库或显式文件夹 | 仓库或显式文件夹 |
| 运行时和资源 | 较低 | 较高 |
| 拉取请求和差异 | 使用变更审核工作流程 | 不支持；使用变更审核工作流程代替 |

<a id="configure-deep-scan-runtime"></a>

## 配置深度扫描运行时

要控制深度扫描的并发性和持续时间，请创建或编辑 `~/.codex/codex-security/config.toml`。如果设置 `CODEX_HOME`，请改用 `$CODEX_HOME/codex-security/config.toml`。

例如，此配置文件以有限的并发性运行较短的扫描：

```toml
[deep_scan]
workers = 2
subagents = 0
stop_after_no_new = 3
max_discovery_runs = 10
max_time_hours = 1.5
```

| 设置 | 默认值 | 说明 |
| ------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| `workers` | `4` | 允许同时运行的独立标准扫描工作器的数量。旧版 `"auto"` 也解析为 `4`。 |
| `subagents` | `3` | 每个工作人员可以启动的子智能体数量。设置 `0` 以禁用它们。                                                |
| `stop_after_no_new` | `4` | 在多次连续完成的工作扫描未产生新发现后停止。                                   |
| `stop_after_consecutive_errors` | `3` | 在多次连续工作错误后停止。                                                                    |
| `max_discovery_runs` | `40` | 限制聚合之前独立标准扫描运行的数量。                                             |
| `max_time_hours` | `96` | 将工作器执行限制为正数小时，最多 `96`；根据需要使用分数。                          |

较低的值可以减少扫描时间和令牌使用，但可能会错过发现结果。配置更改适用于新的深度扫描，而不是已经正在进行的扫描。

当时间限制到期时，Codex Security 停止未完成的工作人员，保留已完成的扫描结果，并将其汇总到最终报告中。如果没有工作人员在截止日期前完成来源审查，报告将记录部分覆盖范围。

`max_time_hours` 设置需要插件版本 `0.1.19` 或更高版本。有关发布详细信息，请参阅 [插件变更日志](changelog.zh-CN.md)。

<a id="start-the-deep-scan"></a>

## 开始深度扫描

在桌面应用程序中，打开 **安全性**，选择 **扫描**，然后选择 **+ 扫描**。选择仓库或其他文件夹，选择 **代码库**，然后打开 **深度扫描**。扫描覆盖整个选定的仓库或文件夹。

您还可以从 Codex 对话启动仓库范围的深度扫描：

```text
使用 $codex-security:deep-security-scan 对此仓库运行深度安全扫描。
```

对于 monorepo 中的一个组件，明确标识该文件夹：

```text
使用 $codex-security:deep-security-scan 对 /absolute/path/to/repository/services/payments 运行深度安全扫描。
```

对于桌面应用程序中的范围深度扫描，请选择文件夹作为代码库。扫描覆盖整个选定的文件夹。

<a id="confirm-setup-and-preflight"></a>

## 确认设置和预检

为了获得最佳扫描质量，请使用 `gpt-5.6-sol` 和 `xhigh` 推理工作。

<WorkflowSteps>

1. 选择**代码库**并打开**深度扫描**。
2. 确认仓库或选定的文件夹是您要扫描的代码。
3. 选择模型和推理工作。
4. 打开 **额外的背景信息** 以获取代码无法透露的具体攻击向量、敏感应用程序区域或仓库上下文。
5. 选择**开始扫描**。

</WorkflowSteps>

深度扫描工作人员继承您选择的模型和推理设置。每个工作人员运行完整的标准扫描，Codex Security 汇总完成的结果。按照 **扫描** 保存的扫描进行操作，或选择 **查看活动** 检查其 Codex 任务。在更新插件或开始长时间运行的扫描之前检查 [插件变更日志](changelog.zh-CN.md)。

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="本机 Codex Security 工作台显示深度扫描及其主动审查阶段"
    lightSrc={deepScanProgress.src}
    darkSrc={deepScanProgressDark.src}
    maxHeight="520px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
在查看完成的结果之前，跟踪活动深度扫描阶段并检查其 Codex 活动。
  </figcaption>
</figure>

<a id="review-the-result"></a>

## 查看结果

深度扫描使用与标准扫描相同的已保存扫描详细信息和完整扫描目录。在 **扫描** 中打开已完成的扫描或在 **研究结果** 中查看其结果。当您请求这些输出时，生成的 `report.md` 链接到详细的漏洞报告或结构强化指南。共享或存档结果时，将所有链接的 `findings/` 和 `hardening/` 目录与报告一起保留。

在发现结果之前查看覆盖范围摘要。即使深度扫描也有局限性，因此在得出结论之前请检查延迟使用界面和剩余的验证间隙。对于您接受的调查结果，请继续 [修复并验证发现的结果](fix-findings.zh-CN.md)。

要查看拉取请求、提交、分支范围或本地补丁，请使用 [检查代码更改](code-changes.zh-CN.md)。深度扫描永远无法替代以差异为中心的工作流程。