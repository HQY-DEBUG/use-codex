> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/get-started-with-work.md)。

<a id="get-started-with-chatgpt-work"></a>

# ChatGPT Work 入门

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<VideoPlayer src="https://cdn.openai.com/devhub/superapp-video-v1.mp4" />

<a id="introducing-work-mode"></a>

<a id="introducing-chatgpt-work"></a>

## ChatGPT Work介绍

ChatGPT Work 是一种将实际工作委托给 ChatGPT 的方法。

当您需要答案、解释、头脑风暴或草稿时，请使用“聊天”。当您希望 ChatGPT 完成具有明确结果的任务时，请使用 ChatGPT Work，例如摘要、幻灯片、分析、定期更新、工作流程或您可以查看和使用的文件。了解有关 [一起使用 Chat 和 ChatGPT Work](use-chatgpt.zh-CN.md) 的更多信息。

ChatGPT Work 可以使用您的文件、插件和批准的工具来检索信息、创建完成的文件、运行工作流程并完成可供您查看的工作。您可以跟踪进度、回答问题、改变方向以及批准重要行动。

在 [桌面应用程序](app.zh-CN.md) 上，ChatGPT Work 还可以使用本地文件、应用程序和浏览器（如果这些工具可用）。

如果您已使用 Codex 进行非编码工作，则可以保留 Codex 或使用 ChatGPT Work 代替。 ChatGPT Work 为您提供相同的核心功能以及专为日常工作设计的体验。

<a id="what-to-try-first"></a>

## 首先尝试什么

<VideoPlayer src="https://cdn.openai.com/devhub/videos-learn/selectnoonboarding.mp4" />

首先，切换到**工作**。然后选择你的第一个任务。好的任务有明确的结果、一些源材料以及可以查看的输出。

<a id="choose-local-or-cloud-work"></a>

### 选择本地或云端工作

在桌面应用程序中，打开标记为 **本地工作** 的 Composer 控件。如果 **云** 作为选项出现，当您希望 ChatGPT Work 在关闭应用程序或关闭计算机后继续运行时，或者当您想从网络或移动应用程序继续聊天时，请选择它。当任务需要计算机上的文件或应用程序时，请保持选中 **本地工作**。

云对于随着时间的推移研究或检查网站的计划任务也很有用，因为它们的运行不依赖于您的计算机是否处于唤醒状态。

以下是您可以开始使用的三个常见用例：

<a id="create-a-presentation"></a>

### 创建演示文稿

使用 ChatGPT Work 将笔记、文档、研究或会议材料转变为结构化的幻灯片。


  

> 插图：在ChatGPT Work中创建的演示文稿






**提示示例：**

```text
查看随附的源材料并为 [audience] 创建八张幻灯片演示文稿。关注主题，包括支持证据，并标记任何需要人工审查的内容。返回草稿供我审阅。
```

<a id="create-a-comparison-spreadsheet"></a>

### 创建比较电子表格

使用 ChatGPT Work 将笔记、文件或研究转换为电子表格，比较选项并帮助您做出决定。


  

> 插图：在ChatGPT Work中创建的比较电子表格






**提示示例：**

```text
创建一个电子表格，比较 [decision] 的选项。使用随附的注释和源材料。包括最重要的标准，对每个选项进行评分，标记风险或缺失信息，并添加包含建议和后续步骤的摘要选项卡。
```

<a id="set-up-a-recurring-update"></a>

### 设置定期更新

当您希望 ChatGPT Work 随着时间的推移重复、监视或刷新某些内容时，请使用计划任务。


  

> 插图：ChatGPT Work 中安排的定期更新






**提示示例：**

```text
每个星期一早上，查看来自 @Slack 和 @Google Drive 的 [project] 的新更新。更新会议议程，包括决策、阻碍因素、负责人和未解决的问题。在分享之前给我发送一份草稿。
```

了解有关 [计划任务](automations.zh-CN.md) 的更多信息。

<a id="best-practices-for-using-work"></a>
<a id="best-practices-for-using-work-mode"></a>

<a id="best-practices-for-using-chatgpt-work"></a>

## 使用 ChatGPT Work 的最佳实践

当您希望 ChatGPT 完成任务、创建文件或管理工作时，请使用 ChatGPT Work。它非常适合以下任务：

- 使用多个来源、插件、工具或步骤。
- 手动完成将花费有意义的时间。
- 生成您将查看、编辑或重复使用的输出。
- 需要随着时间的推移进行重复、监控或更新。

为了获得更好的结果，请告诉 ChatGPT 您需要的结果、要使用的源或插件、要遵循的任何限制、好的外观以及何时停止审查或批准。

**而不是：** 让我介绍一下我们的客户研究。



**提示示例：**

```text
查看随附的访谈记录和调查结果。为产品领导会议创建八张幻灯片的演示文稿。重点关注三个最常见的客户问题，包括支持证据、将调查结果与建议分开，并标记任何没有得到充分支持的主张。使用 @Google Drive 作为源文档。在将其视为最终版本之前，请返回草稿供我审阅。
```

了解有关 [提示 ChatGPT Work](prompting.zh-CN.md#prompting-for-work) 的更多信息。

<a id="add-plugins-for-more-context-and-better-outputs"></a>

## 添加插件以获得更多上下文和更好的输出


  

> 图解：ChatGPT Work 中的插件库




插件将 ChatGPT Work 连接到您的团队使用的工具，例如 Slack、Google Drive、SharePoint、电子邮件、日历、客户关系管理系统和项目跟踪器。

- 在左侧边栏中选择 **插件** 以查看插件库。
- 安装与您的工作最相关的插件。
- 要将 ChatGPT 指向特定工具，请在提示符中键入 `@` 和插件名称。

了解有关 [插件](plugins.zh-CN.md) 的更多信息。

<a id="use-work-mode-efficiently"></a>

<a id="use-chatgpt-work-efficiently"></a>

## 高效使用ChatGPT Work

选择 [GPT-6 阿斯特拉](models.zh-CN.md#gpt-6-astra) 来完成需要仔细推理、视觉判断或完善的最终文件的高要求工作。对于更简单的任务，请考虑 Sol、Terra 或 Luna。从模型选择器中可用的模型中进行选择，并在开始大型任务之前检查 [计划使用](pricing.zh-CN.md)。

ChatGPT Work 最适合涉及多个步骤、来源或工具或需要完整交付成果的实质性任务。更长或更复杂的任务可能会使用更多额度，因为 ChatGPT 会代表您做更多事情。关注完成结果的价值，而不是提示的数量。

通过设定有用的界限来保持任务的重点。例如：“仅使用这些来源”、“比较前五个选项”或“在发送任何内容之前停止”。

如果您只需要建议，请使用聊天来快速提问、简短重写和做出决定。

了解有关 [高效工作](prompting.zh-CN.md#prompting-for-work) 的更多信息。

如果任务因安全审查而暂停，请遵循通知并在继续之前审查任何可用的结果。参见 [安全监控和暂停任务](agent-approvals-security.zh-CN.md#safety-monitoring-and-paused-tasks)。

<a id="more-use-cases"></a>

## 更多用例

探索适用于常见团队和任务的实用 ChatGPT Work 工作流程。

<CodexCollectionList
  slugs={[
    "productivity-and-collaboration",
    "business-operations",
    "data-science",
    "finance",
    "sales",
    "life-sciences",
    "education",
  ]}
/>