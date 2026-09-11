> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/build-plugins.md)。

<a id="build-plugins"></a>

# 创建插件入口

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

要构建或提交插件，请使用完整的 [Developers.openai.com 上的构建器文档](https://developers.openai.com/plugins)。



  <ButtonLink href="https://learn.chatgpt.com/plugins" color="primary" variant="solid" size="lg">
构建并提交插件
  </ButtonLink>



本页提供了简要介绍。插件是一个可安装的包，可以包含技能、MCP 服务器或两者。 MCP 服务器还可以返回可选的 UI。

ChatGPT和Codex共享一个通用插件目录。发布一次公共插件即可从两个产品中支持的界面中发现相同的列表。在开发过程中，在将包提交到通用目录之前，使用本地市场来测试包。

有关通过 GitHub 分配工作区的信息，请参阅 [插件管理](enterprise/plugin-management.zh-CN.md)。

当您仍在迭代一个个人工作流程时，请从一项技能开始。当您想要共享工作流程、打包相关技能、连接到外部服务或向团队分发稳定功能时，构建插件。

<a id="create-a-plugin-with-plugin-creator"></a>

## 使用 `@plugin-creator` 创建插件

为了实现最快的设置，请在 ChatGPT Work 模式下使用内置 `@plugin-creator` 技能，或在 Codex 模式下使用 `$plugin-creator`。


  

> 插图：ChatGPT 中的插件创建技巧




描述结果、要包含的技能或 MCP 服务器，以及您是否希望进入本地市场进行测试。例如：

```text
@plugin-creator 创建一个名为 meet-follow-up 的插件。
包括将会议记录转化为决策、责任人和后续步骤的技能。
将其添加到个人市场，以便我可以在本地进行测试。
```

该技能创建受支持的 `.codex-plugin/plugin.json` 兼容性清单，组织插件文件夹，并可以将插件添加到本地市场。此脚手架与手动示例中使用的可移植根 `plugin.json` 格式不同。有关可选文件和目录，请参阅 [脚手架布局](https://developers.openai.com/plugins/build/plugins#plugin-creator-output)。


  

> 插图：调用插件创建者技能




完成后：

1. 查看 `.codex-plugin/plugin.json`。
2. 根据 [遵循指令的指导](https://developers.openai.com/plugins/build/skills#review-instruction-following) 检查 `skills/` 下的每个捆绑技能。
3. 刷新 ChatGPT 或 Codex 并从其本地市场源安装插件。
4. 在具有代表性请求的新对话中测试插件。

如果插件包含 MCP 服务器，请首先构建并测试该服务器，然后为 `@plugin-creator` 提供注册的连接详细信息。按照完整的 [MCP 服务器工作流程](https://developers.openai.com/plugins/build/mcp-server) 进行工具、身份验证、部署和测试。

<a id="create-a-skills-only-plugin-manually"></a>

## 手动创建仅限技能的插件

最小的便携式智能体插件包包含根清单和至少一项技能：

```text
meeting-follow-up/
├── plugin.json
└── skills/
    └── meeting-follow-up/
        └── SKILL.md
```

在插件根目录创建 `plugin.json`：

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "meeting-follow-up",
  "version": "1.0.0",
  "description": "Turn meeting notes into decisions and next steps"
}
```

便携包自动发现`skills/`中的技能。添加`skills/meeting-follow-up/SKILL.md`：

```md
---
名称：会议后续行动
描述：从会议记录中提取决策、所有者和后续步骤。
---

查看会议记录。返回：

1. Decisions
2. 与业主的行动项目
3. 开放式问题
```

在 kebab 情况下使用稳定的插件名称。保持技能描述足够具体，以便 ChatGPT 和 Codex 能够在工作流程应用时识别。

使用 `@plugin-creator` 将文件夹添加到本地市场，然后在共享之前安装并测试它。

<a id="continue-with-the-builder-documentation"></a>

## 继续查看构建器文档

如需完整的构建器文档，请使用 [插件文档](https://developers.openai.com/plugins/)。它涵盖：

- [插件架构](https://developers.openai.com/plugins/concepts/plugins)
- [培养技能](https://developers.openai.com/plugins/build/skills)
- [搭建MCP服务器](https://developers.openai.com/plugins/build/mcp-server)
- [添加可选的用户界面](https://developers.openai.com/plugins/build/chatgpt-ui)
- [打包一个插件](https://developers.openai.com/plugins/build/plugins)
- [测试插件](https://developers.openai.com/plugins/deploy/connect-chatgpt)
- [提交和发布](https://developers.openai.com/plugins/deploy/submission)

要浏览、安装、启用或删除插件，请参阅 [使用插件](plugins.zh-CN.md)。