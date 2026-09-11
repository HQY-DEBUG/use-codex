> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/image-generation.md)。

<a id="image-generation"></a>

# 图像生成

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

要求 ChatGPT 生成或编辑图像。使用图像生成来生成您想要与代码一起或在 ChatGPT 聊天中创建的 UI 资产、横幅、背景、插图、精灵表和占位符。

<ContentModeSwitch group="codex-surface" id="app">

向应用程序编写者索取图像。当您希望 ChatGPT 转换现有资产或将其用作视觉指导时，请添加参考图像。

<a id="review-and-edit-generated-images"></a>

### 查看和编辑生成的图像

选择生成的图像以打开其扩展查看器。在 **聚焦视图** 之间切换以检查一张图像，在 **画布视图** 之间切换以查看同一聊天中生成的图像。

在**画布视图**中，使用**评论**为一张或多张图像添加精确的反馈。选择 **多选** 以选择您要包含的图像，然后在同一聊天中发送您的评论和任何其他编辑说明。描述什么应该改变，什么应该保持不变。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

在 ChatGPT 网络聊天中索取图像。当您希望 ChatGPT 编辑参考图像或将其用作视觉指导时，请将参考图像附加到输入框。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在交互式会话中描述图像或包含 `$imagegen` 以显式调用图像生成技能。当需要指导结果时，附加带有 `-i` 或 `--image` 的现有图像。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

从扩展聊天中索取图像。按住时将参考图像拖动到编辑器中<kbd>Shift</kbd>当 Codex 应该在现有资产上进行编辑或构建时。

</ContentModeSwitch>

<a id="generate-or-edit-an-image"></a>

## 生成或编辑图像

用自然语言描述图像。当您希望 ChatGPT 转换或扩展现有资产时，添加参考图像。

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

在提示中包含 `$imagegen` 以显式调用图像生成技能。

