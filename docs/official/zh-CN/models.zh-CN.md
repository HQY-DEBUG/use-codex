> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/models.md)。

<a id="models"></a>

# 模型选择

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" id="app">



  


<a id="choose-a-model"></a>

## 选择模型

在 ChatGPT 桌面应用程序中，使用编译器下方的模型和推理控件来选择可用模型并调整其推理工作。

更高的推理努力可以改善复杂任务的结果，但需要更长的时间并使用更多的标记。从默认工作量开始，当任务需要更深入的规划或分析时增加工作量。

**超** 模式超越了单智能体运行。它使用 [分智能体](agent-configuration/subagents.zh-CN.md) 来加速复杂的工作，使其对于可以跨子智能体拆分的较大任务非常有用。

  

  <CodexModelSwitcher client:visible className="lg:mt-7" />



</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">



  


<a id="choose-a-model"></a>

## 选择模型

这些建议适用于网络上的 **ChatGPT Work**。使用输入框下方的模型和推理控件来选择可用模型并调整其推理工作。

更高的推理努力可以改善复杂任务的结果，但需要更长的时间并使用更多的标记。从默认工作量开始，当任务需要更深入的规划或分析时增加工作量。

**超** 模式超越了单智能体运行。它使用 [分智能体](agent-configuration/subagents.zh-CN.md) 来加速复杂的工作，使其对于可以跨子智能体拆分的较大任务非常有用。

  

  <CodexModelSwitcher client:visible className="lg:mt-7" />



</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">



  


<a id="choose-a-model"></a>

## 选择模型

在交互式 CLI 会话中，使用 `/model` 切换模型或调整推理工作。您还可以在使用 `--model` 或其 `-m` 别名启动 Codex 时选择模型：

```bash
codex --model gpt-5.6
```


相同的选项适用于非交互式运行。例如：

```bash
codex exec -m gpt-5.6 "Review the current changes"
```


更高的推理努力可以改善复杂任务的结果，但需要更长的时间并使用更多的标记。从默认工作量开始，当任务需要更深入的规划或分析时增加工作量。

**超** 模式超越了单智能体运行。它使用 [分智能体](agent-configuration/subagents.zh-CN.md) 来加速复杂的工作，使其对于可以跨子智能体拆分的较大任务非常有用。

  

  <CodexReasoningLevelTerminal
    client:load
    className="lg:mt-7 lg:justify-self-end"
  />



</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">



  


<a id="choose-a-model"></a>

## 选择模型

使用编辑器下方的模型切换器来选择可用的模型和推理工作。

更高的推理努力可以改善复杂任务的结果，但需要更长的时间并使用更多的标记。从默认工作量开始，当任务需要更深入的规划或分析时增加工作量。

**超** 模式超越了单智能体运行。它使用 [分智能体](agent-configuration/subagents.zh-CN.md) 来加速复杂的工作，使其对于可以跨子智能体拆分的较大任务非常有用。

  

  <CodexModelSwitcher client:visible forceDark className="lg:mt-7" />



</ContentModeSwitch>

<a id="recommended-models"></a>
<a id="other-models"></a>
<a id="deprecated-codex-models"></a>
<a id="configure-your-default-local-model"></a>
<a id="choose-a-model-for-cloud-tasks"></a>
<a id="gpt-6-astra"></a>

<ContentModeSwitch group="codex-surface" ids="app,web,cli,ide">

<a id="recommended-models"></a>

## 推荐模型

<a id="app-compare-models"></a>



  <ModelDetails
    client:load
    name="gpt-6-astra"
    slug="gpt-6-astra"
    imageLabel="Astra"
    wallpaperUrl="/images/api/models/gpt-6-astra-texture.webp"
    description="我们最有能力的模型，适用于跨代码、应用程序和研究的复杂工作，结合了先进的推理、计算机使用和更强的判断力。"
    data={{
      features: [
        {
          title: "能力",
          value: "",
          icons: [
            "openai.SparklesFilled",
            "openai.SparklesFilled",
            "openai.SparklesFilled",
            "openai.SparklesFilled",
            "openai.SparklesFilled",
          ],
        },
        {
          title: "速度",
          value: "",
          icons: ["openai.Flash", "openai.Flash"],
        },
        { title: "ChatGPT 桌面应用程序", value: true },
        { title: "ChatGPT 网页", value: true },
        { title: "Codex CLI", value: true },
        { title: "Codex IDE扩展", value: true },
        { title: "Codex云", value: false },
        { title: "ChatGPT 制作人员", value: true },
        { title: "API访问", value: true },
      ],
    }}
  />

