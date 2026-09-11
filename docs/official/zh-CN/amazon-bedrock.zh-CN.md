> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/amazon-bedrock.md)。

<a id="use-chatgpt-work-and-codex-with-amazon-bedrock"></a>

# 通过 Amazon Bedrock 使用模型

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

配置本地 ChatGPT Work 和 Codex 使用界面以使用通过 Amazon Bedrock 提供的 OpenAI 模型。在此设置中，本地客户端使用 AWS 托管的身份验证和访问控制向 Bedrock 发送模型请求。

<a id="how-it-works"></a>

## 它是如何运作的

当您使用 Amazon Bedrock 作为模型提供程序配置本地 ChatGPT Work 或 Codex 使用界面时，OpenAI 托管的响应 API 不在请求路径中。本地客户端将模型请求发送到 Amazon Bedrock，Bedrock 为支持的 OpenAI 模型提供与 OpenAI 兼容的响应 API 实现。

身份验证是 AWS 原生的。用户使用 Bedrock API 密钥或 AWS IAM 凭证进行身份验证。他们不使用 ChatGPT 登录或 `OPENAI_API_KEY` 来访问此提供商。

<a id="before-you-start"></a>

## 开始之前

确保您有：

- 访问 Amazon Bedrock 中支持的 OpenAI 模型。
- 所选模型可用的 AWS 区域。
- 为 AWS 账户配置的 Amazon Bedrock Mantle 路径的身份验证。

<a id="configure-the-provider"></a>

## 配置提供者

将 Amazon Bedrock Mantle 路径的 `amazon-bedrock` 模型提供程序添加到 `~/.codex/config.toml`。 ChatGPT 桌面应用程序、Codex CLI、IDE 扩展和 SDK 读取相同的本地配置层。提供模型是可选的。需要时明确选择支持的模型。

```toml
model_provider = "amazon-bedrock"
```

本指南涵盖受支持的商业 AWS 区域中的 Amazon Bedrock Mantle 路径。本地 ChatGPT Work 和 Codex 使用界面不支持 AWS GovCloud 区域中的 Bedrock Mantle 终端节点。

<a id="authentication-options"></a>

## 身份验证选项

本地 ChatGPT Work 和 Codex 使用界面支持两个 Bedrock 身份验证路径。他们按以下顺序检查：

1. 基岩 API 密钥。
2. AWS SDK 凭证链。

<a id="option-1-bedrock-api-key"></a>

### 选项 1：基岩 API 密钥

在本地客户端读取的环境中设置 Bedrock API 密钥。使用 API 密钥身份验证时，您必须指定区域。

```shell
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

<a id="option-2-aws-sdk-credentials"></a>

### 选项 2：AWS 开发工具包凭证

当您的组织通过 AWS 开发工具包凭证链管理 Bedrock 访问时，请使用此路径。本地客户端可以使用这些标准 AWS 开发工具包凭证源：

<a id="shared-aws-configuration-files"></a>

#### 共享AWS配置文件

配置共享的 AWS `config` 和 `credentials` 文件：

```shell
aws configure
```

<a id="environment-variables"></a>

#### 环境变量

设置标准 AWS 开发工具包凭证环境变量：

```shell
export AWS_ACCESS_KEY_ID=<your-access-key-id>
export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
export AWS_SESSION_TOKEN=<your-session-token>
```

<a id="aws-management-console-credentials"></a>

#### AWS 管理控制台凭证

使用 AWS 管理控制台凭证登录：

```shell
aws login
```

<a id="aws-sso-or-a-named-profile"></a>

#### AWS SSO 或命名配置文件

使用 AWS SSO 登录并选择指定的配置文件：

```shell
aws sso login --profile codex-bedrock
export AWS_PROFILE=codex-bedrock
```

<a id="federated-identity"></a>

#### 联合身份

对于企业 SSO 或 OIDC 联合，请在本地客户端外部使用 `credential_process` 配置联合身份，并让 AWS 开发工具包解析凭证。将浏览器登录、令牌交换、缓存和刷新放入您的 AWS 配置文件的 `credential_process` 帮助程序中。

<a id="desktop-app-and-ide-extension"></a>

## 桌面应用程序和 IDE 扩展

桌面应用程序和 IDE 扩展可能无法从 shell 继承环境变量。将所需的值放入 `~/.codex/.env`，然后重新启动应用程序或扩展。

```shell
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

<a id="verify-setup"></a>

## 验证设置

- 在 Codex CLI 中，打开 `/status` 并确认 Codex 正在使用 `amazon-bedrock` 模型提供程序。
- 在ChatGPT桌面应用程序中，选择工作或Codex并在重新启动应用程序后启动新任务。
- 在 IDE 扩展中，重新启动扩展后启动新会话。
- 确认所选模型在配置的 AWS 区域中可用并且 AWS 身份有权访问它。