内置图像生成使用 `gpt-image-2` 并计入您的一般 Codex 使用限制。使用包含限制的图像生成平均比不生成图像的类似对话轮次快 3-5 倍，具体取决于图像质量和尺寸。对于较大批次，请在您的环境中设置 `OPENAI_API_KEY` 并要求 ChatGPT 通过 API 生成图像，以便应用 API 定价。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT Web 中的图像可用性和使用限制取决于您的计划和工作区设置。对于程序化图像生成，请使用 [图像生成API](https://developers.openai.com/api/docs/guides/image-generation)。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,web">

<a id="write-effective-image-prompts"></a>

## 编写有效的图像提示

一个有用的图像提示通常只有一到三个清晰的句子。描述一下决定结果是否成功的细节：

- 解释图像的目的或目标受众。
- 说出主要主题以及正在发生的事情。
- 描述背景、构图和视觉风格。
- 在重要时添加框架、尺寸、灯光、颜色或材料。
- 状态约束，包括图像不得包含的任何内容。

比起广泛的反应，更喜欢具体的视觉语言。例如，描述光从哪里来，而不是要求“美丽的灯光”。重复任何必须保持​​固定的要求。



**提示示例：**

```text
为员工入职指南创建简洁的编辑插图。展示一个人在办公桌前用笔记本电脑、笔记本和简单的进度清单组织项目。利用左侧窗户射入的柔和日光、内敛的色彩以及现代、平易近人的风格。保持背景最小化。请勿包含徽标、文本或未来图像。
```

<a id="refine-the-result"></a>

## 细化结果

从核心思想开始，然后进行小的、有针对性的修改。一次调整一个元素，这样构图和其他重要细节就不会发生变化。您还可以选择图像的特定区域并描述该区域的更改。

编辑现有图像时，准确说明哪些内容应该更改，哪些内容必须保持不变。



**提示示例：**

```text
编辑附加图像。仅用小盆栽植物代替杯子。准确地保留人员、桌面布局、灯光、颜色、裁剪和所有其他细节。请勿添加文字或徽标。
```

对于更广泛的修改，请保持反馈直接且可操作：使图像更明亮，降低颜色饱和度，简化背景，或在改变风格的同时保持构图。

<a id="use-multiple-reference-images"></a>

## 使用多个参考图像

当一个图像定义内容而另一个图像定义样式、布局或其他视觉方向时，请使用一小组参考图像。按顺序识别每个图像并解释图像之间的关系。组合元素时使用空间术语，例如前景、背景、左侧和右侧。



**提示示例：**

```text
图 1 是要编辑的产品照片。图 2 是样式参考。保留图 1 中的产品、相机角度、布局和对象，但应用图 2 中的简洁线条、柔和的调色板和柔和的阴影。保持产品居中，并保留右上角空白，以便以后复制。
```

<a id="add-text-to-an-image"></a>

## 向图像添加文本

保持图像内文本简短并精确指定。将确切的文本放在引号中，保留所需的大小写，并描述其字体样式、大小、颜色和位置。对于不常见的名称，当准确性很重要时，请拼写出字母。说明是否允许任何其他文本。



**提示示例：**

```text
仅添加大号、粗体、白色无衬线字母的标题“SPRING WORKSHOP”，位于图像顶部三分之一的中心。将标题保留在一行上。不要添加任何其他文本或更改底层图像。
```

<a id="create-infographics-and-dense-layouts"></a>

## 创建信息图表和密集布局

图像生成可以帮助起草解释、海报、标记图表、时间线和其他信息丰富的视觉效果。描述信息层次结构和布局，保持标签简洁，并要求清晰的文本呈现。对于密集的副本或生产关键型排版，请检查每个单词并在需要时在设计工具中完成资产。

<a id="additional-considerations"></a>

## 其他注意事项

- **谨慎使用相似之处。** 当描绘真人时，请在适当的时候提供参考照片，并确认您有权使用他们的肖像。
- **要求原始治疗。** 请求通用或原创设计，而不是模仿特定品牌、产品、艺术家或艺术品。
- **信用是可选的。** 您无需将生成的图像归功于 OpenAI，尽管您可以在上下文有用时解释资产是如何制作的。
- **遵循适用的政策。** 根据您组织的指南和 [OpenAI的使用政策](https://openai.com/policies/usage-policies/) 使用图像。

</ContentModeSwitch>

<a id="related-docs"></a>

## 相关文档

<ContentModeSwitch group="codex-surface" id="app">

- [Codex 定价](pricing.zh-CN.md#image-generation-usage-limits)
- [图像输入](image-inputs.zh-CN.md)
- [图像生成API指南](https://developers.openai.com/api/docs/guides/image-generation)
- [处理文件](artifacts-viewer.zh-CN.md)
- [使用 ChatGPT 创建图像](https://openai.com/academy/image-generation/)

[图像生成图库



      <Images />
    

探索更多图像生成提示和结果。](https://developers.openai.com/api/docs/guides/image-generation?gallery=open)

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

- [图像输入](image-inputs.zh-CN.md)
- [图像生成API指南](https://developers.openai.com/api/docs/guides/image-generation)
- [处理文件](artifacts-viewer.zh-CN.md)
- [使用 ChatGPT 创建图像](https://openai.com/academy/image-generation/)

[图像生成图库



      <Images />
    

探索更多图像生成提示和结果。](https://developers.openai.com/api/docs/guides/image-generation?gallery=open)

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="cli,ide">

- [Codex 定价](pricing.zh-CN.md#image-generation-usage-limits)
- [图像输入](image-inputs.zh-CN.md)
- [图像生成API指南](https://developers.openai.com/api/docs/guides/image-generation)
- [处理文件](artifacts-viewer.zh-CN.md)

[图像生成图库



      <Images />
    

探索更多图像生成提示和结果。](https://developers.openai.com/api/docs/guides/image-generation?gallery=open)

</ContentModeSwitch>