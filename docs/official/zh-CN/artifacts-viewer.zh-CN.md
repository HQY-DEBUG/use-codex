> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/artifacts-viewer.md)。

<a id="work-with-files"></a>

# 文件处理与预览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

当任务生成文件时，为 ChatGPT 提供源数据、预期文件类型、结构以及与任务相关的审查标准。预览和查看工具取决于您使用的使用界面。



[观看：在 ChatGPT 中处理文档、电子表格和演示文稿](https://www.youtube.com/watch?v=E3dDr_QtBuo)

<ContentModeSwitch group="codex-surface" id="app">

ChatGPT 桌面应用程序可在聊天的同时预览生成的文档、演示文稿、电子表格和 PDF 文件。启用自动预览后，应用程序可以在任务完成后打开生成的文件。

当 HTML 预览可用时，生成的 `.html` 和 `.htm` 文件也可以作为交互式预览打开。在渲染预览和源视图之间切换以检查输出或其底层 HTML。

使用注释指向支持的预览的特定部分并请求重点修订。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

在 Web 上的 ChatGPT Work 中，附加源文件或要求 ChatGPT 创建文档、演示文稿、电子表格或 PDF。在聊天中查看生成的文件，需要时下载，并为下一个版本提供有针对性的反馈。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

Codex CLI可以在工作目录中创建和编辑文件，但不包含可视化文件预览或注释界面。要求 Codex 报告每个输出路径及其运行的检查。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

IDE 扩展可以在工作区中创建和编辑文件。在编辑器中查看文本和代码文件，并在兼容的查看器中打开文档、演示文稿、电子表格或 PDF 文件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">


  

> 插图：ChatGPT 桌面应用程序显示生成的演示文稿预览




</ContentModeSwitch>

<a id="create-files-for-review"></a>

## 创建文件供审查

对于电子表格和演示文稿，描述您期望的工作表、列、图表、幻灯片部分和检查。请 ChatGPT 解释它保存输出的位置以及如何检查结果。

<a id="refine-files-with-annotations"></a>



<a id="review-and-refine-files"></a>

<ContentModeSwitch group="codex-surface" id="app">

<a id="refine-files-with-annotations"></a>

## 使用注释优化文件

注释可让您指向文件的特定部分并告诉 ChatGPT 要更改的内容。适用于代码、Markdown 文件和网站的相同注释工作流程也适用于文档、电子表格和演示文稿。

例如，您可以：

- 选择网站上的导航栏并要求 ChatGPT 更改其字体。
- 突出投资论文中的主张并询问其来源。
- 在幻灯片上标记图表并请求更清晰的标签。

ChatGPT 使用所选区域作为您的请求的上下文，因此您可以优化文件，而无需重新开始或更改您已经喜欢的部分。当工作需要审查和迭代时，注释在初稿之后特别有用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="review-and-refine-files-on-the-web"></a>

## 在网络上审查和完善文件

打开或下载生成的文件以在适当的查看器中查看。当您请求修订时，请命名需要注意的页面、幻灯片、表格、表格或段落，并描述应保持不变的内容。要求 ChatGPT 报告新文件名以及在下载下一版本之前执行的检查。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="review-and-refine-files"></a>

## 审查和完善文件

任务运行时使用聊天侧边栏。它可以显示智能体的计划、来源、生成的文件和聊天摘要，以便您可以指导工作、检查生成的文件并请求另一次通行证。

请 ChatGPT 解释它保存每个文件的位置以及如何验证结果。使用预览来检查输出，然后提供有关需要另一遍的结构、数据、布局或验证的重点反馈。

</ContentModeSwitch>

<a id="related-docs"></a>

## 相关文档

- [图像生成](image-generation.zh-CN.md)