<a id="personalize-chatgpt"></a>

# 个性化 ChatGPT

> 本文根据本地英文原文 [personalize.md](../en/personalize.md) 翻译，为非官方简体中文译文。

> 完整文档索引请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。在文档页面的网址末尾添加 `.md`，即可获取该页面的 Markdown 版本。

对 ChatGPT 进行个性化设置，让它的回答和工作方式更符合你的偏好。你可以控制启用哪些个性化功能，并随时在 ChatGPT 桌面应用的设置中更改。

<a id="choose-a-personality"></a>

## 选择个性

在**设置 > 个性化（Settings > Personalization）**中，选择**友好（Friendly）**、**务实（Pragmatic）**或**无（None）**作为默认个性。个性会改变 ChatGPT 的交流方式，但不会改变模型的能力。

<a id="add-custom-instructions"></a>

## 添加自定义指令

使用自定义指令，设置你希望 ChatGPT 在不同对话中都遵循的偏好，例如你喜欢的回答风格。在 Codex 中，这些个人指令保存在全局 `AGENTS.md` 文件中。项目和代码仓库也可以提供各自的指令。

[了解 `AGENTS.md` 指令的工作方式](agent-configuration/agents-md.zh-CN.md)。

<a id="carry-context-forward-with-memories"></a>

## 通过记忆延续上下文

[记忆](customization/memories.zh-CN.md)让 ChatGPT 能够将先前对话中的有用上下文带入之后的工作。这些内容可以包括长期稳定的偏好、重复使用的工作流程、项目约定，以及其他原本需要你反复说明的背景信息。

记忆与必须遵循的项目指导是分开的。对于必须始终生效的指令，请将其保存在 `AGENTS.md` 或已纳入版本控制的项目文档中。

<a id="add-recent-activity-with-computer-history"></a>

## 通过 Computer History 添加近期活动

[Computer History（电脑历史记录）](customization/computer-history.zh-CN.md)是一项需要主动启用的 macOS 桌面功能，可以将获准访问的应用和网站中的活动转化为记忆和时间线。它使用交互事件，以及通过 macOS 辅助功能获取的文本和其他上下文。它不会在历史记录中包含屏幕截图，也不会录制音频。

启用前，请先了解 Computer History 会包含哪些内容。你可以随时暂停它、排除特定应用和网站、查看或删除单条时间线记录，以及清除近期或全部历史记录。

<a id="manage-personalization"></a>

## 管理个性化设置

打开[**设置（Settings）**](codex://settings)，更新个性、自定义指令、记忆以及其他可用的个性化选项。有关日常偏好设置的概览，请参阅 [ChatGPT 桌面应用设置](reference/settings.zh-CN.md)。
