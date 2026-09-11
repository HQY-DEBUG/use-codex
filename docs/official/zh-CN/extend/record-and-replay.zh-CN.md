> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/extend/record-and-replay.md)。

<a id="record--replay"></a>

# 录制与回放

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Record & Replay 可在 macOS 上使用。计算机使用也必须可用且启用。

Record & Replay 可让您在 Mac 上演示工作流程并将其转变为可重复使用的技能。当工作流程重复、取决于您的偏好或者比在提示中描述更容易显示时，请使用它。

例如，您可以记录如何归档费用、预订停车位、创建正确配置的问题、发布视频或下载定期报告。 ChatGPT 或 Codex 可以将模式打包成一项技能，您可以通过计算机使用、浏览器操作、连接的插件或它们的组合再次使用该技能。

<a id="before-you-start"></a>

## 开始之前

选择一个您已经知道如何完成的工作流程。当步骤稳定且成功标准明确时，Record & Replay 效果最佳。

<a id="start-a-recording"></a>

## 开始录音

<WorkflowSteps>

1. 在 ChatGPT 桌面应用程序中，选择 ChatGPT 并在切换器中打开“工作”，或选择 Codex。然后打开**插件**。
2. 打开 **+** 菜单。
3. 选择**记录一个技能**。
4. 查看建议的提示，添加任何有用的上下文，然后提交。
5. 当聊天请求允许记录您的操作时，请在准备好演示工作流程后批准该请求。
6. 在 Mac 上执行工作流程。
7. 完成后，从菜单栏或叠加层停止录制，或告诉聊天人员您已完成。

</WorkflowSteps>

在录制过程中，ChatGPT 或 Codex 观察了解工作流程所需的操作和窗口内容。录音将持续进行，直到您停止为止。将录音重点放在您想要教授该技能的任务上。

停止录制后，ChatGPT 或 Codex 检查捕获的工作流程并起草技能。该技能解释了何时使用工作流程、需要哪些输入、要遵循哪些步骤以及如何验证结果。您还可以要求进一步完善。

<a id="replay-the-workflow"></a>

## 重播工作流程

开始新的 ChatGPT 或 Codex 聊天并要求其使用生成的技能。为其指定这次不同的值，例如要上传的文件、要创建的问题或报告的日期范围。

该产品使用该技能作为任务的可重用上下文。然后，它可以使用当前环境中可用的工具完成工作流程，包括计算机使用、浏览器操作和安装的插件。

<a id="tips-for-better-recordings"></a>

## 更好录音的技巧

- 保持演示简短而完整。
- 在开始录制之前，说明您的目标以及可能因技能使用而异的任何具体输入。
- 使用真实的输入，但避免秘密和敏感数据。
- 录制后完善技能，以找出重要的隐藏偏好，例如命名约定、字段默认值或决策点。
- 工作流程完成后停止记录，而不是继续进行不相关的清理。

<a id="when-to-build-another-plugin"></a>

## 何时构建另一个插件

Record & Replay 是一种从演示的工作流程创建技能的快速方法。如果您想在团队中分发单独的稳定包、捆绑多种技能、包括连接器、添加 MCP 服务器或管理安装元数据，请将该工作流程打包为自己的插件。参见 [构建插件](https://developers.openai.com/plugins/build/plugins)。

<a id="troubleshooting"></a>

## 故障排除

<a id="i-dont-see-record--replay"></a>

### 我没有看到 Record & Replay

如果您的组织使用 `requirements.toml` 管理 Codex，则 `[features].computer_use` 要求也控制 Record & Replay。设置 `computer_use = false` 会使这两个功能不可用。