<ModelDetails
  client:load
  name="gpt-5.6-sol"
  slug="gpt-5.6-sol"
  imageLabel="5.6 Sol"
  wallpaperUrl="/images/api/models/gpt-5.6-sol.webp"
  description="适用于复杂编码、计算机使用、研究和网络安全的最强大的 GPT-5.6 模型。"
  data={{
    features: [
      {
        title: "能力",
        value: "",
        icons: [
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
        ],
      },
      {
        title: "速度",
        value: "",
        icons: ["openai.Flash", "openai.Flash"],
      },
      { title: "ChatGPT 桌面应用程序", value: true },
      { title: "ChatGPT 网页", value: true },
      { title: "Codex CLI", value: true },
      { title: "Codex IDE扩展", value: true },
      { title: "Codex云", value: true },
      { title: "ChatGPT 制作人员", value: true },
      { title: "API访问", value: true },
    ],
  }}
/>

<ModelDetails
  client:load
  name="gpt-5.6-terra"
  slug="gpt-5.6-terra"
  imageLabel="5.6 Terra"
  wallpaperUrl="/images/api/models/gpt-5.6-terra.webp"
  description="适用于日常工作的平衡 GPT-5.6 模型，性能可与 GPT-5.5 相媲美，但成本更低。"
  data={{
    features: [
      {
        title: "能力",
        value: "",
        icons: [
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
        ],
      },
      {
        title: "速度",
        value: "",
        icons: ["openai.Flash", "openai.Flash", "openai.Flash"],
      },
      { title: "ChatGPT 桌面应用程序", value: true },
      { title: "ChatGPT 网页", value: true },
      { title: "Codex CLI", value: true },
      { title: "Codex IDE扩展", value: true },
      { title: "Codex云", value: false },
      { title: "ChatGPT 制作人员", value: true },
      { title: "API访问", value: true },
    ],
  }}
/>

<ModelDetails
  client:load
  name="gpt-5.6-luna"
  slug="gpt-5.6-luna"
  imageLabel="5.6 Luna"
  wallpaperUrl="/images/api/models/gpt-5.6-luna.webp"
  description="快速且经济实惠的 GPT-5.6 模型能够以系列中最低的成本提供强大的功能。"
  data={{
    features: [
      {
        title: "能力",
        value: "",
        icons: [
          "openai.SparklesFilled",
          "openai.SparklesFilled",
          "openai.SparklesFilled",
        ],
      },
      {
        title: "速度",
        value: "",
        icons: ["openai.Flash", "openai.Flash", "openai.Flash", "openai.Flash"],
      },
      { title: "ChatGPT 桌面应用程序", value: true },
      { title: "ChatGPT 网页", value: true },
      { title: "Codex CLI", value: true },
      { title: "Codex IDE扩展", value: true },
      { title: "Codex云", value: false },
      { title: "ChatGPT 制作人员", value: true },
      { title: "API访问", value: true },
    ],
  }}
/>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">
  <ModelDetails
    client:load
    name="gpt-5.3-codex-spark"
    slug="gpt-5.3-codex-spark"
    imageLabel="5.3 Codex Spark"
    wallpaperUrl="/images/codex/codex-wallpaper-2.webp"
    description="纯文本研究预览模型针对近乎即时的实时编码迭代进行了优化。可供 ChatGPT Pro 用户使用。"
    data={{
      features: [
        {
          title: "能力",
          value: "",
          icons: ["openai.SparklesFilled", "openai.SparklesFilled"],
        },
        {
          title: "速度",
          value: "",
          icons: [
            "openai.Flash",
            "openai.Flash",
            "openai.Flash",
            "openai.Flash",
            "openai.Flash",
          ],
        },
        { title: "ChatGPT 桌面应用程序", value: true },
        { title: "ChatGPT 网页", value: false },
        { title: "Codex CLI", value: true },
        { title: "Codex IDE扩展", value: true },
        { title: "Codex云", value: false },
        { title: "ChatGPT 制作人员", value: false },
        { title: "API访问", value: false },
      ],
    }}
  />
</ContentModeSwitch>




