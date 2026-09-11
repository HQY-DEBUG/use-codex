> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/security-administration.md)。

<a id="security"></a>

# 安全管理概览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<CodexDocsOverviewLanding
  title="安全性"
  description="控制 ChatGPT 和 Codex 开发人员工具可以访问的内容，了解如何隔离工作，并对安全敏感任务应用保护措施。"
  intro="安全控制定义了 ChatGPT 和 Codex 开发人员工具可以访问的内容以及如何审查敏感操作。权限、沙箱、批准和网络访问建立了信任边界。 Codex Security 有助于查找和修复漏洞，网络安全指南解释了如何处理安全敏感工作。"
  primaryCta={{
    label: "探索权限",
    href:"permissions.zh-CN.md",
  }}
  hero={{
    illustration: "security",
    backgroundImage: "/images/codex/codex-wallpaper-1.webp",
    alt: "ChatGPT 默认、自动、完全和自定义访问的批准选项",
  }}
  sections={[
    {
      title: "权限",
      description:
        "控制文件系统、网络、命令、批准和审查行为。",
      pages: [
        {
          title: "权限",
          description:
            "选择文件系统、命令和网络访问的配置文件。",
          href:"permissions.zh-CN.md",
          icon: "lock",
        },
        {
          title: "沙箱",
          description:
            "了解 Codex 如何隔离命令和文件更改。",
          href:"sandboxing.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "自动审核",
          description:
            "根据您配置的策略自动检查操作。",
          href:"sandboxing/auto-review.zh-CN.md",
          icon: "dataControls",
        },
        {
          title: "智能体审批和安全",
          description: "决定 Codex 在采取行动之前何时必须询问。",
          href:"agent-approvals-security.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "互联网接入",
          description: "控制云聊天可以到达哪些域。",
          href:"cloud/internet-access.zh-CN.md",
          icon: "webSearch",
        },
      ],
    },
    {
      title: "Codex Security",
      description: "查找、理解和修复漏洞。",
      pages: [
        {
          title: "Codex Security概述",
          description:
            "评估代码并将审查结果转化为有针对性的修复。",
          href:"security.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "Codex Security插件",
          description:
            "从 ChatGPT 桌面应用程序和 Codex CLI 运行安全工作流程。",
          href:"security/plugin.zh-CN.md",
          icon: "plugin",
        },
        {
          title: "Codex Security CLI",
          description:
            "运行本地安全扫描并自动进行仓库审查。",
          href:"security/cli.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "Codex Security TypeScript SDK",
          description:
            "将安全扫描和进度报告集成到开发人员工具中。",
          href:"security/sdk.zh-CN.md",
          icon: "code",
        },
        {
          title: "Codex Security 云设置",
          description:
            "连接仓库并配置云安全扫描。",
          href:"security/setup.zh-CN.md",
          icon: "storage",
        },
        {
          title: "安全审查",
          description: "对 GitHub 拉取请求进行深入的安全审查。",
          href:"security/security-review.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "威胁模型",
          description: "检查并改进代码库的威胁模型。",
          href:"security/threat-model.zh-CN.md",
          icon: "webSearch",
        },
        {
          title: "Codex Security云常见问题解答",
          description:
            "获取有关云扫描、结果、隐私和访问的答案。",
          href:"security/faq.zh-CN.md",
          icon: "chat",
        },
      ],
    },
    {
      title: "网络安全",
      description: "选择经批准的模型并配置安全参与。",
      pages: [
        {
          title: "模型和可信访问",
          description:
            "选择网络安全模型并请求可信访问。",
          href:"cyber-safety.zh-CN.md",
          icon: "userLock",
        },
        {
          title: "推荐配置",
          description:
            "隔离环境、实施范围并审查敏感操作。",
          href:"cyber-safety/recommended-configuration.zh-CN.md",
          icon: "settings",
        },
      ],
    },
  ]}
/>