> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/skills.md)。

<a id="skill-controls"></a>

# 技能管理控制

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

技能是由指令和支持资源组成的可重用工作流程。 ChatGPT 工作区技能、ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中涵盖的本地功能使用的文件系统技能，以及打包技能的插件具有单独的生命周期和访问控制。

有关完整的管理模型，请参阅 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="distinguish-the-distribution-models"></a>

<a id="skill-distribution-and-administration"></a>

## 技能分配和管理

| 分布模型 | 用于 | 管理边界 |
| ----------------------- | ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| ChatGPT 工作区技能 | 通过支持的 ChatGPT 工作区功能共享或安装批准的工作流 | ChatGPT 工作区技能权限和生命周期控制 |
| 本地文件系统技能 | 从仓库、用户、管理员或捆绑的系统位置加载已安装的工作流 | 文件系统分发、本地客户端配置和运行时权限 |
| 插件 | 使用可选连接器、MCP 服务器、挂钩和演示元数据打包一项或多项技能 | 插件可用性和安装，以及每个捆绑功能的单独控件 |

ChatGPT 工作区技能分发、本地文件系统技能安装和特定于使用界面的插件安装是单独的路径。移动技能不会转移 ChatGPT 工作区所有权、共享、角色分配、插件安装状态或连接器授权。

插件可在 Web、桌面和移动设备上的 ChatGPT 上的聊天和工作中、在 ChatGPT 桌面应用程序中的 Codex 中以及通过 Codex CLI 插件浏览器中使用。它们在 IDE 扩展中不可用。这些受支持的使用界面从 ChatGPT 和 Codex 共享的一个通用目录中提取公共插件。

<a id="owning-controls"></a>

## 拥有控制权

有关文件系统位置和创作的信息，请参阅 [培养技能](../build-skills.zh-CN.md)；有关当前工作区过程的信息，请参阅 [ChatGPT 的技能](https://help.openai.com/en/articles/20001066-skills-in-chatgpt)；有关插件打包的信息，请参阅 [构建插件](https://developers.openai.com/plugins/build/plugins)。

ChatGPT 工作区控件不安装本地文件系统技能或插件。文件系统分发不分配 ChatGPT 工作区所有权或角色。插件安装不会授予对连接器、MCP 服务器或连接服务的访问权限。通过拥有该功能的控制界面来配置每个功能。

<a id="related-docs"></a>

## 相关文档

- [技能和插件](../skills-and-plugins.zh-CN.md)
- [插件](../plugins.zh-CN.md)
- [培养技能](../build-skills.zh-CN.md)
- [构建插件](https://developers.openai.com/plugins/build/plugins)
- [管理员推出指南](admin-setup.zh-CN.md)
- [插件控件](apps-and-connectors.zh-CN.md)