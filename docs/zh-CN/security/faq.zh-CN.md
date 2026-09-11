> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/faq.md)。

<a id="codex-security-cloud-faq"></a>

# Security 云端常见问题

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

此常见问题解答涵盖 Codex Security 云。对于在 Codex 任务中运行的本地扫描和工作流程，请参阅 [Codex Security 插件快速入门](plugin.zh-CN.md)。

{/* vale Microsoft.Auto = NO */}
{/* vale Vale.Spelling = NO */}

<a id="getting-started"></a>

## 开始使用

<a id="what-is-codex-security"></a>

### Codex Security是什么？

软件安全仍然是工程中最困难和最重要的问题之一。 Codex Security 是一个 LLM 驱动的安全分析工具包，可检查源代码并返回结构化、排名的漏洞结果以及建议的补丁。它可以帮助开发人员和安全团队大规模发现并修复安全问题。

<a id="why-does-it-matter"></a>

### 为什么这很重要？

软件是现代工业和社会的基础，漏洞会带来系统性风险。 Codex Security 通过不断识别可能的问题、在可能的情况下验证问题并提出修复建议，支持防御者优先的工作流程。这有助于团队在不减慢开发速度的情况下提高安全性。

<a id="what-business-problem-does-codex-security-solve"></a>

### Codex Security 解决什么业务问题？

Codex Security 缩短了从可疑问题到通过证据和提议的补丁确认、可重现的发现的路径。与单独使用传统扫描仪相比，这可以减少分类负载并减少误报。

<a id="how-does-codex-security-work"></a>

### Codex Security如何工作？

Codex Security 在临时的隔离容器中运行分析并临时克隆目标仓库。它执行代码级分析并返回结构化结果，其中包含描述、文件和位置、关键性、根本原因和建议的补救措施。

对于包括验证步骤的发现，系统在同一沙箱中执行建议的命令或测试，记录成功或失败、退出代码、标准输出、标准错误、测试结果以及任何生成的差异或工件，并将该输出附加为审查证据。

<a id="does-it-replace-sast"></a>

### 它会取代 SAST 吗？

不会。Codex Security 是先科的补充。它增加了语义、基于 LLM 的推理和自动验证，而现有的 SAST 工具仍然提供广泛的确定性覆盖。

<a id="features"></a>

## 特点

<a id="what-is-the-analysis-pipeline"></a>

### 分析流程是什么？

Codex Security 遵循分阶段管道：

1. **分析** 为仓库构建威胁模型。
2. **提交扫描** 检查合并的提交和仓库历史记录是否存在可能的问题。
3. **验证** 尝试在沙箱中重现可能的漏洞以减少误报。
4. **打补丁** 与 Codex 集成，以提出补丁供审阅者在打开 PR 之前检查。

它与 GitHub、Codex 和标准审核工作流程中的工程师一起工作。

<a id="what-languages-are-supported"></a>

### 支持哪些语言？

Codex Security 与语言无关。在实践中，性能取决于模型对仓库使用的语言和框架的推理能力。

<a id="what-outputs-do-i-get-after-the-scan-completes"></a>

### 扫描完成后我会得到什么输出？

您会获得对结果进行排序的重要性、验证状态以及建议的补丁（如果有可用的补丁）。调查结果还可以包括崩溃输出、复制证据、调用路径上下文和相关注释。

<a id="how-is-customer-code-isolated"></a>

### 客户代码如何隔离？

每个分析和验证作业都在带有会话范围工具的临时 Codex 容器中运行。工件被提取以供检查，工作完成后容器将被拆除。

<a id="does-codex-security-auto-apply-patches"></a>

### Codex Security 是否自动应用补丁？

不会。建议的补丁是建议的补救措施。用户可以查看它并将其作为 PR 从结果 UI 推送到 GitHub，但 Codex Security 不会自动将更改应用到仓库。

<a id="does-the-project-need-to-be-built-for-scanning"></a>

### 是否需要构建项目来进行扫描？

不会。Codex Security 可以从仓库和提交上下文中生成结果，而无需编译步骤。在自动验证期间，如果这有助于重现问题，它可能会尝试在容器内构建项目。有关环境设置的详细信息，请参阅 [Codex 云环境](../environments/cloud-environment.zh-CN.md)。

<a id="how-does-codex-security-reduce-false-positives-and-avoid-broken-patches"></a>

### Codex Security如何减少误报并避免损坏的补丁？

Codex Security 使用两级。首先，该模型对可能的问题进行排名。然后自动验证尝试在干净的容器中重现每个问题。成功重现的结果被标记为已验证，这有助于减少人工审查之前的误报。

<a id="how-long-do-initial-scans-take-and-what-happens-after-that"></a>

### 初始扫描需要多长时间，之后会发生什么？

初始扫描时间取决于仓库大小、构建时间以及需要验证的结果数量。对于某些仓库，扫描可能需要几个小时。对于较大的仓库，可能需要几天的时间。稍后的扫描通常会更快，因为它们专注于新的提交和增量更改。

<a id="what-is-a-threat-model"></a>

### 什么是威胁模型？

威胁模型是仓库的扫描时安全上下文。它将简洁的项目概述与攻击面详细信息（例如入口点、信任边界、身份验证假设和风险组件）结合起来。有关更多详细信息，请参阅 [改进威胁模型](threat-model.zh-CN.md)。

<a id="how-is-a-threat-model-generated"></a>

### 威胁模型是如何生成的？

Codex Security 提示模型总结仓库架构和安全入口点，对仓库类型进行分类，运行专门的提取器，并将结果合并到整个扫描过程中使用的项目概述或威胁模型工件中。

<a id="does-it-replace-manual-security-review"></a>

### 它会取代手动安全审查吗？

不会。Codex Security 可以加速审查并帮助对结果进行排名，但它不会取代代码级验证、可利用性检查或人类威胁评估。

<a id="can-i-edit-the-threat-model"></a>

### 我可以编辑威胁模型吗？

是的。 Codex Security 创建初始威胁模型，您可以随着架构、风险和业务环境的变化来更新它。有关编辑工作流程，请参阅 [改进威胁模型](threat-model.zh-CN.md)。

<a id="do-i-need-to-configure-a-scan-before-using-threat-modeling"></a>

### 在使用威胁建模之前是否需要配置扫描？

是的。威胁模型指南与您扫描的方式和内容相关，因此您需要首先配置仓库。参见 [Codex Security 设置](setup.zh-CN.md)。

<a id="what-does-the-proposed-patch-contain"></a>

### 提议的补丁包含什么内容？

当可以针对发现的问题生成修复时，建议的补丁包含带有文件名和行上下文的最小可操作差异。

<a id="does-the-patch-directly-modify-my-pr-branch"></a>

### 补丁会直接修改我的PR分支吗？

不会。工作流程会生成差异、补丁文件或建议的更改，供维护人员和审阅人员在应用之前进行检查。

<a id="validation"></a>

## 验证

<a id="what-is-auto-validation"></a>

### 什么是自动验证？

自动验证是尝试在隔离容器中重现可疑问题的阶段。它记录复制是否成功或失败，并捕获日志、命令和相关工件作为证据。

<a id="what-happens-if-validation-fails"></a>

### 如果验证失败会发生什么？

该发现尚未得到证实。日志和报告仍然捕获所尝试的内容，以便工程师可以重试、进一步调查或调整再现步骤。

{/* vale Microsoft.Auto = YES */}
{/* vale Vale.Spelling = YES */}