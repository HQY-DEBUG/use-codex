> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/plugin/workbench.md)。

<a id="use-the-codex-security-workbench"></a>

# 安全工作台

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

安全工作台将您的扫描、结果和仓库集中在 Codex 桌面应用程序中。 Codex 在常规任务中执行扫描分析，而工作台会在您返回时保留扫描及其结果。

在 ChatGPT 桌面应用程序中，打开 ChatGPT 下拉列表并选择 **Codex**。安装并启用 [Codex Security插件](../plugin.zh-CN.md)，然后在侧栏中选择 **安全性**。

如果未出现 **安全性**，请确认已选择 **Codex** 并且已安装并启用该插件。如果需要，请更新桌面应用程序和插件，并检查您的工作区管理员是否允许该插件。

<a id="start-a-scan"></a>

## 开始扫描

为了获得最佳扫描质量，请使用 `gpt-5.6-sol` 和 `xhigh` 推理工作。

<WorkflowSteps>

1. 打开**扫描**并选择**+ 扫描**。
2. 选择现有仓库或选择其他文件夹。
3. 选择 **代码库** 来扫描仓库，或选择 **变化** 来查看 Git 支持的更改。
4. 对于标准代码库扫描，请选择整个仓库或文件夹。
5. 对于深度扫描，首先选择仓库或文件夹作为代码库，然后打开 **深度扫描**。深度扫描会检查整个选定的代码库。
6. 对于更改扫描，选择未提交的更改、提交或修订范围。 **深度扫描** 不可用于更改扫描。
7. 选择模型和推理工作。打开 **额外的背景信息** 来描述相关的攻击向量、重点区域或其他安全上下文。
8. 选择**开始扫描**。

</WorkflowSteps>

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="Codex Security 工作台显示新仓库扫描的设置"
    lightSrc={scanOverview.src}
    darkSrc={scanOverviewDark.src}
    maxHeight="520px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
选择一个仓库并在安全工作台中配置扫描。
  </figcaption>
</figure>

有关每种扫描类型的详细信息，请参阅 [运行安全扫描](scans.zh-CN.md)、[运行深度安全扫描](deep-scans.zh-CN.md) 或 [检查代码更改以确保安全](code-changes.zh-CN.md)。

<a id="follow-scan-progress"></a>

## 跟踪扫描进度

扫描页面显示当前阶段以及插件报告的任何扫描进度。对于标准扫描，阶段包括威胁建模、发现、验证、影响和路径分析、报告和最终确定。

选择 **查看活动** 打开运行扫描的 Codex 任务。您可以离开工作台并返回 **扫描**，而不会丢失已保存的扫描。要有意停止工作，请打开扫描并选择 **停止扫描**。

扫描完成后，打开其结果以查看目标、修订、结果、覆盖范围和可用的报告工件。

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="完成的 Codex Security 扫描显示结果、扫描覆盖范围和报告工件"
    lightSrc={findingsWorkspace.src}
    darkSrc={findingsWorkspaceDark.src}
    maxHeight="520px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
扫描完成后查看结果、严重性、扫描覆盖范围和伪影。
  </figcaption>
</figure>

<a id="review-findings-across-scans"></a>

## 检查扫描结果

打开 **研究结果** 以检查跨仓库和扫描保存的结果。搜索或筛选列表，然后选择一个结果来查看其摘要、来源证据、验证和影响。

使用 **总结** 获取查找详细信息，使用 **补丁** 生成、查看、应用或验证重点修复。请参阅 [修复并验证安全结果](fix-findings.zh-CN.md) 了解修复工作流程。

**研究结果** 选项卡显示保存的 Codex Security 扫描的结果。导入的票据和其他现有的安全问题仍然是单独的 [待办事项分类工作流程](triage-backlog.zh-CN.md) 的一部分。

<a id="inspect-repository-history"></a>

## 检查仓库历史记录

打开 **仓库** 浏览可用的仓库和文件夹。选择一个仓库以检查其扫描历史记录、最新扫描的修订版本和打开的结果。从仓库详细信息中，打开以前的扫描或查看与该仓库关联的结果。

如果仓库没有扫描，请从其详细信息开始扫描或在工作台中选择 **+ 扫描**。

<a id="start-a-scan-from-a-conversation"></a>

## 从对话开始扫描

您还可以要求 Codex 在常规对话中运行已安装的 Codex Security 插件。使用共享插件工作台的扫描显示在 **扫描** 中，因此您可以从安全工作台返回其进度和结果。

对于基于终端的扫描和自动化，请参阅 [Codex Security CLI 快速入门](../cli.zh-CN.md)。