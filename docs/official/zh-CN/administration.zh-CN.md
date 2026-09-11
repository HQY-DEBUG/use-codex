> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/administration.md)。

<a id="administration"></a>

# 管理概览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<CodexDocsOverviewLanding
  title="管理概览"
  description="为 ChatGPT、Codex 开发人员工具、API、插件和连接系统设置访问和策略边界。"
  intro="管理涵盖六个相关边界： ChatGPT 工作区访问； ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能的本地运行时策略； Codex 云资格；平台API接入；插件可用性和连接器权限；以及连接系统中的权限。从工作区身份和访问权限开始，然后应用每个部署所需的运行时和源系统控制。"
  primaryCta={{
    label: "探索身份验证",
    href:"auth.zh-CN.md",
  }}
  hero={{
    illustration: "administration",
    backgroundImage: "/images/codex/codex-wallpaper-1.webp",
    alt: "ChatGPT 工作区成员、组、访问令牌和角色控制",
  }}
  sections={[
    {
      title: "开始使用",
      description:
        "从部署指南开始，然后使用每个控制边界的参考页。",
      pages: [
        {
          title: "管理员推出指南",
          description:
            "规划访问权限、分配所有者、配置控制并验证部署。",
          href:"enterprise/admin-setup.zh-CN.md",
          icon: "users",
        },
      ],
    },
    {
      title: "ChatGPT Work",
      description:
        "查看 ChatGPT Work 概述和管理参考。",
      pages: [
        {
          title: "ChatGPT Work概述",
          description:
            "了解托管执行、网络控制、数据边界和审计可见性。",
          href:"enterprise/chatgpt-work-overview.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "ChatGPT Work 云安全",
          description:
            "查看托管执行、连接帐户、访问控制、保留和审计可见性。",
          href:"enterprise/chatgpt-work-cloud-security.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "ChatGPT Work 本地安全",
          description:
            "检查本地执行、设备和浏览器访问、托管策略、数据处理和审核限制。",
          href:"enterprise/chatgpt-work-local-security.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "ChatGPT Work 管理员常见问题解答",
          description:
            "查看 ChatGPT Work 的访问、数据、治理、使用和事件控制。",
          href:"enterprise/work-admin-faq.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "ChatGPT Work：用途和成本",
          description:
            "了解共享额度、计费影响、支出控制和采用规划。",
          href:"enterprise/chatgpt-work-usage-and-cost.zh-CN.md",
          icon: "dataControls",
        },
      ],
    },
    {
      title: "身份和认证",
      description:
        "选择人们如何登录并为编程工作流程颁发凭据。",
      pages: [
        {
          title: "身份验证概述",
          description:
            "比较登录方法、凭据存储和强制控制。",
          href:"auth.zh-CN.md",
          icon: "key",
        },
        {
          title: "工作负载身份",
          description:
            "让可信工作负载使用 Codex，无需长期凭证。",
          href:"enterprise/workload-identity.zh-CN.md",
          icon: "key",
        },
        {
          title: "个人访问令牌",
          description: "创建和管理用于编程访问的令牌。",
          href:"enterprise/access-tokens.zh-CN.md",
          icon: "lock",
        },
        {
          title: "服务账户",
          description:
            "创建和管理自动化工作流程的工作区身份。",
          href:"enterprise/service-accounts.zh-CN.md",
          icon: "robot",
        },
      ],
    },
    {
      title: "工作区访问、策略和模型",
      description:
        "分配 ChatGPT 工作区访问权限，并将其与本地运行时策略、Codex 云访问权限和平台 API 访问权限分开。",
      pages: [
        {
          title: "组和配置",
          description:
            "管理手动和 SCIM 组、配置和部署队列。",
          href:"enterprise/groups-and-provisioning.zh-CN.md",
          icon: "users",
        },
        {
          title: "用户生命周期管理",
          description:
            "配置员工、更新组访问权限并撤销离职用户的凭据。",
          href:"enterprise/user-lifecycle.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "角色和工作区权限",
          description:
            "使用工作区、运行时、API、插件和源系统控件的规范映射。",
          href:"enterprise/roles-and-workspace-permissions.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "GPT 和共享",
          description:
            "管理工作区中的 GPT 共享、所有权、连接的应用程序和第三方操作。",
          href:"enterprise/gpts-and-sharing.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "受管配置",
          description:
            "在受支持的地方分发托管设置，并强制执行 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能的运行时要求。",
          href:"enterprise/managed-configuration.zh-CN.md",
          icon: "dataControls",
        },
        {
          title: "Prisma AIRS",
          description:
            "将工作区范围的安全策略应用于 Codex 提示。",
          href:"enterprise/prisma-airs.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "HIPAA 配置",
          description:
            "为可能处理受保护的健康信息的工作流配置本地运行时保护措施。",
          href:"https://learn.chatgpt.com/codex/hipaa-configuration",
          icon: "shieldCheck",
        },
        {
          title: "工作区模型可用性",
          description:
            "ChatGPT 桌面应用程序、Codex CLI、IDE 扩展、Codex 云和平台 API 中的 ChatGPT、Codex 的单独模型访问。",
          href:"enterprise/workspace-model-availability.zh-CN.md",
          icon: "settings",
        },
      ],
    },
    {
      title: "插件和连接器控件",
      description:
        "控制插件安装、捆绑技能、连接器支持的功能和连接服务访问。",
      pages: [
        {
          title: "插件控件",
          description:
            "管理插件可用性、连接器访问和操作以及源系统权限。",
          href:"enterprise/apps-and-connectors.zh-CN.md",
          icon: "connect",
        },
        {
          title: "插件管理",
          description: "从 GitHub 导入并同步工作区插件。",
          href:"enterprise/plugin-management.zh-CN.md",
          icon: "connect",
        },
        {
          title: "技能控制",
          description:
            "比较 ChatGPT 工作区、本地文件系统和插件技能控制。",
          href:"enterprise/skills.zh-CN.md",
          icon: "tools",
        },
      ],
    },
    {
      title: "使用、治理和合规性",
      description:
        "衡量采用情况并将报告或审核数据传送到拥有它的系统。",
      pages: [
        {
          title: "治理",
          description:
            "为每个问题选择正确的分析、支出和审计界面。",
          href:"enterprise/governance.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "管理插件",
          description:
            "使用管理插件来获取权限、批准和支持的管理工作流程。",
          href:"enterprise/admin-plugin.zh-CN.md",
          icon: "tools",
        },
        {
          title: "工作区分析",
          description:
            "查看工作区级别的 ChatGPT 采用情况和 Codex 使用情况。",
          href:"enterprise/workspace-analytics.zh-CN.md",
          icon: "dataControls",
        },
        {
          title: "分析API",
          description:
            "使用 Codex Analytics API 自动生成开发人员活动和代码审查报告。",
          href:"enterprise/analytics-api.zh-CN.md",
          icon: "code",
        },
        {
          title: "合规性 API 和审核事件",
          description:
            "导出审计和调查工作流程的活动记录。",
          href:"enterprise/compliance-api.zh-CN.md",
          icon: "userLock",
        },
      ],
    },
    {
      title: "部署和模型提供商",
      description:
        "部署和更新桌面应用程序、连接托管主机或配置受支持的外部模型提供程序。",
      pages: [
        {
          title: "管理应用程序更新",
          description:
            "通过设备管理平台控制桌面应用程序更新并部署批准的版本。",
          href:"enterprise/manage-app-updates.zh-CN.md",
          icon: "settings",
        },
        {
          title: "Windows 应用程序部署",
          description:
            "选择托管 Windows 设备的安装和更新路径。",
          href:"enterprise/windows-deployment.zh-CN.md",
          icon: "settings",
        },
        {
          title: "远程连接",
          description: "启动和控制连接的计算机上的工作。",
          href:"remote-connections.zh-CN.md",
          icon: "connect",
        },
        {
          title: "亚马逊基岩",
          description:
            "配置支持的本地客户端以使用通过 Bedrock 提供的模型。",
          href:"amazon-bedrock.zh-CN.md",
          icon: "storage",
        },
      ],
    },
  ]}
/>