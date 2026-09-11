> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/image-inputs.md)。

<a id="image-inputs"></a>

# 图像输入

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

当任务依赖于视觉上下文（例如错误屏幕截图、界面设计、架构图或现有资产）时，将图像添加到提示中。解释 ChatGPT 应该检查什么以及您想要什么结果；不要仅依靠图像来传达任务。

<ContentModeSwitch group="codex-surface" id="app">

按住时将图像拖到提示编辑器中<kbd>Shift</kbd>将其包含为上下文。您还可以要求 ChatGPT 检查系统上的图像或使用屏幕截图工具来验证其他应用程序中的工作。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

将图像附加、粘贴或拖动到 ChatGPT Web 编辑器中。在提示中，告诉 ChatGPT 要检查什么以及您希望从图像中获得什么结果。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

将图像粘贴到交互式编辑器中，或在命令行上传递一个或多个文件：

```bash
codex -i screenshot.png "Explain this error and suggest the smallest fix"
codex --image before.png,after.png "Compare these states and list the regressions"
```

对于多个图像，请用逗号分隔路径或重复 `--image`。 Codex 接受常见的图像格式，包括 PNG 和 JPEG。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

按住时将图像拖到提示编辑器中<kbd>Shift</kbd>因此扩展程序接受拖放而不是将其传递给编辑器。

</ContentModeSwitch>

<a id="write-the-prompt-around-the-image"></a>

## 在图像周围写下提示

命名图像显示的内容，指出重要的区域，并说明输出和约束。如果您附加多于一张图像，请识别每张图像并解释 ChatGPT 应如何比较它们。

例如：

```text
将此结帐屏幕与设计进行比较。仅修复间距和版式；
不要改变行为。使用新的屏幕截图验证结果。
```

<a id="use-the-right-image-feature"></a>

## 使用正确的图像特征

当您希望 ChatGPT 检查视觉参考时，请使用图像输入。当您希望 ChatGPT 创建或编辑图像时，请使用 [图像生成](image-generation.zh-CN.md)。