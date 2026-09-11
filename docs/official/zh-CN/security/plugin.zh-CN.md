> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/plugin.md)。

<a id="codex-security-plugin-quickstart"></a>

# Security 插件快速开始

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex Security 扫描您的代码是否存在漏洞并验证合理的发现。对于每个可报告的问题，它都会为您提供审查结果所需的证据和补救指导。仅扫描您拥有或有权评估的代码。

按照此快速入门安装插件并对 Codex 中的本地仓库运行标准只读扫描。

本页面介绍桌面应用程序或 Codex CLI 中的 Codex Security 插件。要扫描 Codex 云中连接的 GitHub 仓库，请参阅 [Codex Security 云设置](setup.zh-CN.md)。

<a id="install-the-plugin"></a>

## 安装插件

<ContentModeSwitch group="codex-surface" id="app">

1. 打开[ChatGPT 桌面应用程序中的 Codex](../app.zh-CN.md)。
2. 打开**插件**，搜索**Codex Security**，或使用以下按钮：

   

     <ButtonLink
       href="codex://plugins/install/codex-security?marketplace=openai-curated"
       color="primary"
       variant="solid"
       size="lg"
       pill
     >
安装Codex Security插件
     </ButtonLink>
   


3. 确认插件已启用，然后在侧边栏中打开 **安全性**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

1. 在您的终端中，转到要评估的仓库并启动 Codex：

```bash
   codex
```

2. 输入“`/plugins`”，搜索“**Codex Security**”，选择“**安装插件**”。
3. 输入 `/new` 为仓库启动新的聊天。

</ContentModeSwitch>



在依赖某个功能或开始长时间运行的扫描之前，请检查 [插件变更日志](plugin/changelog.zh-CN.md)。如果 **安全性** 未出现在桌面应用程序侧栏中，请更新应用程序和插件并确认插件已启用。

<a id="run-your-first-scan"></a>

## 运行您的第一次扫描

为了获得最佳扫描质量，请使用 `gpt-5.6-sol` 和 `xhigh` 推理工作。

<ContentModeSwitch group="codex-surface" id="app">

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="本机 Codex Security 工作台在仓库扫描开始之前显示新的扫描设置"
    lightSrc={scanOverview.src}
    darkSrc={scanOverviewDark.src}
    maxHeight="520px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
在启动之前选择一个仓库并配置新的安全扫描。
  </figcaption>
</figure>

<WorkflowSteps variant="headings">

1. 打开扫描设置

在侧边栏中选择“**安全性**”，打开“**扫描**”，然后选择“**+ 扫描**”。

2. 选择代码库和扫描区域

选择现有仓库或使用其他文件夹。选择 **代码库**，关闭 **深度扫描**，然后选择整个仓库或一个文件夹。确认分支和修订版本标识您要扫描的代码。

3. 添加相关上下文

选择模型和推理工作。仅当您需要描述应指导审查的特定攻击媒介、安全敏感区域或仓库详细信息时，才打开 **额外的背景信息**。

   <figure className="not-prose my-6">
     <CodexScreenshot
       alt="本机 Codex Security 扫描设置，具有启用的附加上下文以及示例攻击向量、重点区域和安全指南"
       lightSrc={scanSetup.src}
       darkSrc={scanSetupDark.src}
       maxHeight="460px"
     />
     <figcaption className="mt-3 text-sm text-secondary">
打开附加上下文来描述攻击向量、重点领域和相关安全指南。
     </figcaption>
   </figure>

4. 开始扫描

选择 **开始扫描** 并按照安全工作台中的扫描阶段进行操作。选择 **查看活动** 以检查执行扫描的 Codex 任务。

5. 查看结果

打开已完成的扫描以检查结果、覆盖范围和可用的报告工件。使用 **研究结果** 查看扫描中的问题，或使用 **仓库** 检查仓库的扫描历史记录。

   <figure className="not-prose my-6">
     <CodexScreenshot
       alt="已完成的 Codex Security 扫描显示本机工作台中的发现"
       lightSrc={findingsWorkspace.src}
       darkSrc={findingsWorkspaceDark.src}
       maxHeight="520px"
     />
     <figcaption className="mt-3 text-sm text-secondary">
在安全工作台中查看扫描结果、发现结果和覆盖范围。
     </figcaption>
   </figure>

</WorkflowSteps>

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<WorkflowSteps variant="headings">

1. 要求进行标准扫描

在新聊天中发送此提示：

```text
   在此仓库上运行 Codex Security 扫描。
```

2. 让扫描完成

Codex 在终端中运行扫描，无需打开设置工作区。保持任务运行，直到 Codex 报告任务已完成。如果 Codex 发现配置限制，请在批准配置更新之前查看该限制和具体建议的更改。

3. 查看结果

查看终端中的摘要，然后打开生成的 `report.md` 以获取完整结果。

</WorkflowSteps>

</ContentModeSwitch>



<a id="what-the-scan-creates"></a>

## 扫描创建什么

<ContentModeSwitch group="codex-surface" id="app">

已完成的扫描仍可在 **扫描** 中使用。在安全工作台中查看他们的发现和覆盖范围，或检查 **研究结果** 和 **仓库** 中的相关发现和仓库历史记录。扫描还会创建以下文件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

每次完成的扫描都会在终端中报告摘要并创建以下文件。

</ContentModeSwitch>



- `report.md`，扫描结果的主要可读入口点。
- `调查结果/<slug>/`，当详细的漏洞报告和支持概念验证文件可用时。
- `hardening/`，当结构加固指南和支持建议或图表可用时。
- `scan-manifest.json`、`findings.json` 和 `coverage.json` 中的结构化扫描数据用于自动化和集成。您无需打开这些文件即可查看扫描结果。

共享或存档结果时将完整扫描目录放在一起，以便 `report.md` 的链接继续有效。

<a id="choose-your-next-workflow"></a>

## 选择您的下一个工作流程

- [使用安全工作台](plugin/workbench.zh-CN.md) 用于管理桌面应用程序中保存的扫描、结果、仓库和扫描活动。
- [从 CLI 运行扫描](cli.zh-CN.md) 如果您有测试版访问权限并且需要具有结构化结果的可重复终端工作流程。
- [运行标准或范围扫描](plugin/scans.zh-CN.md) 使用默认工作流程查看仓库或一个文件夹。
- [评估第一次扫描](plugin/scans.zh-CN.md#assess-a-first-scan) 根据已知问题检查结果并决定何时再次扫描。
- 当您可以允许更长的运行时间时，[运行深度扫描](plugin/deep-scans.zh-CN.md) 可进行更彻底的扫描。
- [检查代码更改](plugin/code-changes.zh-CN.md) 用于评估拉取请求、提交、分支范围或工作树补丁。
- [对积压订单进行分类](plugin/triage-backlog.zh-CN.md) 审查现有的安全调查结果。
- [修复并验证发现的结果](plugin/fix-findings.zh-CN.md) 在您接受一项补救结果后。
- [导出或跟踪结果](plugin/export-findings.zh-CN.md) 用于创建 JSON、CSV、SARIF、经过批准的 Linear、GitHub 或 Jira 问题，或私人草稿 GitHub 安全建议。
- [撰写漏洞报告](plugin/vulnerability-reports.zh-CN.md) 将提供的调查结果、披露说明、来源和 PoC 转化为独立的报告。
- [提出安全强化建议](plugin/security-hardening.zh-CN.md) 根据扫描结果或其他安全证据考虑结构或架构选项。