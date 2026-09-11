> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/third-party/github.md)。

<a id="review-github-pull-requests-with-codex"></a>

# GitHub 拉取请求审查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Codex 代码审查来获得 GitHub 拉取请求的另一个高信号审查通过。 Codex 审查拉取请求差异，遵循仓库指南，并发布针对严重问题的标准 GitHub 代码审查。研究预览中提供的安全审查可对拉取请求中的潜在安全问题进行更深入的审查。



[观看：Codex 代码审查演练](https://www.youtube.com/watch?v=HwbSWVg5Ln4)




<a id="before-you-start"></a>

## 开始之前

确保您有：

- [Codex云](../cloud.zh-CN.md) 为您要查看的仓库设置。
- 访问 [Codex 代码审查设置](https://chatgpt.com/codex/settings/code-review)。
- 如果您希望 Codex 遵循仓库特定的审核指南，则需要 `AGENTS.md` 文件。

<a id="set-up-codex-code-review"></a>

## 设置 Codex 代码审查

要配置自动审核，您需要连接的 GitHub 仓库和 GitHub 推送或管理员权限对其设置。

1. 设置 [Codex云](../cloud.zh-CN.md)。
2. 转到 [Codex设置](https://chatgpt.com/codex/settings/code-review)。
3. 为您的仓库打开 **代码审查**。



  
    

> 插图：显示代码审查切换的 Codex 设置


  





<a id="request-a-codex-review"></a>

## 请求 Codex 审核

1. 在拉取请求评论中，提及 `@codex review`。
2. 等待 Codex 反应 (👀) 并发表评论。



  
    

> 插图：带有 @codex review 的拉取请求评论


  





Codex 发布了对拉取请求的评论，就像队友一样。在 GitHub 中，Codex 仅标记 P0 和 P1 问题，因此审核评论仍集中在高优先级风险上。



  
    

> 插图：针对拉取请求的 Codex 代码审查示例


  





<a id="enable-automatic-reviews"></a>

## 启用自动评论

如果您希望 Codex 自动审核每个拉取请求，请在 [Codex设置](https://chatgpt.com/codex/settings/code-review) 中启用 **自动评论**。每当有人打开新的 PR 进行审核时，Codex 都会发布评论，而不需要 `@codex review` 评论。

<a id="customize-what-codex-reviews"></a>

## 自定义 Codex 评论内容

Codex 在您的仓库中搜索 `AGENTS.md` 文件并遵循适用的代码审查规则。将 `## Code Review Rules` 部分添加到最接近规则管辖的代码的文件中。如果有帮助，请使用 `###` 标题对相关检查进行分组。

例如，实验报告服务可以防止暴露后行为改变比较队列：

```md
## 代码审查规则

### 实验队列

- 不要过滤暴露后行为的治疗比较，包括转化或保留。
  安全路径：通过分配或接触来建立队列；将转化报告为结果。
```

将仓库范围的规则放在根 `AGENTS.md` 中，将特定于服务的规则放在嵌套文件中，例如 `services/experiment_reporting/AGENTS.md`。 Codex 应用涵盖每个更改文件的根和更具体的指南，因此不相关的更改不必携带特定于服务的上下文。

从两到三个简明规则开始，这些规则对审阅者经常解释的检查进行编码。有用的规则：

- **专注于相应的、特定于仓库的行为。** 描述兼容性约束、数据边界或标记的不安全副作用及其重要性。
- **说明安全路径或例外情况。** 为 Codex 提供足够的上下文，以区分真实问题和预期行为。
- **保持规则的范围和持久性。** 更喜欢结果而不是可以更改的函数名称，并将指导放在其管辖的代码附近。
- **将机械检查留在 CI 中。** 将格式、lint 和其他确定性检查排除在审核规则之外。

打开代表拉取请求并请求 `@codex review` 进行审核。根据您看到的发现和反馈完善规则，并缩小或删除产生噪音的指导。

代码审查规则指南Codex；它们不会取代测试、分支保护或所需的批准。

对于一次性焦点，请将其添加到您的拉取请求评论中：

`@codex review for issues in the database migration`

<a id="security-review"></a>

## 安全审查

安全审查是针对希望特别关注拉取请求中的安全问题的客户的额外审查。通过分析拉取请求差异、支持仓库上下文以及配置的威胁模型或安全指南，它比代码审查更深入地分析特定于安全的风险。

代码审查还可以识别与安全相关的问题，作为其一般审查的一部分，因此您可能会发现代码审查和安全审查结果之间偶尔有重叠。

<a id="set-up-security-review"></a>

### 设置安全审查

有关更详细的设置说明和配置选项，请参阅 [安全审查](../security/security-review.zh-CN.md)。

1. 设置 [Codex云](../cloud.zh-CN.md)。
2. 转到 [Codex设置](https://chatgpt.com/codex/settings/code-review)。
3. 在 **仓库首选项** 下，选择哪些拉取请求接受安全审查以及何时运行。选择 **每当代码审查运行时** 将其与代码审查一起运行。

<a id="request-a-security-review"></a>

### 请求安全审查

要手动请求安全审查，请将此评论添加到拉取请求中：

`@codex security review`

Codex 在审查运行时做出反应，然后直接在拉取请求上发布安全发现。打开关联的 Codex 任务并选择 **安全报告** 选项卡以查看完整报告。

<a id="act-on-review-findings"></a>

## 根据审查结果采取行动

Codex 发表评论后，您可以通过留下另一条评论来要求其修复同一拉取请求中的问题：

```md
@codex 修复 P1 问题
```

Codex 以拉取请求作为上下文启动云聊天，并且可以在有权执行此操作时将修复推送回分支。

<a id="give-codex-other-tasks"></a>

## 给Codex其他任务

如果您在评论中提及 `@codex` 以及 `review` 以外的任何内容，则 Codex 使用您的拉取请求作为上下文启动 [云聊天](../cloud.zh-CN.md)。

```md
@codex 修复 CI 失败
```

<a id="troubleshoot-code-review"></a>

## 解决代码审查问题

如果 Codex 没有反应或发表评论：

- 确认您为 [Codex设置](https://chatgpt.com/codex/settings/code-review) 中的仓库打开了 **代码审查**。
- 确认拉取请求属于设置了 [Codex云](../cloud.zh-CN.md) 的仓库。
- 在拉取请求注释中使用确切的触发器 `@codex review`。
- 对于自动审核，请检查您是否打开了 **自动评论** 并且拉取请求事件是否与您的审核触发器设置匹配。