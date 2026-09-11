> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/automations.md)。

<a id="scheduled-tasks"></a>

# 定时任务

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

安排重复任务在后台运行。在 ChatGPT Web 和移动设备上，符合条件的计划还可以从支持的应用程序事件运行任务。在 **预定** 中查看活动、暂停和已完成的任务以及最近的运行。您可以将计划任务与 [技能](build-skills.zh-CN.md) 结合起来进行更复杂的工作。



[观看：使用 ChatGPT 安排任务](https://www.youtube.com/watch?v=CToxp125mhc)

<ContentModeSwitch group="codex-surface" id="app">

在ChatGPT桌面应用程序中，计划任务可以与本地项目配合使用，并在项目目录或隔离的工作树中运行。当计划任务需要本地文件时，保持计算机开启并运行应用程序。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

为您的工作区启用计划任务后，可以从 Chat 或 Web 上的 ChatGPT Work 创建它们，并从 **预定** 管理它们的运行。 Web 任务可以使用上传的上下文和连接的工具，但它们无法直接在计算机上的文件夹中运行。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

Codex CLI不提供定时管理界面。使用 ChatGPT Web 或桌面应用程序创建和管理计划任务。 CLI 可以帮助您首先准备和测试提示、技能或脚本。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

IDE 扩展不提供计划管理界面。使用 ChatGPT Web 或桌面应用程序创建和管理计划任务。 IDE 扩展可以帮助您首先准备和测试提示、技能或工作区更改。

</ContentModeSwitch>

<a id="managing-tasks"></a>
<a id="ask-codex-to-create-or-update-automations"></a>
<a id="ask-chatgpt-to-create-or-update-scheduled-tasks"></a>
<a id="thread-automations"></a>
<a id="scheduled-tasks-in-threads"></a>
<a id="scheduled-tasks-in-chats"></a>
<a id="schedule-work-from-a-task"></a>
<a id="schedule-a-task-inside-a-chat"></a>
<a id="test-automations"></a>
<a id="test-scheduled-tasks"></a>
<a id="worktree-cleanup-for-automations"></a>
<a id="worktree-cleanup-for-scheduled-tasks"></a>
<a id="permissions-and-security-model"></a>
<a id="examples"></a>
<a id="automatically-create-new-skills"></a>
<a id="stay-up-to-date-with-your-project"></a>
<a id="combining-automations-with-skills-to-fix-your-own-bugs"></a>
<a id="combining-scheduled-tasks-with-skills-to-fix-your-own-bugs"></a>

<ContentModeSwitch group="codex-surface" id="web">

<a id="manage-scheduled-tasks-on-the-web"></a>

## 在 Web 上管理计划任务

打开 **预定** 查看任务状态和最近运行。当每次运行都应从保存的提示开始时，请使用独立的计划任务。当您希望 ChatGPT 返回到具有现有上下文的同一聊天时，请在聊天中使用计划任务。

网络上的计划任务可以使用该聊天可用的上传文件、连接的工具、技能和插件。他们不会在运行之间保留可用的本地文件夹或工作树。将持久的说明放入任务提示或附加技能中，并将所需的源材料保存在可访问的项目、上传或连接的服务中。

在安排任务之前，请在常规网络聊天中测试其提示。检查前几次运行，如果结果太宽泛或需要额外的上下文，则调整提示、工具或节奏。

<a id="trigger-tasks-from-app-events"></a>

## 从应用程序事件触发任务

在符合条件的计划中，计划任务可以在发生受支持的 Gmail、Slack 或 GitHub 事件时运行。事件触发任务可在 Web 和移动设备上的 ChatGPT 中使用。它们在 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中不可用。

要求 ChatGPT 创建任务，然后描述要监视的事件以及发生时要执行的操作。触发器决定任务何时运行；保存的提示决定每次运行的作用。一项任务可以使用多个事件触发器，但不能将事件触发器与基于时间的计划结合起来。

支持的事件触发器包括：

- **邮箱：** 新传入消息，可以选择按发件人或主题进行过滤。
- **Slack：** 选定频道中的新消息，可以选择按作者过滤以及是否包含线程回复。不支持反应、编辑、删除和直接消息。
- **GitHub：** 仓库中的拉取请求活动。按拉取请求、作者、标题或标签进行过滤，并选择是评论、评论、提交更新还是仅合并应触发任务。

在创建任务之前连接并授权应用程序。对于 Slack，将 `@ChatGPT` 添加到任务监视的每个通道。对于 GitHub，连接的应用程序必须有权访问仓库。

当多个匹配事件同时到达时，ChatGPT 可能会将它们组合在一次运行中。打开 **预定** 以查看待处理事件或选择 **立即运行** 来处理它们。

可用性取决于您的计划和工作区设置。在托管工作区中，管理员可以使用 **允许事件触发的计划任务** 权限控制访问。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

例如，安排任务来评估遥测错误并提交修复，或创建有关最近代码库更改的报告。对于应继续使用相同上下文的正在进行的工作，[在现有聊天中安排任务](#schedule-a-task-inside-a-chat)。

对于项目范围内的计划任务，请保持计算机开机且 ChatGPT 桌面应用程序运行。当任务计划运行时，所选项目必须仍然在磁盘上可用。

在 Git 仓库中，您可以选择计划任务是在本地项目中运行还是在新的 [工作树](environments/git-worktrees.zh-CN.md) 上运行。两个选项都在后台运行。工作树将计划任务的更改与未完成的本地工作分开，而在本地项目中运行可以修改您仍在处理的文件。在非版本控制的项目中，计划任务直接在项目目录中运行。

您还可以将模型和推理工作保留为默认设置，或者如果您希望更好地控制计划任务的运行方式，则明确选择它们。

如果计划任务使用`gpt-5.4`或者`gpt-5.4-mini`和ChatGPT登录，在这些模型于 2026 年 8 月 31 日退役之前更新它。替换`gpt-5.4`和`gpt-5.6-terra`和`gpt-5.4-mini`和`gpt-5.6-luna`.



> 插图：ChatGPT 输入框已准备好创建计划任务，并选择了 5.6 Sol Extended。



计划任务使用默认沙箱设置在无人值守的情况下运行。从让任务成功的最窄访问权限开始，仅在需要时授予网络或更广泛的文件访问权限。 [了解沙箱](sandboxing.zh-CN.md)。

<a id="manage-scheduled-tasks"></a>

## 管理计划任务

在 ChatGPT 桌面应用程序侧栏中查找 **预定** 上的所有计划任务及其运行。

**预定** 视图充当您的收件箱。计划任务运行的结果会显示在那里，未读指示器会显示运行何时需要您的注意。



> 插图：计划任务页面，包含“全部”、“活动”和“暂停”过滤器以及三个计划任务。



独立计划任务为每个计划运行启动一个新的聊天，并在 **预定** 中报告结果。当每次运行应该独立时或者当一项计划任务应该跨一个或多个项目运行时，请使用它们。如果您需要自定义节奏，请使用自定义时间表控件。对于高级计划，请编辑其 RFC 5545 重复规则 (RRULE)，例如 `RRULE:FREQ=MONTHLY;BYMONTHDAY=1;BYHOUR=9;BYMINUTE=0`。

对于 Git 仓库，每个计划任务都可以在本地项目中运行，也可以在专用后台 [工作树](environments/git-worktrees.zh-CN.md) 上运行。当您想要将计划任务更改与未完成的本地工作隔离开时，请使用工作树。当您希望计划任务直接在主结帐中工作时，请使用本地模式，请记住它可能会更改您正在编辑的文件。在非版本控制的项目中，计划任务直接在项目目录中运行。您可以在多个项目上运行相同的计划任务。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,web">

在 Web 上使用 ChatGPT Work 创建的计划任务，或者在桌面应用程序中使用 ChatGPT Work 或 Codex 创建的计划任务可以使用插件。计划任务也可以使用技能。为了保持计划任务的可维护性和跨团队共享性，请使用 [技能](build-skills.zh-CN.md) 定义操作并提供工具和上下文。当工作流不应依赖于自动工具选择时，在任务提示中选择或调用特定技能。

<a id="ask-chatgpt-to-create-or-update-scheduled-tasks"></a>

## 要求ChatGPT创建或更新计划任务

您可以从 ChatGPT 或 Codex 聊天创建和更新计划任务。描述工作、何时运行以及每次运行是否应返回当前聊天或开始新的聊天。 ChatGPT 可以起草提示、选择正确的目标，并在任务范围或节奏发生变化时更新任务。

例如，要求 ChatGPT 在部署完成时安排当前聊天的后续行动，或者要求其创建一个独立的计划任务来定期检查项目。

技能还可以创建或更新计划任务。例如，照顾拉取请求的技能可以设置一个计划任务，使用 GitHub 插件检查 PR 状态并修复新的评论反馈。

<a id="schedule-a-task-inside-a-chat"></a>

## 在聊天中安排任务

当您希望 ChatGPT 按计划返回该聊天时，在现有聊天中安排任务。计划任务使用聊天的现有上下文，而不是每次都从新提示开始。

聊天中的计划任务可以使用基于分钟的间隔进行主动后续循环，或者当您需要在特定时间签到时使用每日和每周计划。

在聊天中安排任务：

- 检查长时间运行的操作直至其完成
- 当您需要定期快照而不是对一个支持的应用程序事件的响应时，以固定的节奏检查连接的源
- 提醒 ChatGPT 以固定节奏继续复习循环
- 运行使用插件的技能驱动工作流程，例如检查 PR 状态和处理新反馈
- 继续正在进行的研究或分类聊天而不丢失其上下文

当每次运行应该独立或当结果应该在 **预定** 中显示为单独的运行时，请使用独立计划任务。

当您在聊天中安排任务时，请使提示持久。它应该描述 ChatGPT 在每次计划的运行中应该做什么，如何决定是否有重要的报告，以及何时停止或要求您提供输入。

<a id="test-scheduled-tasks"></a>

## 测试计划任务

在安排任务之前，请先在常规聊天中手动测试提示。这可以帮助您确认：

- 提示清晰且范围正确。
- 所选或默认模型、推理工作和工具的行为符合预期。
- 结果输出是可审查的。

当您开始安排运行时，请检查前几个输出并根据需要调整提示或节奏。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

在 ChatGPT 桌面应用程序中，您可以使用 `$skill-name` 在计划任务提示中显式触发技能。

<a id="worktree-cleanup-for-scheduled-tasks"></a>

## 计划任务的工作树清理

如果您为 Git 仓库选择工作树，频繁的计划可能会随着时间的推移创建许多工作树。存档您不再需要的计划运行，并避免固定运行，除非您打算保留其工作树。

<a id="permissions-and-security-model"></a>

## 权限和安全模型

计划任务在无人值守的情况下运行并使用默认沙箱设置。

有关这些边界的简单语言解释，请参阅 [沙箱概述](sandboxing.zh-CN.md)。有关文件系统和网络规则，请参阅 [权限](permissions.zh-CN.md)。

- 如果您的沙箱模式为 **只读**，则如果工具调用需要修改文件、访问网络或使用计算机上的应用程序，则会失败。考虑将沙箱设置更新为工作区写入。
- 如果您的沙箱模式为 **工作区写入**，则如果工具调用需要修改工作区之外的文件、访问网络或使用计算机上的应用程序，则会失败。您可以使用 [规则](agent-configuration/rules.zh-CN.md) 有选择地将命令列入沙箱外部运行。
- 如果您的沙箱模式是 **完全访问权限**，则后台计划任务的风险会较高，因为 ChatGPT 可能会在不询问的情况下更改文件、运行命令和访问网络。考虑将沙箱设置更新为工作区写入，并使用 [规则](agent-configuration/rules.zh-CN.md) 有选择地定义智能体可以以完全访问权限运行哪些命令。

如果您处于托管环境中，管理员可以使用管理员强制要求来限制这些行为。例如，他们可以禁止 `approval_policy = "never"` 或限制允许的沙箱模式。参见 [管理员强制要求 (`requirements.toml`)](enterprise/managed-configuration.zh-CN.md#admin-enforced-requirements-requirementstoml)。

当您的组织策略允许时，计划任务使用 `approval_policy = "never"`。如果管理员要求不允许 `approval_policy = "never"`，计划任务将回退到您所选权限模式的批准行为。

<a id="examples"></a>

## 示例

<a id="automatically-create-new-skills"></a>

### 自动创造新技能

```markdown
扫描过去一天的所有 `~/.codex/sessions` 文件，如果使用特定技能时出现任何问题，请更新技能以使其更有帮助。仅个人技能，无回购技能。

如果有什么事情是我们经常做并且遇到困难的，我们应该把它作为一项技能保存起来，以加快未来的工作，那就去做吧。

绝对不觉得您需要更新任何内容 - 除非有充分的理由！

如果你做了的话请告诉我。
```

<a id="stay-up-to-date-with-your-project"></a>

### 及时了解您的项目

```markdown
查看最新的远程 origin/master 或 origin/main 。然后为最近 24 小时涉及 <DIRECTORY> 的提交生成一份执行简报

格式+结构：

- 使用丰富的 Markdown（H1 工作流部分、副标题斜体、根据需要的水平规则）。
- 序言可以是这样的内容：“这是 <directory> 的最后 24 小时简报：”
- 副标题应为：“与所有者一起进行叙述性演练；按工作流分组。”
- 按工作流分组而不是列出每个提交。工作流标题应为 H1。
- 每个工作流写一个简短的叙述，用通俗易懂的语言解释这些变化。
- 使用项目符号和粗体可以使内容更具可读性
- 随意为每个人制作子弹，但要加粗他们的名字

内容要求：

- 包含内联 PR 链接（例如 [#123](...)），不带“PRs：”标签。
- 不要包含提交哈希或“密钥提交”部分。
- 如果一个工作流下出现多个 PR 也没关系，但要避免每次提交项目符号列表。

范围规则：

- 仅包含当前 cwd（或主要结帐等效项）内的更改
- 仅包含最后 24 小时的提交。
- 使用 `gh` 获取 PR 标题和描述（如果有帮助）。
  也可以随意拉 PR 评论和评论
```

<a id="combining-scheduled-tasks-with-skills-to-fix-your-own-bugs"></a>

### 将计划任务与技能结合起来修复自己的bug

创建一项新技能，尝试通过创建新的 `$recent-code-bugfix` 和 [将其存储在您的个人技能中](build-skills.zh-CN.md#where-to-save-skills) 来修复由您自己的提交引入的错误。

```markdown
---
名称：最近代码错误修复
描述：查找并修复当前作者上周在当前工作目录中引入的错误。当用户希望从最近的更改中主动修复错误时，当提示为空时，或者当要求分类/修复由最近提交引起的问题时使用。根本原因必须直接映射到作者自己的更改。
---

# 最近的代码错误修复

## Overview

找到当前作者上周引入的错误，实施修复，并在可能的情况下进行验证。在当前工作目录中操作，假设代码是本地的，并确保根本原因与作者自己的编辑直接相关。

## Workflow

### 1) 建立最近更改范围

使用 Git 识别作者以及上周更改的文件。

- 从`git config user.name`/`user.email`中确定作者。如果不可用，请使用环境中的当前用户名或询问一次。
- 使用 `git log --since=1.week --author=<author>` 列出最近的提交和文件。重点关注这些提交所涉及的文件。
- 如果用户的提示为空，则直接使用此默认范围继续。

### 2) 查找与最近更改相关的具体故障

优先考虑直接归因于作者编辑的缺陷。

- 如果日志或 CI 输出在本地可用，则查找最近的故障（测试、lint、运行时错误）。
- 如果未提供失败信息，请运行涉及已编辑文件的最小相关验证（单个测试、文件级 lint 或目标重现）。
- 确认根本原因与作者的更改直接相关，而不是不相关的遗留问题。如果仅发现不相关的故障，则停止并报告未检测到合格的错误。

### 3) 实施修复

进行符合项目惯例的最小修复。

- 仅更新解决问题所需的文件。
- 避免添加额外的防御检查或不相关的重构。
- 保持更改与本地风格和测试一致。

### 4) Verify

尽可能尝试验证。

- 优先选择最小的验证步骤（有针对性的测试、集中 lint 或直接重现命令）。
- 如果无法运行验证，请说明将运行什么以及未执行的原因。

### 5) Report

总结根本原因、修复方法以及执行的验证。明确根本原因与作者最近的更改之间的关系。
```

然后，创建一个新的计划任务：

```markdown
检查我过去 24 小时的提交并提交 $recent-code-bugfix。
```

</ContentModeSwitch>