可用性取决于部署、您的登录方法和您的客户。有关计划访问和使用，请参阅 [定价](pricing.zh-CN.md)，有关企业访问，请参阅 [工作区模型可用性](enterprise/workspace-model-availability.zh-CN.md#gpt-6-astra-in-enterprise)。

从您帐户可用的默认电源设置开始。转向 **更聪明** 进行更深入的推理，或者转向 **更快** 进行更快、成本更低的工作。当您需要 `gpt-5.6-luna` 或特定模型、推理工作量或速度时，请打开 **高级**。

<ContentModeSwitch group="codex-surface" ids="app,web">

选择器插图显示了 GPT-5.6 控件。对于符合条件的 Pro、Business（100 美元）和 Enterprise 帐户，Astra 推出将电源选项更新为 Terra Light、Sol Light、Sol Medium、Astra Light、Astra Medium 和 Astra Extra High。选项可能因计划和推出阶段而异。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="experimental-context-management"></a>

### 实验上下文管理

在受支持的 Codex 客户端上，使用 ChatGPT Plus 或 Pro 登录的用户可以选择加入实验性上下文管理。 Astra 可以跨上下文窗口保存笔记，并且可以搜索同一任务的早期消息和工具结果。此实验默认处于关闭状态，并且在启动时无法通过 Business、Enterprise 或 API 密钥登录进行。

要选择加入，请在 `config.toml` 中设置 `features.context_management.experimental_mode = true`，然后开始新任务。有关设置，请参阅 [配置参考](config-file/config-reference.zh-CN.md)，有关文件位置，请参阅 [配置基础知识](config-file/config-basic.zh-CN.md)。工作区要求仍然适用。

</ContentModeSwitch>

<a id="choosing-sol-terra-and-luna"></a>

<a id="choosing-astra-sol-terra-and-luna"></a>

## 选择 Astra、Sol、Terra 和 Luna

当任务需要跨多个步骤和工具的最强功能时，请选择 **阿斯特拉**。 **索尔** 提供深度和抛光，**泰拉** 适合日常工作，**露娜** 适合清晰、可重复的任务。

<a id="where-each-model-shines"></a>

### 每个模型的闪光点

- **Astra，用于最困难的端到端工作。** 选择 Astra 来实现跨代码、应用程序和需要持续推理和判断的研究的完整工作流程。为其提供定义有用结果的源、模板、约束和检查。 Astra 更擅长提出有针对性的问题并结合您的指导，同时牢记最初的目标和限制。
- **Sol，用于复杂的、开放式的工作。** 选择 Sol 来执行需要额外分析、判断或润色的模糊、困难或高价值任务，例如复杂的代码更改、深入研究或润色文档。对于范围较小的任务，定义完成的内容以保持工作重点。
- **Terra，务实的多面手。** 当您不需要 Sol 的全部深度时，选择 Terra 来进行需要强大推理和工具使用的日常工作。这是您之前进行的 GPT-5.5 工作的自然起点。
- **Luna，用于执行清晰、可重复的任务。** 当您知道什么是好的结果时，请选择 Luna 来执行特定的大批量任务，例如提取、分类、转换和结构化摘要。

<a id="pick-a-reasoning-effort"></a>

### 选择推理努力

使用最少的推理努力来产生您需要的结果。对于需要更多计划、分析或检查的任务，请增加它。

- ChatGPT 桌面应用程序中的 **光**、Web 上的 ChatGPT Work 和 IDE 扩展或 CLI 中的 **低** 适合快速、范围广泛的任务。
- **中等** 平衡需要更多规划的任务的速度和深度。
- **高** 和 **超高** 适合具有多个步骤、来源或权衡的困难工作。

从 GPT-5.5 推理工作到 GPT-5.6 没有精确的映射。在较低的设置下尝试熟悉的任务，并根据结果进行调整。

<a id="know-when-to-use-max-or-ultra"></a>

### 了解何时使用 Max 或 Ultra

**最大** 为所选模型提供了更多时间来推理单个任务。当深度比速度或使用更重要时，可以使用它来解决最困难的问题。如果您在选项中没有看到 Max，则必须在应用设置中启用它。

**超** 使用 [分智能体](agent-configuration/subagents.zh-CN.md) 并行处理复杂任务的各个部分。当您可以将工作划分为有意义的部分时选择它。大多数任务不需要 Max 或 Ultra。

如果 Ultra 未出现在桌面应用程序的模型滑块中，请转至 **设置** > **配置**，然后打开 **超模型选择器滑块**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="other-models"></a>

## 其他模型

当您使用 ChatGPT 登录时，Codex 与上面列出的推荐模型配合使用效果最佳。

**GPT-5.4 和 GPT-5.4 mini 将于 2026 年 8 月 31 日从 Codex 退役。** 如果您使用 ChatGPT 登录，请在保存的配置、自定义智能体和计划任务中将 `gpt-5.4` 替换为 `gpt-5.6-terra`，将 `gpt-5.4-mini` 替换为 `gpt-5.6-luna`。使用您自己的 API 密钥进行身份验证的 OpenAI API 和 Codex 不受影响。

<ToggleSection title="查看其他模型">
  

    <ModelDetails
      client:load
      name="gpt-5.5"
      slug="gpt-5.5"
      imageLabel="5.5"
      wallpaperUrl="/images/api/models/gpt-5.5.jpg"
      description="适用于复杂编码、计算机使用、知识工作和研究工作流程的上一代旗舰模型。"
      data={{
        features: [
          {
            title: "能力",
            value: "",
            icons: [
              "openai.SparklesFilled",
              "openai.SparklesFilled",
              "openai.SparklesFilled",
              "openai.SparklesFilled",
            ],
          },
          {
            title: "速度",
            value: "",
            icons: ["openai.Flash", "openai.Flash", "openai.Flash"],
          },
          { title: "ChatGPT 桌面应用程序", value: true },
          { title: "ChatGPT 网页", value: true },
          { title: "Codex CLI", value: true },
          { title: "Codex IDE扩展", value: true },
          { title: "Codex云", value: false },
          { title: "ChatGPT 制作人员", value: true },
          { title: "API访问", value: true },
        ],
      }}
    />

    <ModelDetails
      client:load
      name="gpt-5.4"
      slug="gpt-5.4"
      imageLabel="5.4"
      wallpaperUrl="/images/api/models/gpt-5.4.jpg"
      description="适用于专业工作的旗舰模型，具有强大的编码、推理、工具使用和代理工作流程功能。"
      data={{
        features: [
          {
            title: "能力",
            value: "",
            icons: [
              "openai.SparklesFilled",
              "openai.SparklesFilled",
              "openai.SparklesFilled",
            ],
          },
          {
            title: "速度",
            value: "",
            icons: ["openai.Flash", "openai.Flash", "openai.Flash"],
          },
          { title: "ChatGPT 桌面应用程序", value: true },
          { title: "ChatGPT 网页", value: true },
          { title: "Codex CLI", value: true },
          { title: "Codex IDE扩展", value: true },
          { title: "Codex云", value: false },
          { title: "ChatGPT 制作人员", value: true },
          { title: "API访问", value: true },
        ],
      }}
    />

    <ModelDetails
      client:load
      name="gpt-5.4-mini"
      slug="gpt-5.4-mini"
      imageLabel="5.4 Mini"
      wallpaperUrl="/images/api/models/gpt-5-mini.jpg"
      description="用于响应式编码任务和子智能体的快速、高效的迷你模型。"
      data={{
        features: [
          {
            title: "能力",
            value: "",
            icons: ["openai.SparklesFilled", "openai.SparklesFilled"],
          },
          {
            title: "速度",
            value: "",
            icons: [
              "openai.Flash",
              "openai.Flash",
              "openai.Flash",
              "openai.Flash",
            ],
          },
          { title: "ChatGPT 桌面应用程序", value: true },
          { title: "ChatGPT 网页", value: true },
          { title: "Codex CLI", value: true },
          { title: "Codex IDE扩展", value: true },
          { title: "Codex云", value: false },
          { title: "ChatGPT 制作人员", value: true },
          { title: "API访问", value: true },
        ],
      }}
    />

  

</ToggleSection>

您还可以将 Codex 指向支持 [聊天完成](https://platform.openai.com/docs/api-reference/chat) 或 [响应 API](https://platform.openai.com/docs/api-reference/responses) 的任何模型和提供商，以适合您的特定用例。

对聊天完成 API 的支持已弃用，并将在 Codex 的未来版本中删除。

<a id="deprecated-codex-models"></a>

## 已弃用的 Codex 模型

`gpt-5.4` 和 `gpt-5.4-mini` 模型将于 2026 年 8 月 31 日从 Codex 中退役，并登录 ChatGPT。在工作区默认值、保存的模型设置、托管配置中，将 `gpt-5.4` 替换为 `gpt-5.6-terra`，将 `gpt-5.4-mini` 替换为 `gpt-5.6-luna`，自定义智能体和计划任务。

当您使用 ChatGPT 登录时，`gpt-5.2` 和 `gpt-5.3-codex` 模型已在 Codex 中弃用。更新仍然引用这些模型的脚本、配置文件和 `codex exec --model` 命令。

使用您自己的 API 密钥进行身份验证的 OpenAI API 和 Codex 不受 GPT-5.4 停用的影响。有关当前 API 模型可用性的信息，请参阅 [API 模型页面](https://developers.openai.com/api/docs/models)。

<a id="configure-your-default-local-model"></a>

## 配置您的默认本地模型

ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展使用相同的 `config.toml` [配置文件](config-file/config-basic.zh-CN.md)。要指定模型，请将 `model` 条目添加到配置文件中。如果您不指定模型，ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展将使用推荐的模型。

```toml
model = "gpt-5.6"
```


<a id="choose-a-model-for-cloud-chats"></a>

## 选择云聊天模型

目前，您无法更改 Codex 云聊天的默认模型。

</ContentModeSwitch>