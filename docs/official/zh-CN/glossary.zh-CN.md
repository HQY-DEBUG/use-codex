> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/glossary.md)。

<a id="glossary"></a>

# 术语表

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用此术语表作为跨应用程序、CLI、IDE 扩展、云、SDK 和相关集成的 Codex 术语的快速参考。

<GlossaryTable
  client:load
  searchPlaceholder="按术语、定义或使用界面过滤"
  searchLabel="搜索词汇表术语"
  emptyStateMessage="没有与您的搜索匹配的词汇表术语。"
  maxVisibleEntries={100}
  options={[
    {
      key: "行动",
      href:"agent-approvals-security.zh-CN.md",
      appliesTo: "桌面应用程序、Web、移动、CLI、IDE 扩展、云",
      description:
        "由人员 ChatGPT 或 Codex 执行的操作，例如编辑文件、运行命令或使用连接的服务。",
    },
    {
      key: "智能体",
      href:"https://learn.chatgpt.com/codex",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description:
        "Codex 智能体根据上下文进行推理、使用工具并完成任务。",
    },
    {
      key: "AGENTS.md",
      href:"agent-configuration/agents-md.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description:
        "提供 Codex 持久指令的仓库或用户指导文件。",
    },
    {
      key: "分析仪表板",
      href:"enterprise/workspace-analytics.zh-CN.md",
      appliesTo: "企业",
      description:
        "用于 ChatGPT 工作区采用和以 Codex 为中心的报告的管理中心。",
    },
    {
      key: "API密钥登录",
      href:"auth.zh-CN.md#sign-in-with-an-api-key",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "使用 OpenAI API 密钥进行身份验证。",
    },
    {
      key: "审批政策",
      href:"agent-approvals-security.zh-CN.md#sandbox-and-approvals",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "Codex 在采取行动之前必须询问的规则。",
    },
    {
      key: "批准请求",
      href:"agent-approvals-security.zh-CN.md#automatic-approval-reviews",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "Codex 请求允许限制操作。",
    },
    {
      key: "应用程序（配置）",
      href:"plugins.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "Codex 配置和应用程序服务器字段，用于在 `apps` 名称下存储连接器设置。",
    },
    {
      key: "应用快照",
      href:"appshots.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "发送到 ChatGPT 或 Codex 聊天的最前面的应用程序窗口的快照。",
    },
    {
      key: "验证缓存",
      href:"auth.zh-CN.md#login-caching",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "Codex 重复使用的本地存储的登录凭据。",
    },
    {
      key: "自动审批审核",
      href:"agent-approvals-security.zh-CN.md#automatic-approval-reviews",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "在继续进行之前，对符合条件的批准请求进行基于模型的审查。",
    },
    {
      key: "计划任务",
      href:"automations.zh-CN.md",
      appliesTo: "桌面应用程序、网络",
      description:
        "提示 ChatGPT 在未来某个时间或按重复计划运行，具有自己的设置和运行历史记录。",
    },
    {
      key: "预定运行",
      href:"automations.zh-CN.md#managing-tasks",
      appliesTo: "桌面应用程序、网络",
      description:
        "计划任务的一次执行，包括其状态和任何结果结果。",
    },
    {
      key: "计算机在浏览器中使用",
      href:"https://learn.chatgpt.com/codex/browser?surface=app#app-computer-use-in-the-browser",
      appliesTo: "桌面应用程序",
      description:
        "让ChatGPT直接操作内置浏览器的功能。",
    },
    {
      key: "聊天",
      href:"projects.zh-CN.md#start-a-chat",
      appliesTo: "桌面应用程序、Web、移动、CLI、IDE 扩展、云",
      description:
        "用于与 ChatGPT 或 Codex 交换消息的保存空间，包括共享上下文、结果和操作。快速聊天从 Codex 开始 ChatGPT 聊天。",
    },
    {
      key: "ChatGPT 登录",
      href:"auth.zh-CN.md#sign-in-with-chatgpt",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description:
        "使用 ChatGPT 帐户和工作区权限进行身份验证。",
    },
    {
      key: "Computer History",
      href:"customization/computer-history.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "选择加入 macOS 功能，可根据允许的应用程序和网站之间的交互事件构建记忆和时间线。",
    },
    {
      key: "云",
      href:"cloud.zh-CN.md",
      appliesTo: "桌面应用程序、IDE 扩展、Web",
      description:
        "Codex 在 OpenAI 管理的环境中远程工作的模式。",
    },
    {
      key: "云环境",
      href:"environments/cloud-environment.zh-CN.md",
      appliesTo: "云",
      description: "用于 Codex 云聊天的已配置容器设置。",
    },
    {
      key: "云聊天",
      href:"environments/cloud-environment.zh-CN.md#how-codex-cloud-tasks-run",
      appliesTo: "云",
      description: "在云环境中远程运行的 Codex 聊天。",
    },
    {
      key: "Codex",
      href:"https://learn.chatgpt.com/codex",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、Web、云、SDK",
      description: "OpenAI 用于软件开发任务的编码智能体。",
    },
    {
      key: "ChatGPT 桌面应用程序",
      href:"app.zh-CN.md",
      appliesTo: "桌面",
      description:
        "带有 ChatGPT 和 Codex 的桌面应用程序，包括聊天和工作、项目、文件预览、计划任务和开发人员工具。",
    },
    {
      key: "Codex 应用程序服务器",
      href:"app-server.zh-CN.md",
      appliesTo: "桌面应用程序、IDE 扩展、SDK",
      description:
        "本地 JSON-RPC 服务器，用于在自定义客户端中嵌入 Codex 线程、轮次、批准、历史记录和流事件。",
    },
    {
      key: "Codex CLI",
      href:"https://learn.chatgpt.com/codex/cli",
      appliesTo: "终端",
      description:
        "用于以交互方式或在脚本中运行 Codex 的终端客户端。",
    },
    {
      key: "Codex云",
      href:"cloud.zh-CN.md",
      appliesTo: "Web、桌面应用程序、IDE 扩展",
      description:
        "OpenAI 托管执行环境，Codex 可以在其中远程处理仓库任务。",
    },
    {
      key: "法典执行",
      href:"non-interactive-mode.zh-CN.md",
      appliesTo: "命令行界面",
      description:
        "用于从脚本或 CI 以非交互方式运行 Codex 的 CLI 命令。",
    },
    {
      key: "Codex IDE扩展",
      href:"https://learn.chatgpt.com/codex/ide",
      appliesTo: "集成开发环境",
      description:
        "编辑器集成，可在 VS Code、JetBrains IDE、Cursor 和 Windsurf 等 IDE 中使用 Codex。",
    },
    {
      key: "Codex SDK",
      href:"codex-sdk.zh-CN.md",
      appliesTo: "软件开发工具包",
      description:
        "用于构建 Codex 支持的工作流程或集成的编程接口。",
    },
    {
      key: "Codex 管理的工作树",
      href:"environments/git-worktrees.zh-CN.md#codex-managed-and-permanent-worktrees",
      appliesTo: "桌面应用程序",
      description: "临时工作树 Codex 为聊天创建和管理。",
    },
    {
      key: "压实",
      href:"prompting.zh-CN.md#context",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description:
        "总结旧的背景，以便长期运行的工作可以继续。",
    },
    {
      key: "合规API",
      href:"enterprise/compliance-api.zh-CN.md",
      appliesTo: "企业",
      description:
        "用于导出支持的 ChatGPT 工作区记录和审核元数据的 API。",
    },
    {
      key: "电脑使用",
      href:"computer-use.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "桌面功能使 ChatGPT 通过 UI 与其他应用程序交互。",
    },
    {
      key: "配置文件",
      href:"config-file/config-reference.zh-CN.md#configtoml",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "本地Codex配置文件。",
    },
    {
      key: "连接主机",
      href:"remote-connections.zh-CN.md#what-comes-from-the-connected-host",
      appliesTo: "桌面应用程序、移动应用程序",
      description:
        "为通过远程打开的 ChatGPT 或 Codex 聊天提供文件、工具和 shell 访问的计算机或开发环境。",
    },
    {
      key: "连接器",
      href:"plugins.zh-CN.md",
      appliesTo: "桌面应用程序（ChatGPT Work、Codex）、网页（ChatGPT Work）",
      description:
        "将 ChatGPT 或 Codex 连接到外部服务中的数据和操作的插件组件。",
    },
    {
      key: "对话",
      href:"projects.zh-CN.md#start-a-chat",
      appliesTo: "桌面应用程序、Web、移动、CLI、IDE 扩展、云",
      description:
        "聊天中人员与 ChatGPT 或 Codex 之间持续交换消息和共享上下文。",
    },
    {
      key: "容器缓存",
      href:"environments/cloud-environment.zh-CN.md#container-caching",
      appliesTo: "云",
      description:
        "保存的云容器状态可重复使用以加快未来的云聊天速度。",
    },
    {
      key: "背景",
      href:"prompting.zh-CN.md#context",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云、SDK",
      description:
        "Codex 在工作时可以使用的信息，例如文件、先前消息、工具输出和指令。",
    },
    {
      key: "上下文窗口",
      href:"https://learn.chatgpt.com/api/docs/guides/conversation-state#managing-the-context-window",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云、SDK",
      description:
        "模型一次可以考虑的最大信息量。",
    },
    {
      key: "定制智能体",
      href:"agent-configuration/subagents.zh-CN.md#custom-agents",
      appliesTo: "桌面应用程序、CLI",
      description:
        "用户定义的智能体角色具有自己的指令和设置。",
    },
    {
      key: "拒绝读取规则",
      href:"permissions.zh-CN.md#deny-reads-with-exact-paths-or-globs",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、企业",
      description:
        "阻止 Codex 读取敏感路径或全局匹配的文件系统权限规则。",
    },
    {
      key: "差异",
      href:"https://learn.chatgpt.com/codex/code-review?surface=app#app-what-changes-it-shows",
      appliesTo: "桌面应用程序、Git、评论",
      description:
        "显示的一组 Git 文件更改以供检查、注释、暂存或恢复。",
    },
    {
      key: "域白名单",
      href:"cloud/internet-access.zh-CN.md#domain-allowlist",
      appliesTo: "云",
      description:
        "当启用智能体互联网访问时，Codex 云可以到达的域集。",
    },
    {
      key: "环境（当地）",
      href:"environments/local-environment.zh-CN.md",
      appliesTo: "桌面应用程序，工作树",
      description:
        "桌面应用程序配置告诉 Codex 如何为项目设置工作树。",
    },
    {
      key: "环境变量",
      href:"environments/cloud-environment.zh-CN.md#environment-variables-and-secrets",
      appliesTo: "云、CLI、IDE 扩展",
      description:
        "任务执行期间可用的运行时配置值。",
    },
    {
      key: "临时会话",
      href:"non-interactive-mode.zh-CN.md#basic-usage",
      appliesTo: "命令行界面",
      description:
        "非交互式运行，完成后会跳过保存会话状态。",
    },
    {
      key: "快速模式",
      href:"agent-configuration/speed.zh-CN.md#fast-mode",
      appliesTo: "CLI、IDE 扩展",
      description:
        "速度设置使支持的模型以更高的信用成本响应更快。",
    },
    {
      key: "文件系统权限",
      href:"permissions.zh-CN.md#filesystem-permissions",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "授予或拒绝对路径的读写访问权限的权限配置文件规则。",
    },
    {
      key: "寻找",
      href:"automations.zh-CN.md#managing-tasks",
      appliesTo: "桌面应用程序",
      description: "计划任务出现显着的结果或问题。",
    },
    {
      key: "完全访问",
      href:"sandboxing.zh-CN.md#configure-defaults",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "Codex 在没有正常沙箱限制的情况下运行的模式。",
    },
    {
      key: "Git 工作树",
      href:"environments/git-worktrees.zh-CN.md#whats-a-worktree",
      appliesTo: "桌面应用程序、Git",
      description:
        "对同一仓库进行第二次检查以进行并行分支工作。",
    },
    {
      key: "切换",
      href:"environments/git-worktrees.zh-CN.md#working-between-local-and-worktree",
      appliesTo: "桌面应用程序",
      description: "在本地和工作树之间移动聊天及其工作。",
    },
    {
      key: "心跳",
      href:"automations.zh-CN.md#schedule-a-task-inside-a-chat",
      appliesTo: "桌面应用程序",
      description:
        "将 ChatGPT 返回到同一聊天的重复计划任务。",
    },
    {
      key: "钩子",
      href:"hooks.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "当 Codex 事件匹配时运行的生命周期处理程序，例如工具使用、权限请求或对话轮次停止时。",
    },
    {
      key: "挂钩事件",
      href:"hooks.zh-CN.md#config-shape",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "配置的钩子处理程序可以运行的生命周期点。",
    },
    {
      key: "猛男",
      href:"https://learn.chatgpt.com/codex/code-review?surface=app#app-staging-and-reverting-files",
      appliesTo: "桌面应用程序、Git、评论",
      description:
        "差异的连续部分，可以独立地暂存、取消暂存或恢复。",
    },
    {
      key: "内嵌评论",
      href:"https://learn.chatgpt.com/codex/code-review?surface=app#app-inline-comments-for-feedback",
      appliesTo: "桌面应用程序",
      description: "附加到差异的特定于行的反馈。",
    },
    {
      key: "实时网络搜索",
      href:"config-file/config-basic.zh-CN.md#web-search-mode",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "实时网络查找当前信息。",
    },
    {
      key: "本地",
      href:"environments/git-worktrees.zh-CN.md#working-between-local-and-worktree",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "Codex 在用户计算机上工作的模式。",
    },
    {
      key: "本地聊天",
      href:"environments/modes.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "在用户计算机上运行的 ChatGPT 或 Codex 聊天。",
    },
    {
      key: "维护脚本",
      href:"environments/cloud-environment.zh-CN.md#container-caching",
      appliesTo: "云",
      description: "当缓存的云容器恢复时运行可选脚本。",
    },
    {
      key: "受管配置",
      href:"enterprise/managed-configuration.zh-CN.md",
      appliesTo: "企业",
      description: "组织控制的 Codex 默认值和限制。",
    },
    {
      key: "MCP",
      href:"extend/mcp.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "模型上下文协议，用于将 Codex 连接到外部工具和上下文的标准。",
    },
    {
      key: "MCP资源",
      href:"extend/mcp.zh-CN.md#supported-mcp-features",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "MCP 服务器公开的可读上下文，供 Codex 检查。",
    },
    {
      key: "MCP服务器",
      href:"extend/mcp.zh-CN.md#supported-mcp-features",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "通过 MCP 公开的外部工具或上下文提供程序。",
    },
    {
      key: "MCP工具",
      href:"extend/mcp.zh-CN.md#supported-mcp-features",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "由 MCP 服务器公开的操作，Codex 在任务期间可以调用该操作。",
    },
    {
      key: "主数据管理",
      href:"enterprise/managed-configuration.zh-CN.md#macos-managed-preferences-mdm",
      appliesTo: "企业",
      description:
        "用于分发设备配置文件和托管 Codex 设置的移动设备管理工具。",
    },
    {
      key: "回忆",
      href:"customization/memories.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "本地存储的上下文 Codex 可以跨会话重用。",
    },
    {
      key: "模型",
      href:"models.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云、SDK",
      description: "AI模型Codex用于推理和工具工作。",
    },
    {
      key: "网络接入",
      href:"agent-approvals-security.zh-CN.md#network-access",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description:
        "命令或环境访问互联网的权限。",
    },
    {
      key: "网络政策",
      href:"agent-approvals-security.zh-CN.md#network-policy",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "基于域的允许和拒绝规则，限制沙箱出站网络流量。",
    },
    {
      key: "非交互模式",
      href:"non-interactive-mode.zh-CN.md",
      appliesTo: "命令行界面",
      description: "用于从脚本或 CI 运行 Codex 的 CLI 模式。",
    },
    {
      key: "输出模式",
      href:"non-interactive-mode.zh-CN.md#create-structured-outputs-with-a-schema",
      appliesTo: "命令行界面",
      description:
        "JSON Schema 传递给 `codex exec` 以约束最终响应。",
    },
    {
      key: "永久工作树",
      href:"environments/git-worktrees.zh-CN.md#codex-managed-and-permanent-worktrees",
      appliesTo: "桌面应用程序",
      description: "一个长期存在的工作树作为其自己的项目保留。",
    },
    {
      key: "权限配置文件",
      href:"permissions.zh-CN.md#define-and-select-a-profile",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "命名的最小权限策略结合了文件系统和网络规则以执行本地命令。",
    },
    {
      key: "计划",
      href:"https://learn.chatgpt.com/codex/learn/best-practices#plan-first-for-difficult-tasks",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description: "Codex 建议或跟踪的完成任务的步骤。",
    },
    {
      key: "插件",
      href:"plugins.zh-CN.md",
      appliesTo: "桌面应用程序（ChatGPT Work、Codex）、Web（ChatGPT Work）、CLI",
      description:
        "可安装的功能包，例如技能、连接器和工具，通过 ChatGPT 和 Codex 共享的通用目录分发。",
    },
    {
      key: "插件清单",
      href:"https://developers.openai.com/plugins/build/plugins#plugin-structure",
      appliesTo: "插件创作",
      description:
        "插件元数据文件，用于标识插件并指向捆绑技能、连接器映射、MCP 服务器、挂钩和元数据。",
    },
    {
      key: "前缀规则",
      href:"agent-configuration/rules.zh-CN.md#understand-the-rules-language",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、企业",
      description:
        "允许、提示或禁止匹配命令前缀的命令规则模式。",
    },
    {
      key: "公司简介",
      href:"config-file/config-advanced.zh-CN.md#profiles",
      appliesTo: "CLI、IDE 扩展",
      description: "Codex 的命名配置预设。",
    },
    {
      key: "渐进式披露",
      href:"build-skills.zh-CN.md",
      appliesTo: "桌面应用程序、Web (ChatGPT Work)、CLI、IDE 扩展",
      description:
        "仅在需要保留上下文时加载技能详细信息。",
    },
    {
      key: "项目",
      href:"projects.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "一组相关的聊天和共享源，或用于基于文件的工作的本地文件夹。",
    },
    {
      key: "提示",
      href:"prompting.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云、SDK",
      description: "发送至 ChatGPT 或 Codex 的问题、指令或目标。",
    },
    {
      key: "拉取请求审查",
      href:"https://learn.chatgpt.com/codex/code-review?surface=app#app-pull-request-reviews",
      appliesTo: "桌面应用程序、CLI、GitHub",
      description: "Codex 审查拉取请求的更改或反馈。",
    },
    {
      key: "RBAC",
      href:"enterprise/roles-and-workspace-permissions.zh-CN.md",
      appliesTo: "企业",
      description: "基于角色的工作区权限访问控制。",
    },
    {
      key: "只读模式",
      href:"sandboxing.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "Codex可以检查但未经批准不得修改的模式。",
    },
    {
      key: "推理努力",
      href:"config-file/config-basic.zh-CN.md#reasoning-effort",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、SDK",
      description:
        "控制模型使用多少推理预算的设置。",
    },
    {
      key: "远程连接",
      href:"remote-connections.zh-CN.md",
      appliesTo: "桌面应用程序、移动应用程序",
      description:
        "连接允许您通过连接的主机访问另一台设备上的 ChatGPT 或 Codex 聊天。",
    },
    {
      key: "需求.toml",
      href:"config-file/config-reference.zh-CN.md#requirementstoml",
      appliesTo: "企业",
      description: "针对托管 Codex 设置的管理员强制要求文件。",
    },
    {
      key: "审阅窗格",
      href:"code-review.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "用于检查差异、注释和 Git 更改的桌面应用程序视图。",
    },
    {
      key: "规则",
      href:"agent-configuration/rules.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "允许、提示或拒绝命令前缀或权限例外的策略。",
    },
    {
      key: "沙箱",
      href:"sandboxing.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "强制限制 Codex 命令可以访问或修改的内容。",
    },
    {
      key: "沙箱模式",
      href:"config-file/config-basic.zh-CN.md#sandbox-level",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "定义 Codex 的文件系统和网络限制的配置。",
    },
    {
      key: "沙箱预设",
      href:"codex-sdk.zh-CN.md#sandbox-presets",
      appliesTo: "软件开发工具包",
      description:
        "SDK 常见沙箱策略的简写，例如只读、工作区写入或完全访问。",
    },
    {
      key: "时间表",
      href:"automations.zh-CN.md",
      appliesTo: "桌面应用程序",
      description: "计划任务的计时规则。",
    },
    {
      key: "秘密",
      href:"environments/cloud-environment.zh-CN.md#environment-variables-and-secrets",
      appliesTo: "云",
      description:
        "加密值可用于设置脚本，但在智能体阶段之前被删除。",
    },
    {
      key: "设置脚本",
      href:"environments/local-environment.zh-CN.md#setup-scripts",
      appliesTo: "桌面应用程序工作树",
      description:
        "在智能体开始安装依赖项或准备工具之前运行脚本。",
    },
    {
      key: "技能",
      href:"build-skills.zh-CN.md",
      appliesTo: "桌面应用程序、Web (ChatGPT Work)、CLI、IDE 扩展",
      description:
        "可重复使用的工作流程包，包含说明和可选脚本或参考。",
    },
    {
      key: "技能调用",
      href:"build-skills.zh-CN.md#how-codex-uses-skills",
      appliesTo: "桌面应用程序、Web (ChatGPT Work)、CLI、IDE 扩展",
      description: "显式或隐式激活技能。",
    },
    {
      key: "斜线命令",
      href:"developer-commands.zh-CN.md",
      appliesTo: "命令行界面",
      description:
        "输入带有前导斜杠的命令来控制或检查 Codex CLI 会话。",
    },
    {
      key: "独立计划任务",
      href:"automations.zh-CN.md",
      appliesTo: "桌面应用程序、网络",
      description:
        "每次运行的计划任务都会启动一个新的聊天并在分类中报告结果。",
    },
    {
      key: "STDIO MCP 服务器",
      href:"extend/mcp.zh-CN.md#stdio-servers",
      appliesTo: "CLI、IDE 扩展",
      description:
        "MCP 服务器通过配置的命令和参数作为本地进程启动。",
    },
    {
      key: "可流式 HTTP MCP 服务器",
      href:"extend/mcp.zh-CN.md#streamable-http-servers",
      appliesTo: "CLI、IDE 扩展",
      description:
        "通过 HTTP 访问 MCP 服务器，可选择使用不记名令牌或 OAuth 身份验证。",
    },
    {
      key: "子智能体",
      href:"agent-configuration/subagents.zh-CN.md",
      appliesTo: "桌面应用程序、CLI",
      description: "产生专门的子智能体来完成部分任务。",
    },
    {
      key: "子智能体工作流程",
      href:"agent-configuration/subagents.zh-CN.md#core-terms",
      appliesTo: "桌面应用程序、CLI",
      description:
        "Codex 并行运行委托智能体并合并其结果的工作流程。",
    },
    {
      key: "独立任务",
      href:"projects.zh-CN.md",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云",
      description: "未分组在项目中的 Codex 任务。",
    },
    {
      key: "任务",
      href:"projects.zh-CN.md",
      appliesTo: "桌面应用程序、Web、移动、CLI、IDE 扩展、云",
      description:
        "ChatGPT 或 Codex 致力于实现明确的结果，例如修复错误、创建文档或研究主题。",
    },
    {
      key: "线程",
      href:"app-server.zh-CN.md#threads",
      appliesTo: "应用服务器、SDK",
      description:
        "Codex 应用程序服务器 API 中的技术对象，包含轮流和存储的对话历史记录。",
    },
    {
      key: "聊天中的计划任务",
      href:"automations.zh-CN.md#schedule-a-task-inside-a-chat",
      appliesTo: "桌面应用程序、网络",
      description:
        "使用现有聊天上下文并将每次运行的结果返回到该聊天的计划任务。",
    },
    {
      key: "螺纹叉",
      href:"app-server.zh-CN.md#start-or-resume-a-thread",
      appliesTo: "应用服务器、SDK",
      description:
        "新线程从现有线程的存储历史中分支出来。",
    },
    {
      key: "转",
      href:"app-server.zh-CN.md#core-primitives",
      appliesTo: "桌面应用程序、CLI、IDE 扩展、云、SDK",
      description:
        "聊天中的一次交流，通常是用户提示加上客服人员的响应和操作。",
    },
    {
      key: "通用形象",
      href:"environments/cloud-environment.zh-CN.md#default-universal-image",
      appliesTo: "云",
      description:
        "默认 Codex 云容器镜像，预装常用工具。",
    },
    {
      key: "网页搜索缓存",
      href:"config-file/config-basic.zh-CN.md#web-search-mode",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description:
        "预索引搜索结果 Codex 无需实时浏览即可使用。",
    },
    {
      key: "ChatGPT Work",
      href:"get-started-with-work.zh-CN.md",
      appliesTo: "桌面应用程序、网络",
      description:
        "ChatGPT 中的智能体用于研究、分析和创建文档、演示文稿、电子表格和其他已完成的工作。",
    },
    {
      key: "工作树",
      href:"environments/git-worktrees.zh-CN.md",
      appliesTo: "桌面应用程序",
      description:
        "Codex 在单独的 Git 工作树中隔离更改的模式。",
    },
    {
      key: "可写根",
      href:"agent-approvals-security.zh-CN.md#protected-paths-in-writable-roots",
      appliesTo: "桌面应用程序、CLI、IDE 扩展",
      description: "目录Codex 允许修改。",
    },
  ]}
/>