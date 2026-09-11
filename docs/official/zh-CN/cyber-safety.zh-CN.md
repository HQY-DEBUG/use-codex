> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/cyber-safety.md)。

<a id="models-and-trusted-access"></a>

# 网络安全模型与受信任访问

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

OpenAI Daybreak帮助经批准的用户执行授权的防御性网络安全工作。 Daybreak Blue 提供对旗舰模型的访问，减少了对授权防御工作流程的拒绝。 Daybreak Red 提供单独批准的专业网络模型访问权限，以进行更高级的安全研究。

将您批准的模型与受控环境相结合，对批准的系统和操作进行明确的限制，最小权限，以及在敏感操作运行之前进行自动审查。仅将模型与批准的身份、工作区或 API 组织和项目以及产品使用界面一起使用。

<a id="choose-the-right-model"></a>

## 选择合适的模型

从 **GPT-黎明-蓝色** 开始，适用于大多数授权的防御工作。该模型提供了对高级功能的访问，同时减少了防御安全工作流程的拒绝，包括：

- 漏洞发现和分类。
- 安全代码审查和威胁建模。
- 检测工程和事件响应。
- 在受控环境中进行恶意软件分析。
- 修复和补丁验证。

**GPT-黎明-红色** 是一种专门的网络模型，适用于单独批准、明确授权的工作流程，例如受控漏洞复制、概念验证或利用验证、渗透测试、红队和复杂系统分析。它不是日常安全工作的默认选择，并且访问权限并非自动可用或在每个使用界面上可用。

这些高级工作流程可能类似于未经明确授权的恶意活动。仅将批准的模型和使用界面用于您拥有或明确授权评估的系统，并保持适当的人工监督。

例如：

- **GPT-黎明-蓝色：** 审查已批准的实验室仓库中的身份验证漏洞，按证据和影响对结果进行排名，并在不访问外部系统的情况下提出补丁。
- **GPT-黎明-红色：** 在批准的实验室和测试窗口内，重现记录的身份验证缺陷，验证最小的概念证明，并在凭证访问、持久性或生产更改之前停止。

<a id="trusted-access-for-cyber"></a>

## 网络可信访问

通过[网络可信访问](https://help.openai.com/en/articles/20001258-trusted-access-for-cyber)请求**黎明访问**。访问权限取决于对您的特定身份或服务、ChatGPT 工作区或 API 组织和项目、授权产品和模型以及允许的产品使用界面的批准和配置。

- 个人可以通过 [个人可信访问应用程序](https://chatgpt.com/cyber) 请求访问。
- 组织可以提交 [企业可信访问申请表](https://openai.com/form/enterprise-trusted-access-for-cyber/) 并与其 OpenAI 代表进行协调。

提交申请或完成身份验证并不能保证获得批准。

申请、验证您的身份或获得 Daybreak Blue 的批准并不授予对 Daybreak Red 或 GPT-Daybreak-Red 的访问权限。专业产品需要单独的批准和配置。

对于企业访问，仅将批准的工作区、API 组织或项目用于组织授权的内部工作。请勿将其扩展到外部用户、第三方客户、外部提供的服务、下游产品功能或批准工作之外的系统。如果批准的身份、工作区、API 组织、项目、模型或使用界面不清楚，请停止并与您的 OpenAI 代表确认。

可信访问不会自动授予 [零数据保留](https://developers.openai.com/api/docs/guides/your-data#data-retention-controls-for-abuse-monitoring)。在开始之前，请确认针对具体 API 组织和适用端点的任何单独批准的保留控制。

<a id="false-positives"></a>

## 误报

合法的网络安全或不相关的活动仍然可以触发保障措施。如果安全措施阻止、重新路由或限制请求，请检查可用的客户端通知和请求日志。查看 [常见问题和故障排除](https://help.openai.com/en/articles/20001259) 以了解要收集的详细信息和后续步骤。通过 `/feedback` 报告可疑的 Codex 误报（如有）。对于API访问限制和申诉，请遵循[API 网络安全检查指南](https://developers.openai.com/api/docs/guides/safety-checks/cybersecurity#appeals)。

所有用户仍受 [使用政策](https://openai.com/policies/usage-policies/) 和 [使用条款](https://openai.com/policies/row-terms-of-use/) 的约束。

<a id="configure-your-security-workflow"></a>

## 配置您的安全工作流程

可信访问管理批准的模型访问，但它不会配置您的环境、对批准的系统和操作实施限制或审查建议的操作。

- [使用推荐配置](cyber-safety/recommended-configuration.zh-CN.md) 用于隔离、最低权限、明确定义的边界以及敏感操作的护栏。