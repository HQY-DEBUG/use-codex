> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/gpts-and-sharing.md)。

<a id="gpts-and-sharing"></a>

# GPT 与共享管理

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="sharing"></a>

## 分享

控制谁可以创建 GPT 以及是否可以与特定人员、组或整个工作区共享。

GPT 构建者可以使用连接的应用程序或自定义操作，但不能在同一 GPT 中同时使用两者。

<a id="connected-apps-and-actions"></a>

## 连接的应用程序和操作

允许 GPT 构建者使用批准的工作区应用程序或配置与允许的第三方 API 交互的操作。访问仍受工作区策略、用户权限和批准的域的约束。

<a id="domains"></a>

## 域名

将 GPT 操作限制为批准的外部域，以控制在工作区中创建的 GPT 可以访问哪些第三方 API。如果不允许任何域，则无法执行自定义 GPT 操作。

<WarningTip>
域批准不会取代 API 身份验证或用户授权。
</WarningTip>

<a id="managing-gpts"></a>

## 管理 GPT

查看在工作区中创建的 GPT、管理共享和所有权，并在适当时删除 GPT。管理视图按已分配和未分配的所有权组织 GPT，并包含以下信息：

- 名称
- 建设者
- 自定义操作
- 谁可以访问
- 聊天记录
- 已创建
- 已更新