<a id="supported-models"></a>

## 支持机型

使用准确的模型 ID：

```text
openai.gpt-5.6-sol
openai.gpt-5.6-terra
openai.gpt-5.6-luna
openai.gpt-5.5
openai.gpt-5.4
```

模型可用性因 AWS 区域而异。选择模型之前，请参阅 [AWS 区域的模型支持](https://docs.aws.amazon.com/bedrock/latest/userguide/models-region-compatibility.html)。

<a id="feature-availability"></a>

## 功能可用性

此配置支持本地 ChatGPT Work 和 Codex 工作流程。 Web 上托管的 ChatGPT Work、Codex 云以及依赖于 OpenAI 托管的云服务、托管工具或云托管发现的功能当前不可用。

Amazon Bedrock 不提供快速模式。快速模式使用优先级处理，最初的 Amazon Bedrock 产品仅支持按需推理。

<ToggleSection title="详细的功能可用性">
  <CodexPlanFeatureMatrix
    client:load
    data={{
      plans: [
        {
          id: "bedrock",
          shortLabel: "Amazon Bedrock",
          label: "亚马逊基岩",
        },
      ],
      sections: [
        {
          title: "通道和使用界面",
          features: [
            {
              name: "网络上的 ChatGPT Work",
              href:"get-started-with-work.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Codex cloud",
              href:"cloud.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "ChatGPT 桌面应用程序中的 ChatGPT Work 或 Codex",
              shortName: "ChatGPT desktop app",
              href:"app.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Codex CLI",
              href:"https://learn.chatgpt.com/codex/cli",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Codex Security CLI",
              href:"security/cli.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "IDE extension",
              href:"https://learn.chatgpt.com/codex/ide",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Codex SDK、`codex exec` 和可编写脚本的工作流程",
              shortName: "Codex SDK and scripting",
              href:"codex-sdk.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
          ],
        },
        {
          title: "模型和多式联运",
          features: [
            {
              name: "支持 OpenAI 模型的基岩支持推理",
              shortName: "Bedrock-backed inference",
              href:"amazon-bedrock.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Fast mode",
              href:"agent-configuration/speed.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "图像生成和编辑",
              href:"image-generation.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Voice dictation",
              href:"prompting.zh-CN.md#use-voice-dictation",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Web search",
              href:"web-search.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
          ],
        },
        {
          title: "当地特色",
          features: [
            {
              name: "Codex Security 插件和本地扫描",
              shortName: "Codex Security plugin",
              href:"security/plugin.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "使用 `/review` 进行本地代码审查",
              shortName: "Local code review",
              href:"prompting.zh-CN.md#do-a-local-code-review",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "自动审核批准请求",
              href:"sandboxing/auto-review.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "沙箱和权限控制",
              href:"permissions.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "项目和独立计划任务",
              shortName: "Scheduled tasks",
              href:"automations.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Scheduled tasks",
              href:"automations.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "工作树和内置 Git 工具",
              shortName: "Built-in Git tools",
              href:"environments/git-worktrees.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "本地环境和可重复的操作",
              shortName: "Repeatable actions",
              href:"environments/local-environment.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Appshots",
              href:"appshots.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
          ],
        },
        {
          title: "浏览器和远程控制",
          features: [
            {
              name: "内置浏览器预览和评论",
              shortName: "Built-in browser",
              href:"browser.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "计算机在浏览器中使用",
              href:"https://learn.chatgpt.com/codex/browser?surface=app#app-computer-use-in-the-browser",
              availability: {
                bedrock: "limited",
              },
            },
            {
              name: "将 ChatGPT 与 Chrome 结合使用",
              shortName: "Chrome browser control",
              href:"chrome-extension.zh-CN.md",
              availability: {
                bedrock: "limited",
              },
            },
            {
              name: "Computer Use",
              href:"computer-use.zh-CN.md",
              availability: {
                bedrock: "limited",
              },
            },
            {
              name: "SSH remote connections",
              shortName: "SSH remote",
              href:"remote-connections.zh-CN.md#connect-to-an-ssh-host",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Mobile remote control",
              href:"remote-connections.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
          ],
        },
        {
          title: "定制和扩展",
          features: [
            {
              name: "`AGENTS.md` 的自定义指令",
              shortName: "Custom instructions",
              href:"agent-configuration/agents-md.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Skills",
              href:"build-skills.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Plugins",
              href:"plugins.zh-CN.md",
              availability: {
                bedrock: "limited",
              },
              limitedFootnote: "plugins",
            },
            {
              name: "Plugin sharing",
              href:"https://developers.openai.com/plugins/build/plugins#share-a-local-plugin-with-your-workspace",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Connectors",
              href:"plugins.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "MCP",
              href:"extend/mcp.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "子智能体和定制智能体",
              shortName: "Subagents",
              href:"agent-configuration/subagents.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Memories",
              href:"customization/memories.zh-CN.md",
              availability: {
                bedrock: "limited",
              },
            },
            {
              name: "Computer History",
              href:"customization/computer-history.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
          ],
        },
        {
          title: "云和集成",
          features: [
            {
              name: "Codex cloud chats",
              shortName: "Cloud chats",
              href:"cloud.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Sites",
              href:"sites.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "GitHub 与 `@codex` 的发行和公关授权",
              shortName: "GitHub delegation",
              href:"third-party/github.zh-CN.md#give-codex-other-tasks",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "GitHub 代码审查和自动 PR 审查",
              shortName: "GitHub PR reviews",
              href:"third-party/github.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Slack cloud integration",
              shortName: "Slack integration",
              href:"third-party/slack.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Linear cloud integration",
              shortName: "Linear integration",
              href:"third-party/linear.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
          ],
        },
        {
          title: "管理、安全和分析",
          features: [
            {
              name: "SAML SSO、MFA 和工作区用户管理",
              shortName: "Workspace management",
              href:"enterprise/admin-setup.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "`requirements.toml` managed config",
              shortName: "`requirements.toml` config",
              href:"enterprise/managed-configuration.zh-CN.md",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Cloud-managed config policies",
              shortName: "Cloud-managed policies",
              href:"enterprise/managed-configuration.zh-CN.md#cloud-managed-requirements",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "ChatGPT 工作区 RBAC 和自定义角色",
              shortName: "RBAC and roles",
              href:"enterprise/roles-and-workspace-permissions.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "SCIM、EKM 和域验证",
              shortName: "SCIM, EKM, and domains",
              href:"enterprise/admin-setup.zh-CN.md#enterprise-grade-security-and-privacy",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "企业保留和驻留控制",
              shortName: "Retention and residency",
              href:"enterprise/admin-setup.zh-CN.md#enterprise-grade-security-and-privacy",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "No training on API or business data by default",
              shortName: "No default training",
              href:"https://openai.com/business-data/",
              availability: {
                bedrock: "available",
              },
            },
            {
              name: "Analytics dashboard",
              href:"enterprise/workspace-analytics.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "Analytics API",
              href:"enterprise/analytics-api.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "合规 API 和审核日志",
              shortName: "Compliance and audit logs",
              href:"enterprise/compliance-api.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
            {
              name: "用于连接 GitHub 仓库的 Codex Security 云",
              shortName: "Codex Security cloud",
              href:"security/setup.zh-CN.md",
              availability: {
                bedrock: "unavailable",
              },
            },
          ],
        },
      ],
    }}
  />

  <div
    id="codex-plan-region-limits"
    className="not-prose mt-3 text-sm text-secondary"
  >
    <sup>*</sup>该功能目前仅限于特定区域。查看各个功能文档以了解有关地理限制的更多信息。
  

  <div
    id="codex-plan-plugin-limits"
    className="not-prose mt-1 text-sm text-secondary"
  >
    <sup>†</sup>本地插件包和 OpenAI 策划的不需要 ChatGPT 身份验证的插件（包括 Codex Security）均可用。需要 ChatGPT 身份验证、连接器或云托管共享的插件不可用。
  

</ToggleSection>

<a id="troubleshooting"></a>

## 故障排除

如果设置失败，请检查以下内容：

- 模型 ID 与支持的模型完全匹配。
- 您指定模型可用的 AWS 区域。
- Bedrock API 密钥或 AWS 凭证有效且未过期。
- AWS 身份有权访问所选的 Bedrock 模型。
- `AWS_BEARER_TOKEN_BEDROCK` 未设置为过期或意外的密钥。
- 对于桌面应用程序或 IDE 扩展的使用，所需的环境变量位于 `~/.codex/.env` 中。

<a id="support-boundaries"></a>

## 支持边界

OpenAI 支持可以帮助 ChatGPT Work 和 Codex 客户端设置、配置、本地 CLI 行为、桌面应用程序行为、IDE 扩展行为和本地产品体验。

有关 AWS 凭证、IAM 权限、Bedrock 模型访问、配额、账单、区域可用性、Bedrock 请求失败、AWS 服务日志或 Bedrock 服务行为，请联系客户的 AWS 管理员或 AWS Support。