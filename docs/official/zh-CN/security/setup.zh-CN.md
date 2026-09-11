> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/setup.md)。

<a id="codex-security-cloud-setup"></a>

# Security 云端设置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

此页面将引导您从初始访问到 Codex Security 云中已审核的结果和修复拉取请求。

首先确认您已设置 Codex 云。如果没有，请参阅 [Codex云](../cloud.zh-CN.md) 开始。

<a id="1-access-and-environment"></a>

## 1. 交通及环境

Codex Security 云扫描通过 [Codex云](../cloud.zh-CN.md) 连接的 GitHub 仓库。

- 确认您的工作区可以访问 Codex Security 云。
- 确认您要扫描的仓库在 Codex 云中可用。

进入[Codex 环境](https://chatgpt.com/codex/settings/environments)，检查仓库是否已经有环境。如果没有，请在继续之前创建一个。

<CtaPillLink
  href="https://chatgpt.com/codex/settings/environments"
  label="开放环境"
  icon="external"
  class="my-8"
/>



  
    

> 插图：Codex 环境


  



<a id="2-new-security-scan"></a>

## 2.新的安全扫描

环境存在后，进入[创建安全扫描](https://chatgpt.com/codex/security/scans/new)，选择刚刚连接的仓库。

<CtaPillLink
  href="https://chatgpt.com/codex/security/scans/new"
  label="创建安全扫描"
  icon="external"
  class="my-8"
/>

Codex Security 首先从最新提交向后扫描仓库。当新提交进入时，它使用它来构建和刷新扫描上下文。

配置仓库：

1. 选择 GitHub 组织。
2. 选择仓库。
3. 选择您要扫描的分支。
4. 选择环境。
5. 选择 **历史窗口**。较长的窗口提供更多上下文，但回填需要更长的时间。
6. 单击 **创建**。



  
    

> 插图：创建安全扫描


  



<a id="3-initial-scans-can-take-a-while"></a>

## 3. 初始扫描可能需要一段时间

当您创建扫描时，Codex Security 首先在选定的历史窗口中运行提交级安全传递。初始回填可能需要几个小时，特别是对于较大的仓库或较长的窗口。如果结果不能立即显现，这是预料之中的。等待初始扫描完成，然后再开票或进行故障排除。

初始扫描设置是自动且彻底的。这可能需要几个小时。如果第一组调查结果延迟，请不要惊慌。

<a id="4-review-scans-and-improve-the-threat-model"></a>

## 4.审查扫描并改进威胁模型

<CtaPillLink
  href="https://chatgpt.com/codex/security/scans"
  label="检查扫描结果"
  icon="external"
  class="my-8"
/>



  
    

> 插图：Codex Security 中的威胁模型编辑器


  



初始扫描完成后，打开扫描并查看生成的威胁模型。出现初步结果后，更新威胁模型，使其与您的架构、信任边界和业务环境相匹配。这有助于 Codex Security 对您的团队的问题进行排名。

如果您希望更改扫描结果，您可以使用更新的范围、优先级和假设来编辑威胁模型。

出现初步结果后，重新访问模型，以便扫描指导与当前优先事项保持一致。保持最新有助于 Codex Security 产生更好的建议。

有关威胁模型及其如何影响关键性和分类的更深入说明，请参阅 [改进威胁模型](threat-model.zh-CN.md)。

<a id="5-review-findings-and-patch"></a>

## 5. 审查结果并修补

初始回填完成后，查看 **研究结果** 视图中的结果。

<CtaPillLink
  href="https://chatgpt.com/codex/security/findings"
  label="开放调查结果"
  icon="external"
  class="my-8"
/>

您可以使用两个视图：

- **推荐结果**：仓库中不断变化的十大最关键问题列表
- **所有调查结果**：仓库中可排序、可过滤的结果表


  

> 插图：推荐的调查结果视图




单击结果可打开其详细信息页面，其中包括：

- 问题的简要描述
- 关键元数据，例如提交详细信息和文件路径
- 关于影响力的情境推理
- 相关代码摘录
- 调用路径或数据流上下文（如果可用）
- 验证步骤和验证输出

您可以查看每个发现并直接从发现详细信息页面创建 PR。

<CtaPillLink
  href="https://chatgpt.com/codex/security/findings"
  label="审查调查结果并创建 PR"
  icon="external"
  class="my-8"
/>

<a id="related-docs"></a>

## 相关文档

- [Codex Security 概览](../security.zh-CN.md) 给出了产品概述。
- [Codex Security云常见问题解答](faq.zh-CN.md) 涵盖常见的云问题。
- [改进威胁模型](threat-model.zh-CN.md) 解释了如何改进扫描上下文和查找优先级。