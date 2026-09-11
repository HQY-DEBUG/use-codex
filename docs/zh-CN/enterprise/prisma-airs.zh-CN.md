> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/prisma-airs.md)。

<a id="prisma-airs"></a>

# Prisma AIRS 集成

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

连接 Palo Alto Networks Prisma AIRS，在 Codex 提示到达模型之前将您的安全策略应用到它们。工作区管理员为其工作区配置一次集成。

Prisma AIRS 可以应用安全配置文件中配置的保护，例如数据丢失防护、提示注入检测和恶意 URL 检测。

<a id="before-you-begin"></a>

## 开始之前

您需要：

- 启用 Prisma AIRS 访问的 ChatGPT 工作区。请联系您的 OpenAI 客户团队请求访问权限。
- 工作区管理员权限。
- Prisma AIRS API 密钥、配置的安全配置文件以及部署的服务端点。

<a id="connect-prisma-airs"></a>

## 连接 Prisma AIRS

1. 以工作区管理员身份打开 [Codex 数据控制](https://chatgpt.com/codex/cloud/settings/data)。
2. 在**外部护栏**下，找到**Prisma AIRS**。如果此部分不可用，请要求您的 OpenAI 客户团队启用对您的工作区的访问权限。
3. 输入您的 **API密钥**、**安全配置文件** 姓名或 ID 以及 **端点 URL**。
4. 选择 **执行方式** 和行为 **关于 AIRS 故障**。
5. 选择**保存连接**。 Codex 验证连接并加密您的 API 密钥。
6. 选择 **测试连接** 以验证保存的配置。
7. 打开 **启用 Prisma AIRS** 以开始扫描工作区中的提示。

保存连接不会启用扫描。您还必须打开 **启用 Prisma AIRS**。

<a id="choose-an-endpoint"></a>

## 选择一个端点

使用批准的端点进行 Prisma AIRS 部署：

| 区域 | 端点 |
| ------------- | -------------------------------------------------------- |
| 美国 | `https://service.api.aisecurity.paloaltonetworks.com` |
| 德国 | `https://service-de.api.aisecurity.paloaltonetworks.com` |
| 印度 | `https://service-in.api.aisecurity.paloaltonetworks.com` |
| 新加坡 | `https://service-sg.api.aisecurity.paloaltonetworks.com` |

Codex 默认使用美国端点。工作区数据驻留要求可能会限制您可以使用的端点。

<a id="choose-how-to-handle-prompts"></a>

## 选择如何处理提示

**执行方式** 确定 Prisma AIRS 标记提示时会发生什么：

- **块**：在到达模型之前停止提示。这是默认设置。
- **仅警报**：记录检测并允许提示继续。

**关于 AIRS 故障** 确定 Prisma AIRS 不可用或不响应时会发生什么情况：

- **允许提示**：在未完成扫描的情况下继续。这是默认设置。
- **阻止提示**：停止提示，直到 Prisma AIRS 可以扫描它。

当您的安全策略要求每个涵盖的提示都收到扫描决定时，请选择 **阻止提示**。

<a id="understand-what-gets-scanned"></a>

## 了解扫描的内容

Codex 将新提交的提示文本发送到配置的 Prisma AIRS 端点以进行检查。当用户对配置的 ChatGPT 工作区进行身份验证时，这适用于涵盖的 Codex 工作流程，包括应用程序、CLI、IDE 扩展和云。不包括使用平台 API 密钥进行身份验证的会话。请参阅 [强制执行登录方法或工作区](../auth.zh-CN.md#enforce-a-login-method-or-workspace) 以了解所需的登录方法和工作区。

Prisma AIRS 不会通过此集成扫描助手响应、工具调用、工具结果、文件或图像。您配置的安全配置文件决定 Prisma AIRS 检测哪些威胁和敏感数据。

Codex 会加密您的 API 密钥，并且在保存后不会显示它。在启用即时检查之前，请先查看 Palo Alto Networks 的数据处理、保留和驻留策略。这些政策适用于发送到 Prisma AIRS 的提示。

<a id="manage-the-connection"></a>

## 管理连接

返回[Codex 数据控制](https://chatgpt.com/codex/cloud/settings/data)来管理集成：

- 选择 **测试连接** 以验证您保存的 API 密钥、安全配置文件和端点。
- 输入新密钥并选择 **轮换 API 密钥** 替换已保存的密钥而不更改其他设置。
- 关闭 **启用 Prisma AIRS** 以停止扫描，同时保留保存的配置。
- 选择**断开连接**，然后确认，停止扫描并删除保存的连接和API密钥。

有关更广泛的工作区设置和策略管理，请参阅 [管理员推出指南](admin-setup.zh-CN.md) 和 [受管配置](managed-configuration.zh-CN.md)。