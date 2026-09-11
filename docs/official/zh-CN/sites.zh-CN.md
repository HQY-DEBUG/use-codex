> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/sites.md)。

<a id="sites"></a>

# Sites 网站工具

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Sites 处于公开测试阶段，可通过 ChatGPT Plus、Pro、Business、Enterprise 和 Edu 计划使用。测试期间，特定计划的使用限制适用于所有站点。 ChatGPT 显示当前限制并在您接近限制时通知您。达到限制可能会阻止您创建站点、添加存储或将高使用率站点保持为公开状态，但您仍然可以编辑和管理现有站点。

站点允许 ChatGPT 创建、托管、完善和共享网站、Web 应用程序和游戏。当您想要将提示或兼容的现有项目转变为托管体验而无需设置单独的部署工作流程时，请使用站点。

<ContentModeSwitch group="codex-surface" id="app">

在 ChatGPT 桌面应用程序中打开 **站点**。您可以从提示符或兼容的本地项目启动站点，然后返回到站点视图进行管理。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

使用 Web 上 ChatGPT 中的站点来创建和管理托管站点。选择 **更多** > **站点**，或直接转到 [chatgpt.com/sites](https://chatgpt.com/sites)，查找您创建的站点。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

站点没有独立的 Codex CLI 管理视图。使用 ChatGPT Web 或桌面应用程序创建、保存、部署和管理站点项目。您仍然可以在发布之前使用 Codex CLI 编辑和测试本地项目。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

站点没有独立的 IDE 扩展管理视图。使用 ChatGPT Web 或桌面应用程序进行站点操作，并使用 IDE 扩展来编辑和测试本地源项目。

</ContentModeSwitch>

每个站点部署 URL 都是生产部署。如果您想在构建生效之前对其进行审查，请要求 ChatGPT 保存版本而不部署它。

<a id="get-started-with-sites"></a>

## 开始使用网站

在 ChatGPT 中，在提示中包含“网站”一词或提及 `@Sites` 以显式启动站点工作流程。

<WorkflowSteps variant="headings">

1. 描述网站

描述网站应使用的受众、目的、所需行为和信息。

2. 查看网站

查看生成的内容和行为。检查网站是否使用预期信息并按预期处理数据。

3. 完善网站

描述你想要的改变。添加相关文件或视觉上下文有助于 ChatGPT 进行更改。

4. 管理和共享网站

返回 **站点** 重新打开或完善网站。准备就绪后，选择谁可以访问它并共享生成的链接。

</WorkflowSteps>

<ContentModeSwitch group="codex-surface" id="web">

在预览中，选择 **编辑**。在 **描述网站编辑** 下，描述您想要的更改。当附加上下文有帮助时，请使用 **截图** 或 **添加文件等**。

</ContentModeSwitch>

<a id="prompt-sites-for-common-tasks"></a>

## 常见任务的提示站点

对于新网站、仪表板或内部工具，请包括受众、核心体验和所需信息：

```text
为我的运营团队构建项目请求仪表板。让团队成员
提交请求，查看谁拥有每个请求，更新状态并过滤列表。
要求人们使用其工作区帐户登录并保留请求
访问之间保存的数据。
```

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

对于现有项目，请要求协作平台准备并发布当前应用程序：

```text
使用站点部署此项目。检查是否兼容，进行任意设置
所需的更改，并给我部署 URL。
```

</ContentModeSwitch>

当站点需要持久的应用程序数据或上传的文件时，请在请求中说明：

```text
添加玩家分数和头像上传到此游戏。保留分数并上传
两次访问之间的头像。
```

浏览 [站点展示](https://developers.openai.com/showcase) 以获取已部署的内部应用程序以及用于创建它们的完整提示。

<a id="review-site-analytics"></a>

## 查看网站分析

站点会自动记录流量，因此您可以了解人们如何使用已部署的站点，而无需添加分析 SDK。分析视图显示总的唯一访问者和页面视图，以及一段时间内的这两个指标。更改日期范围或粒度以检查不同的时期。

<ContentModeSwitch group="codex-surface" id="app">

打开 **站点**，找到站点，然后选择 **更多行动** > **分析**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

转到 [chatgpt.com/sites](https://chatgpt.com/sites)，找到站点，然后选择 **更多行动** > **分析**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="cli,ide">

站点在 CLI 或 IDE 扩展中没有独立的分析视图。在网络上或桌面应用程序中打开 ChatGPT 中的站点以查看其分析。

</ContentModeSwitch>



> 插图：交互式网站分析仪表板显示 7 天内的唯一访问者和页面视图。



分析目前可用于不属于企业工作区的站点。

<a id="add-sign-in-with-chatgpt"></a>

## 添加使用ChatGPT登录

公共站点可以保持对所有人开放，同时提供可选的使用 ChatGPT 登录的身份识别功能，例如保存的进度、个性化视图或属于特定人员的记录。工作区受限站点已使用 ChatGPT 身份来强制执行其共享设置。

要求网站添加登录体验：

```text
将使用 ChatGPT 登录添加到此公共站点。确保网站可供退出的访问者使用。当某人注销时，显示使用 ChatGPT 登录操作。他们登录后，请使用他们的全名（如果有）或电子邮件地址向他们打招呼。添加注销操作，并将授权决策保留在服务器端代码中。
```

<ToggleSection title="它是如何运作的">

站点通过平台提供的路径处理登录和注销流程，然后将访问者返回到您的站点：

```html
<a href="/signin-with-chatgpt">Sign in with ChatGPT</a>
<a href="/signout-with-chatgpt">Sign out</a>
```

访问者登录后，站点通过以下请求标头将其身份转发到服务器：

- `oai-authenticated-user-email` 包含经过身份验证的电子邮件地址。
- `oai-authenticated-user-full-name` 可能包含非空配置文件名称。将其视为可选并回退到电子邮件地址。

将授权决策保留在服务器端代码中，并且不依赖于名称分割标头。

</ToggleSection>

<a id="understand-projects-versions-and-deployments"></a>

## 了解项目、版本和部署

站点是持久托管的输出，您可以从 ChatGPT 中的 **站点** 重新打开、优化、配置和共享该输出。

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

站点项目将本地源项目链接到通过站点管理的托管。站点将该链接和可选存储绑定名称存储在 `.openai/hosting.json` 中。新创建的本地启动器可以在没有 `project_id` 的情况下启动； Sites 在配置托管项目后会添加一项。

例如，使用关系数据库绑定并且没有文件存储的配置站点可以包含：

```json
{
  "project_id": "<project-id>",
  "d1": "DB",
  "r2": null
}
```

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

即使创建站点的 ChatGPT Work 聊天结束后，该站点也会出现在您的站点列表中。您不需要本地项目或清单即可在网络上启动站点。站点与 ChatGPT 项目是分开的。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

网站发布有两个独立的阶段：

1. **保存一个版本。** ChatGPT 构建可部署版本。对于本地源项目，ChatGPT 将版本与用于构建的 Git 提交相关联。当您需要可审查的部署候选时，请使用此阶段。
2. **部署一个版本。** ChatGPT 发布保存的版本并在部署成功时报告生产 URL。仅当您打算让选定的受众访问该网站时才使用此选项。

当您需要识别以前的部署候选时，要求 ChatGPT 列出或检查保存的版本。

</ContentModeSwitch>

<a id="choose-a-supported-site-shape"></a>

## 选择支持的网站形状

对于新项目，站点工作流程可以使用其推荐的站点启动器启动。对于现有项目，请在请求部署之前要求 ChatGPT 确认该项目可以生成兼容的部署工件。

告诉 ChatGPT 您需要的产品行为，以便它可以选择适当的站点形状：

| 网站需要 | 需要什么 网站需要 |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 以内容为主导的网站或登陆页面 | 没有持久应用程序状态的站点，除非经验需要 |
| 保存的记录、用户进度或游戏分数 | D1，持久结构化数据的关系数据库 |
| 图片、文档、音频、视频或其他上传 | R2，文件的对象存储 |
| 上传的文件包含可搜索元数据 | D1 用于元数据，R2 用于文件内容 |
| 需要当前工作区用户身份的内部站点 | 工作区验证的用户身份 |
| 公共登录或外部身份提供商 | 启用身份验证的站点 |

不要为临时演示状态请求持久存储，例如主题选择或取消的横幅。请请求它提供人们希望托管网站记住的产品数据。

<a id="control-access-and-secrets"></a>

## 控制访问和秘密

在您更改新站点的访问权限之前，新站点仅限于其所有者和工作区管理员。在查看内容、数据处理和预期受众时，保持访问受限。

根据您的帐户和工作区设置，共享选项可以包括：

- **所有者和工作区管理员**
- **选定的活跃用户或组**，如果支持的话
- **邀请外部观众**，当有外部邀请时
- **工作区中的任何人**，如果支持的话
- **互联网上的任何人**，仅当启用公共发布时

访客访问允许人们打开网站；它不给他们编辑权限。在企业工作区中，公共发布默认处于关闭状态，必须由管理员启用。

对于有限共享，受邀访问者必须使用获得访问权限的帐户登录。无需访问 ChatGPT 工作区即可使用公共站点。站点的受众设置和站点内置的任何登录功能都是单独的控件。

例如：

```text
向我展示后，更改此网站对我工作区中每个人的访问权限
当前站点并确认其 URL。
```

<a id="invite-people-outside-your-workspace"></a>

### 邀请工作区之外的人

通过外部邀请，您可以授予指定人员访问网站的权限，而无需将其公开。您可以邀请工作区之外的查看者，或从个人帐户共享私人网站。该功能正在向 Plus、Pro、Business 和 Enterprise 套餐的 Sites 用户推出。

<WorkflowSteps>

1. 打开您拥有的站点并选择 **分享**。
2. 要保持站点私有，请将 **谁有权访问** 设置为 **仅限受邀者**。
3. 在 **搜索人员或群组** 下输入查看者的电子邮件地址，或者在个人网站的 **输入电子邮件地址** 下输入查看者的电子邮件地址，然后选择收件人。
4. 查看受众和收件人的 **观众** 访问权限，然后选择 **邀请**。
5. 确认查看器出现在保存的访问列表中。共享网站的链接并要求他们使用获得访问权限的帐户登录。

</WorkflowSteps>

外部查看者可以打开并使用该网站。他们不会成为工作区成员或网站编辑者，也无法编辑或发布网站。该邀请授予访问本网站的权限；在共享之前查看其内容和连接的数据。

在 Enterprise 中，管理员在 **工作区设置 > 权限和角色** 下管理 **允许成员邀请外部访问者访问网站**。此权限与公开发布网站的权限是分开的。业务工作区没有单独的外部邀请权限切换；站点必须已启用，并且该功能必须可供帐户使用。如果缺少邀请选项，请检查所选帐户、站点所有权、工作区权限和推出可用性。

要删除查看者，请打开站点的共享控件并删除其访问权限。另请检查剩余的受众设置：删除一项邀请并不会删除此人通过公共、工作区或群组共享所拥有的访问权限。

<a id="collaborate-on-a-site"></a>

### 在网站上进行协作

站点协作需要工作区。当该功能可用时，网站所有者可以邀请同一工作区的活跃成员作为编辑者。

编辑者可以读取网站的实时数据库数据。仅邀请您信任且掌握网站代码和数据的人员。

<WorkflowSteps>

1. 打开站点并选择 **分享**。
2. 在 **添加人员或群组** 下，查找并选择一个工作区成员。他们被添加为访客。
3. 打开该人旁边的 **可以查看** 并选择 **可以编辑**。 Access 自动保存。该站点显示在成员的站点视图中的 **与您分享** 下。
4. 在所有者首次发布网站后，编辑者可以打开网站、进行更改、保存版本和发布更新。

</WorkflowSteps>

网站所有者管理编辑者访问权限，并可以将现有访问者提升为编辑者、将编辑者更改为 **可以查看** 或删除其访问权限。共同编辑不会添加单独的工作区权限切换。

编辑者无法更改网站的受众、邀请或删除其他人、管理设置或分析、恢复早期版本或转让所有权。编辑者也无法执行网站的首次发布；所有者必须先发布网站，然后编辑者才能发布以后的更新。

编辑者访问权限与访问者访问权限是分开的。上述步骤首先将该人添加为访客，然后授予编辑权限。将访问者提升为编辑者不会改变网站的受众设置。

<a id="configure-runtime-environment-values"></a>

### 配置运行时环境值

打开 **站点**，然后打开站点的设置以添加、更新或删除托管环境变量和机密。将秘密值保留在提示、附加文件和站点内容之外。

<ContentModeSwitch group="codex-surface" id="web">

转到 [chatgpt.com/sites](https://chatgpt.com/sites)，找到站点，然后选择 **更多行动** > **设置**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

不要将这些值存储在 `.openai/hosting.json` 中。使本地 `.env` 和 `.env.example` 文件与本地开发所需的密钥保持一致，并且不要提交秘密值。

当您添加、更新或删除托管环境值时，请要求 ChatGPT 重新部署已批准的已保存版本，以便下一次部署使用更新的配置。

</ContentModeSwitch>

<a id="change-a-site-url"></a>

## 更改站点 URL

在可以进行 URL 编辑的情况下，站点所有者可以更改现有站点的 ChatGPT 托管 URL，而无需创建另一个部署。

1. 打开 **站点**，找到该站点，然后打开其设置。
2. 找到站点 URL 并选择 **更改网址**。
3. 输入可用的名称。它必须包含至少五个字符，以小写字母开头，并且仅使用小写字母、数字和单个连字符。它不能以连字符结尾或包含连续的连字符。
4. 确认更改并等待站点更新地址。

URL 更改不会创建另一个部署。前一地址重定向到新地址，包括路由和查询参数。

更改 ChatGPT 托管的 URL 不会添加、删除或更改自定义域。自定义域是一个单独的现有功能；当该功能可用时，使用自定义域设置。

<a id="connect-a-custom-domain"></a>

## 连接自定义域

在可用自定义域的情况下，您可以连接您已拥有的顶级域或子域。网站不会为您注册域，因此您必须能够更改域的 DNS 记录。自定义域在启动时在企业工作区中不可用。

连接域：

1. 打开站点设置并选择 **添加域名**。
2. 输入您要使用的顶级域或子域。
3. 复制站点提供的 DNS 记录和值，然后通过您的域提供商添加它们。
4. 等待几分钟，然后返回站点设置并刷新域状态。

您还可以要求 ChatGPT 帮助将域名指向您的站点。如果启用了浏览或计算机使用，ChatGPT 可以在您登录后帮助您导航域名提供商。

<a id="review-before-you-share"></a>

## 分享前先回顾一下

在共享网站之前：

- 查看其内容、生成的文本和图像、链接、上传的文件、表单和交互行为。
- 确认它不会泄露机密或敏感信息、秘密值或您无权共享的第三方内容。
- 从预期的访问者体验（包括其访问和登录行为）测试网站。
- 查看收集个人信息或其他访问者内容的功能。决定网站是否应收集、共享或发布该信息。
- 如果网站使用 ChatGPT 登录，请解释其收到的访问者信息以及如何使用该信息。
- 如果本网站收集或处理个人数据，请遵守 [适用的隐私和数据保护法](https://help.openai.com/en/articles/20001340)。
- 选择适合目标受众的最窄共享选项。
- 打开共享站点并确认目标受众可以访问它。

<ContentModeSwitch group="codex-surface" id="app">

对于从本地项目构建的站点，还应检查 Codex [审阅窗格](code-review.zh-CN.md) 中的源更改和任何数据库迁移。

</ContentModeSwitch>

<a id="take-down-or-delete-a-site"></a>

## 撤下或删除网站

要在不删除站点的情况下删除访问权限，请打开其共享设置并将访问权限限制为您自己或选定的人员。确认之前的观众无法再打开它。

要永久删除站点：

1. 打开 **站点** 并找到该站点。
2. 选择 **删除站点** 并按照提示中的说明进行操作。
3. 输入站点 slug，然后选择 **永久删除**。

删除站点会将其永久删除。您无法恢复已删除的网站。

<a id="understand-limits-and-unsupported-uses"></a>

## 了解限制和不支持的用途

站点托管在受支持的站点运行时中运行的 Web 体验。不支持某些框架、专用网络、数据库、后台服务和托管模式。

支持 HTTP、HTTPS 和 WebSocket。原始入站和出站 TCP 连接则不然。

每个站点都有以下存储限制：

| 资源 | 限制 |
| ------------------- | ---------------------- |
| D1 数据库存储 | 10 GB |
| R2 对象存储 | 无固定存储限制 |

站点在启动时不支持数据驻留或推理驻留。这包括部署的站点、站点代码、D1 和 R2 数据和文件存储、生成的工件和日志。

请勿使用网站处理受保护的健康信息或支付卡数据；针对 13 岁以下或适用的数字同意年龄的儿童；实现金融交易；传播恶意软件；启用网络钓鱼；冒充个人或组织；或以其他方式违反 OpenAI 政策。有关当前限制和政策链接，请参阅 [创建和管理 ChatGPT 站点](https://help.openai.com/en/articles/20001339)。

<a id="related-documentation"></a>

## 相关文档

<ContentModeSwitch group="codex-surface" id="app">

- [ChatGPT 桌面应用程序](app.zh-CN.md) 引入了应用程序导航、项目和聊天。
- [审核并发布变更](code-review.zh-CN.md) 解释了如何在发布源代码更改之前检查它们。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="cli,ide">

- [项目和聊天](projects.zh-CN.md) 解释了文件夹和工作区上下文如何跨聊天传递。
- [审核并发布变更](code-review.zh-CN.md) 解释了每个 Codex 客户的审核工作流程。
- [沙箱](sandboxing.zh-CN.md) 解释了本地执行边界。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

- [在 ChatGPT 中打开站点](https://chatgpt.com/sites) 返回您创建的站点。
- [项目和聊天](projects.zh-CN.md) 解释了如何将相关聊天记录和源文件保存在一起。
- [处理文件](artifacts-viewer.zh-CN.md) 解释了如何在 ChatGPT web 中查看生成的文件。

</ContentModeSwitch>