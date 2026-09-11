> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/long-running-work.md)。

<a id="long-running-work"></a>

# 长时间任务

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

对于可能需要多个步骤的工作，请为 ChatGPT 提供明确的结果、约束和完成的定义。将相关工作保留在同一个聊天中，以便 ChatGPT 可以使用相同的上下文来选择下一步并决定工作何时完成。

<ContentModeSwitch group="codex-surface" id="app">

在 ChatGPT 桌面应用程序中，输入 `/goal` 启动目标模式。进度行可让您在 ChatGPT 工作时暂停、恢复、编辑或清除目标。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

对于 ChatGPT Web 中托管的长期运行工作，请使用 ChatGPT Work 并将结果、约束和审核标准直接放入提示中。

继续在同一个网络聊天中添加上下文、更改约束或请求状态更新。当独立任务可以并行运行时，请使用单独的聊天，并避免向两个任务提供对同一连接源的写访问权限。对于相关工作，请将聊天记录和源文件放在 [项目](projects.zh-CN.md) 中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在交互式 Codex CLI 会话中，输入 `/goal` 以启动目标模式。继续同一会话来指导工作或请求状态更新。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

在 IDE 扩展聊天中，输入 `/goal` 以启动打开的工作区的目标模式。继续相同的聊天以在任务运行时引导任务。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">


  

> 插图：ChatGPT 桌面应用程序目标进度控件位于输入框上方




</ContentModeSwitch>

<a id="start-a-goal"></a>
<a id="define-what-done-means"></a>
<a id="steer-a-running-goal"></a>
<a id="run-goals-in-parallel"></a>
<a id="related-docs"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="start-a-goal"></a>

## 开始一个目标

在 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中键入 `/goal`。目标文本既成为任务的第一个提示又成为完成标准。

如果结果仍不清楚，请从 `/plan` 开始。请 ChatGPT 采访您，找出限制因素，并将结果转化为具有可衡量的成功标准的目标。然后用`/goal`开始细化目标。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,web,cli,ide">

<a id="define-what-done-means"></a>

## 定义完成的含义

编写一个目标，让 ChatGPT 验证自己的进度。应用时包括三件事：

| 目标元素 | 包含什么 |
| ---------------- | ----------------------------------------------------------------------------- |
| **结果** | 描述您想要的结果，而不仅仅是 ChatGPT 应该执行的活动。   |
| **约束条件** | 列出所需的工具、边界、兼容性需求或要避免的方法。 |
| **验证** | 添加测试、测量或审核标准以证明工作已完成。  |

例如：

```text
将此代码库从 JavaScript 迁移到 TypeScript。保留现有行为，
在没有显式 `any` 类型的严格模式下编译，并使完整的测试套件通过。
```

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="steer-a-running-goal"></a>

## 引导跑步目标

在 ChatGPT 桌面应用程序中，目标进度行显示在编辑器上方。用它来暂停或恢复工作、编辑目标或清除目标。您还可以在目标运行时发送后续消息以添加上下文或调整约束。

当您希望在不中断主聊天的情况下进行状态回顾或解释时，请使用旁聊。在预计会失去连接之前暂停目标，然后在准备好 ChatGPT 继续时恢复目标。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="steer-a-running-task"></a>

<a id="steer-running-work"></a>

## 引导跑步工作

继续在同一聊天中添加上下文、调整约束或要求状态回顾。当另一个任务可以独立运行时开始单独的聊天。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="steer-a-running-goal"></a>

## 引导跑步目标

在同一交互式会话中发送后续消息以添加上下文或调整约束。当您希望 Codex 在继续之前总结进度时，请要求进行状态回顾。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="steer-a-running-goal"></a>

## 引导跑步目标

继续在同一个 IDE 聊天中添加上下文、调整约束或请求状态回顾。在目标运行时保持工作区可用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

启动目标并不授予 ChatGPT 更广泛的访问权限。它保持相同的 [沙箱和审批政策](sandboxing.zh-CN.md) 并在需要决定时暂停。通过 [自动审批审核](sandboxing/auto-review.zh-CN.md)，单独的审核者可以评估符合条件的请求，而无需扩大这些边界。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="run-goals-in-parallel"></a>

## 并行运行目标

每个聊天都有自己的上下文、消息、结果和目标。同时运行聊天，但避免让两个聊天更改相同的文件。使用 [工作树](environments/git-worktrees.zh-CN.md) 为并行编码聊天提供单独的结帐。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

对于本地工作，请在设置中打开 **防止跑步时睡觉**，以便您的 Mac 保持唤醒状态。使用 [宠物](pets.zh-CN.md) 或 [系统通知](notifications.zh-CN.md) 查看聊天何时需要输入或准备好进行审核。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="related-docs"></a>

## 相关文档

- [项目和聊天](projects.zh-CN.md)
- [目标模式和提示](prompting.zh-CN.md#goal-mode)
- [Git 工作树](environments/git-worktrees.zh-CN.md)

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="related-docs"></a>

## 相关文档

- [项目和聊天](projects.zh-CN.md)
- [计划任务](automations.zh-CN.md)
- [沙箱和权限](sandboxing.zh-CN.md)

</ContentModeSwitch>