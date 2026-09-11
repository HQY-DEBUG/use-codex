> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/third-party/gitlab.md)。

<a id="review-gitlab-merge-requests-with-codex"></a>

# GitLab 合并请求审查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Codex 代码审查来获得 GitLab 合并请求的另一个高信号审查通过。 Codex 审查合并请求差异，遵循仓库指南，并发布针对严重问题的标准 GitLab 代码审查。

GitLab 支持处于测试阶段，所有 ChatGPT 计划均可用。 Codex 集成在 Codex 云中运行。桌面应用程序中的 GitHub 样式仓库控件（例如 **创建拉取请求**）不包含在此测试版中。

<a id="before-you-start"></a>

## 开始之前

确保您有：

- 已连接的 GitLab 帐户。 GitLab.com需要[标准连接流程](https://help.openai.com/articles/20001486)；自管理或专用 GitLab 实例需要 [工作区管理模板设置](https://help.openai.com/articles/20001487)。
- 如果您希望 Codex 遵循仓库特定的审核指南，则需要 `AGENTS.md` 文件。

<a id="set-up-codex-code-review"></a>

## 设置 Codex 代码审查

<a id="set-up-the-gitlab-connection-and-codex-review-identity"></a>

### 设置 GitLab 连接和 Codex 审核身份

对于 GitLab.com，一旦您拥有 [连接到 ChatGPT 中的 GitLab](https://help.openai.com/articles/20001486)，请在 Codex 中连接您的 GitLab 帐户。对于自我管理或专用 GitLab，每个审阅者应在 [工作区管理模板](https://help.openai.com/articles/20001487) 发布后进行连接。

对于自我管理或专用 GitLab，打开 **Codex 云** → **设置** → [**连接器**](https://chatgpt.com/codex/cloud/settings/connectors)。工作区管理员可以让 Codex 创建服务帐户或保存现有服务帐户个人访问令牌。

<a id="let-codex-create-the-account"></a>

#### 让 Codex 创建帐户

在 **Codex 云** → **设置** → **连接器** 中，为您的自管理或专用 GitLab 主机选择应用程序 → 选择 **设置服务帐户** → **创建服务帐户**。完成设置的工作区管理员必须具有 GitLab 实例的管理员访问权限。选择 **选定的组** 或 **仅限选定的项目**，然后选择 Codex 应在何处操作并创建帐户。组选项授予开发人员对每个选定组的访问权限，并由其项目和子组继承；项目选项仅授予开发人员对您选择的各个项目的访问权限。 Codex 将使用具有 `api` 范围的个人访问令牌创建 ChatGPT Codex 连接器实例服务帐户。

<a id="use-an-existing-account"></a>

#### 使用现有帐户

在GitLab中，创建或选择一个服务帐户，并仅在Codex应操作的组或项目中授予其开发人员访问权限。从 **服务账户** 页面中，选择账户 → **管理访问令牌** → **添加新令牌** 到 [创建个人访问令牌](https://docs.gitlab.com/user/profile/service_accounts/#create-a-personal-access-token-for-a-service-account)，范围为 `api`，且到期日期至少还有 30 天。返回 Codex，选择 **使用现有服务帐户**，粘贴令牌，然后选择 **保存令牌**。令牌在保存时会被加密，并且不会再次显示。

<a id="manage-the-service-account-token"></a>

#### 管理服务帐户令牌

工作区管理员可以管理 **Codex 云** → **设置** → **连接器** 中的服务帐户。对于 Codex 创建的帐户，管理员可以撤销当前令牌并生成新令牌。对于现有帐户，管理员可以替换或删除 Codex 中保存的令牌，并根据需要在 GitLab 中单独撤销它。在配置有效令牌之前，Codex 无法响应 GitLab 活动。

<a id="choose-how-gitlab-activity-reaches-codex"></a>

### 选择 GitLab 活动如何到达 Codex

<a id="create-a-project-environment-for-coding-tasks-or-project-specific-setup"></a>

#### 创建用于编码任务或特定于项目的设置的项目环境

在 **Codex 云** → **设置** → **环境** 中，当您希望 Codex 为该项目编写或执行代码（例如，编辑文件、提交更改或将更新推送到合并请求分支）时，或者当审查取决于项目特定的机密、网络访问或设置命令时，选择 GitLab 项目并创建项目环境。

对于GitLab.com，还需要一个项目环境来启用Codex评论。

创建环境时，打开 **从 GitLab 启用 Codex 活动** 以安装项目 Webhook，该 Webhook 向 Codex 传递合并请求、评论和问题事件。创建项目 Webhook 需要维护者或所有者访问权限、管理员访问权限或可以管理项目 Webhook 的自定义角色。签名的项目和组 Webhook 需要 GitLab 19.0 或更高版本。在自管理 GitLab 19.0 上，确认 `webhook_signing_token` 功能标志已启用；它默认启用，并在 GitLab 19.1 中删除。

<a id="enable-activity-for-codex-reviews-for-projects-across-a-gitlab-group"></a>

#### 为 GitLab 组中的项目启用 Codex 审核活动

对于自我管理或专用 GitLab，工作区管理员可以打开 **环境** → **GitLab 活动** → **管理群组** 以启用跨组及其子组的 Codex 审核。 Codex 将安装一个覆盖整个组项目的组 webhook。连接的 GitLab 用户必须是组所有者，组 Webhook 需要 GitLab Premium 或 Ultimate 以及 GitLab 19.0 或更高版本。

小组活动可以进行代码审查，但不会创建项目环境。要运行 GitLab 触发的编码任务（例如编辑文件、运行命令、提交更改或将更新推送到合并请求），请创建项目环境。

<a id="configure-code-review-policies"></a>

### 配置代码审查策略

在[Codex 审核设置](https://chatgpt.com/codex/cloud/settings/code-review?provider=gitlab)中配置代码审查策略。选择仓库策略：`Review my MRs`、`Review team MRs`、`Review all MRs` 或 `Follow personal`。然后选择审核运行时间：**MR打开时**、**每次推动时** 或 **智能触发器（实验）**。仓库设置可以覆盖个人默认设置。

<a id="request-a-codex-review"></a>

## 请求 Codex 审核

1. 在合并请求评论中，提及 `@codex review`。
2. 等待 Codex 反应 (👀) 并发表评论。

Codex 发布关于合并请求的 GitLab 讨论和注释，就像队友一样。默认情况下，手动请求的审核可以包括 P0、P1 和 P2 结果，而自动审核则重点关注 P0 和 P1 结果。

<a id="enable-automatic-reviews"></a>

## 启用自动评论

要自动审查合格的合并请求，请在 Codex 设置中打开 **自动评论**，选择 GitLab 仓库策略，然后选择触发器：**MR打开时**、**每次推动时** 或 **智能触发器（实验）**。当合并请求事件与该策略和触发器匹配时，Codex 在没有 `@codex review` 注释的情况下运行。

GitLab 活动必须通过项目 Webhook 或祖先组 Webhook 启用。对于自我管理或专用 GitLab，配置的服务帐户还必须具有写回项目的权限。 Codex 使用已配置的项目环境（如果存在）。如果祖先组已启用活动，则后代项目将继承该覆盖范围。

<a id="customize-what-codex-reviews"></a>

## 自定义 Codex 评论内容

Codex 在您的仓库中搜索 `AGENTS.md` 文件并遵循适用的代码审查规则。将 `## Code Review Rules` 部分添加到最接近规则管辖的代码的文件中。如果有帮助，请使用 `###` 标题对相关检查进行分组。

例如，实验报告服务可以防止暴露后行为改变比较队列：

```md
## 代码审查规则

### 实验队列

- 不要过滤暴露后行为的治疗比较，包括转化或保留。
  安全路径：通过分配或接触来建立队列；将转化报告为结果。
```

将仓库范围的规则放在根 `AGENTS.md` 中，将特定于服务的规则放在嵌套文件中，例如 `services/experiment_reporting/AGENTS.md`。 Codex 应用涵盖每个更改文件的根和更具体的指南，因此不相关的更改不必携带特定于服务的上下文。

从两到三个简明规则开始，这些规则对审阅者经常解释的检查进行编码。有用的规则：

- **专注于相应的、特定于仓库的行为。** 描述兼容性约束、数据边界或标记的不安全副作用及其重要性。
- **说明安全路径或例外情况。** 为 Codex 提供足够的上下文，以区分真实问题和预期行为。
- **保持规则的范围和持久性。** 更喜欢结果而不是可以更改的函数名称，并将指导放在其管辖的代码附近。
- **将机械检查留在 CI 中。** 将格式、lint 和其他确定性检查排除在审核规则之外。

打开代表性合并请求并请求 `@codex review` 进行审核。根据您看到的发现和反馈完善规则，并缩小或删除产生噪音的指导。

代码审查规则指南Codex；它们不会取代测试、分支保护或所需的批准。

对于一次性焦点，请将其添加到您的合并请求评论中：

`@codex review for issues in the database migration`

<a id="act-on-review-findings"></a>

## 根据审查结果采取行动

修复审查结果需要 **配置好的项目环境**；单独的小组活动支持评审，但无法运行编码任务。如果项目有环境，请让 Codex 通过留下另一条评论来修复同一合并请求中的问题：

```md
@codex 修复 P1 问题
```

Codex 以合并请求作为上下文启动 [云聊天](../cloud.zh-CN.md)，并且可以在有权执行此操作时将修复推送回分支。

<a id="give-codex-other-tasks"></a>

## 给Codex其他任务

其他编码任务也需要 **配置好的项目环境**；小组活动本身就支持评论。如果您在评论中提及 `@codex` 以及 `review` 以外的任何内容，则 Codex 使用您的合并请求作为上下文来启动 [云聊天](../cloud.zh-CN.md)。

```md
@codex 修复 CI 失败
```

<a id="troubleshoot-code-review"></a>

## 解决代码审查问题

如果 Codex 没有反应或发表评论：

- 确认已选择所需的 GitLab 应用程序；如果您使用特定于项目的设置，请确认项目具有预期的 Codex 云环境。
- 确认项目或祖先组的活动。在 GitLab 中，检查 **网络钩子** → [**近期活动**](https://docs.gitlab.com/user/project/integrations/webhooks/) 并验证合并请求和注释交付是否成功。
- 对于自我管理或专用 GitLab，请确认项目或组 Webhook 已签名、SSL 验证已启用，并且实例位于 GitLab 19.0 或更高版本上。在自管理 GitLab 19.0 上，确认 `webhook_signing_token` 功能标志已启用；修复故障后自动禁用的挂钩。
- 对于自我管理或专用 GitLab，请确认现有服务帐户个人访问令牌处于活动状态并且具有 `api` 范围。如果 Codex 创建了服务帐户，请确认它在 [Codex 连接器设置](https://chatgpt.com/codex/cloud/settings/connectors) 中正确配置并且项目或组已启用。
- 对于自我管理或专用 GitLab，请确认工作区服务帐户（而不仅仅是连接的 GitLab 用户）具有对项目或父组的开发人员访问权限，以便 Codex 可以发布评论和反应。会员资格是继承的；活动和服务帐户访问是分开的。
- 确认 **代码审查** 或 **自动评论** 已启用并且 MR 与仓库策略和触发器匹配。
- 使用`@codex review`。