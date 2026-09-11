> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/agent-configuration/agents-md.md)。

<a id="custom-instructions-with-agentsmd"></a>

# AGENTS.md 自定义指令

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 在执行任何工作之前读取 `AGENTS.md` 文件。通过将全局指导与特定于项目的覆盖进行分层，无论您打开哪个仓库，您都可以以一致的期望开始每项任务。

<a id="how-codex-discovers-guidance"></a>

## Codex 如何发现指导

Codex 在启动时构建指令链（每次运行一次；在 TUI 中，这通常意味着每次启动会话一次）。发现遵循以下优先顺序：

1. **全球范围：** 在您的 Codex 主目录中（默认为 `~/.codex`，除非您设置 `CODEX_HOME`），Codex 读取 `AGENTS.override.md`（如果存在）。否则，Codex 读取为 `AGENTS.md`。 Codex 仅使用该级别的第一个非空文件。
2. **项目范围：** 从项目根目录（通常是 Git 根目录）开始，Codex 向下走到当前工作目录。如果 Codex 找不到项目根目录，则仅检查当前目录。在路径上的每个目录中，它会检查 `AGENTS.override.md`，然后检查 `AGENTS.md`，然后检查 `project_doc_fallback_filenames` 中的任何后备名称。 Codex 每个目录最多包含一个文件。
3. **合并顺序：** Codex 从根向下连接文件，用空行将它们连接起来。更接近当前目录的文件会覆盖之前的指导，因为它们稍后出现在组合提示中。

一旦组合大小达到 `project_doc_max_bytes` 定义的限制（默认为 32 KiB），Codex 就会跳过空文件并停止添加文件。有关这些旋钮的详细信息，请参见 [项目指令发现](../config-file/config-advanced.zh-CN.md#project-instructions-discovery)。当达到上限时，提高限制或跨嵌套目录分割指令。

<a id="create-global-guidance"></a>

## 创建全球指导

在 Codex 主目录中创建持久默认值，以便每个仓库都继承您的工作协议。

1. 确保目录存在：

```bash
   mkdir -p ~/.codex
```

2. 创建具有可重用首选项的 `~/.codex/AGENTS.md`：

```md
   # ~/.codex/AGENTS.md

   ## 工作协议

   - 修改 JavaScript 文件后始终运行 `npm test`。
   - 安装依赖项时首选 `pnpm`。
   - 在添加新的生产依赖项之前请求确认。
```

3. 在任何地方运行 Codex 以确认它加载文件：

```bash
   codex --ask-for-approval never "Summarize the current instructions."
```

预期：Codex 在提出工作之前引用 `~/.codex/AGENTS.md` 中的项目。

当您需要临时全局覆盖而不删除基本文件时，请使用 `~/.codex/AGENTS.override.md`。删除覆盖以恢复共享指导。

<a id="layer-project-instructions"></a>

## 图层项目说明

仓库级文件使 Codex 了解项目规范，同时仍然继承您的全局默认值。

1. 在您的仓库根目录中，添加涵盖基本设置的 `AGENTS.md`：

```md
   # AGENTS.md

   ## 仓库期望

   - 在打开拉取请求之前运行 `npm run lint`。
   - 当您改变行为时，在 `docs/` 中记录公用事业。
```

2. 当特定团队需要不同的规则时，在嵌套目录中添加覆盖。例如，在`services/payments/`内部创建`AGENTS.override.md`：

```md
   # services/payments/AGENTS.override.md

   ## 支付服务规则

   - 使用 `make test-payments` 代替 `npm test`。
   - 切勿在未通知安全通道的情况下轮换 API 密钥。
```

3. 从付款目录启动 Codex：

```bash
   codex --cd services/payments --ask-for-approval never "List the instruction sources you loaded."
```

预期：Codex 首先报告全局文件，其次报告仓库根 `AGENTS.md`，最后报告支付覆盖。

Codex 一旦到达当前目录就会停止搜索，因此将覆盖尽可能靠近专门的工作。

以下是添加全局文件和特定于付款的覆盖后的示例仓库：

<FileTree
  class="mt-4"
  tree={[
    {
      name: "AGENTS.md",
      comment: "仓库期望",
      highlight: true,
    },
    {
      name: "services/",
      open: true,
      children: [
        {
          name: "payments/",
          open: true,
          children: [
            {
              name: "AGENTS.md",
              comment: "由于存在覆盖而被忽略",
            },
            {
              name: "AGENTS.override.md",
              comment: "支付服务规则",
              highlight: true,
            },
            { name: "README.md" },
          ],
        },
        {
          name: "search/",
          children: [{ name: "AGENTS.md" }, { name: "…", placeholder: true }],
        },
      ],
    },
  ]}
/>

<a id="add-code-review-rules"></a>

## 添加代码审查规则

对于 [Codex GitHub 中的代码审查](../third-party/github.zh-CN.md#customize-what-codex-reviews)，将 `## Code Review Rules` 部分添加到最接近规则管辖的代码的 `AGENTS.md`。将仓库范围的检查放在根目录中，将特定于服务的检查放在嵌套文件中。

```md
## 代码审查规则

### 实验队列

- 不要过滤暴露后行为的治疗比较，包括转化或保留。
  安全路径：通过分配或接触来建立队列；将转化报告为结果。
```

保持规则简洁，解释标记行为以及任何安全路径或异常，并保留 CI 的格式和 lint 检查。有关设置和规则编写指南，请参阅 [自定义 Codex 评论内容](../third-party/github.zh-CN.md#customize-what-codex-reviews)。

<a id="customize-fallback-filenames"></a>

## 自定义后备文件名

如果您的仓库已使用不同的文件名（例如 `TEAM_GUIDE.md`），请将其添加到后备列表中，以便 Codex 将其视为指令文件。

1. 编辑您的 Codex 配置：

```toml
   # 〜/.codex/config.toml
   project_doc_fallback_filenames = ["TEAM_GUIDE.md", ".agents.md"]
   project_doc_max_bytes = 65536
```

2. 重新启动 Codex 或运行新命令以便加载更新的配置。

现在Codex按以下顺序检查每个目录：`AGENTS.override.md`、`AGENTS.md`、`TEAM_GUIDE.md`、`.agents.md`。指令发现时将忽略不在此列表中的文件名。较大的字节限制允许在截断之前进行更多组合指导。

后备列表到位后，Codex 将备用文件视为指令：

<FileTree
  class="mt-4"
  tree={[
    {
      name: "TEAM_GUIDE.md",
      comment: "通过后备列表检测到",
      highlight: true,
    },
    {
      name: ".agents.md",
      comment: "根目录中的后备文件",
    },
    {
      name: "support/",
      open: true,
      children: [
        {
          name: "AGENTS.override.md",
          comment: "覆盖后备指导",
          highlight: true,
        },
        {
          name: "playbooks/",
          children: [{ name: "…", placeholder: true }],
        },
      ],
    },
  ]}
/>

当您需要不同的配置文件（例如特定于项目的自动化用户）时，请设置 `CODEX_HOME` 环境变量：

```bash
CODEX_HOME=$(pwd)/.codex codex exec "List active instruction sources"
```

预期：输出列出与自定义 `.codex` 目录相关的文件。

<a id="verify-your-setup"></a>

## 验证您的设置

- 从仓库根运行 `codex --ask-for-approval never "Summarize the current instructions."`。 Codex 应按优先顺序回显全局文件和项目文件中的指导。
- 使用 `codex --cd subdir --ask-for-approval never "Show which instruction files are active."` 确认嵌套覆盖替换更广泛的规则。
- 要审核加载的指令文件 Codex，请选择使用 `codex -c log_dir=./.codex-log` 进入纯文本 TUI 日志并检查 `./.codex-log/codex-tui.log`，或者如果启用了会话日志记录，则检查最新的 `session-*.jsonl` 文件。
- 如果指令看起来过时，请在目标目录中重新启动 Codex。 Codex 在每次运行时（以及在每个 TUI 会话开始时）都会重建指令链，因此无需手动清除缓存。

<a id="troubleshoot-discovery-issues"></a>

## 解决发现问题

- **没有加载任何内容：** 验证您位于预期仓库中，并且 `codex status` 报告您期望的工作区根目录。确保说明文件包含内容； Codex 忽略空文件。
- **出现错误的指导：** 在目录树的较高位置或 Codex 主目录下查找 `AGENTS.override.md`。重命名或删除覆盖以回退到常规文件。
- **Codex 忽略后备名称：** 确认您在 `project_doc_fallback_filenames` 中列出的名称没有拼写错误，然后重新启动 Codex 以使更新的配置生效。
- **指令被截断：** 提升 `project_doc_max_bytes` 或跨嵌套目录拆分大文件，以保持关键指导的完整性。
- **个人资料混乱：** 在启动 Codex 之前运行 `echo $CODEX_HOME`。非默认值将 Codex 指向与您编辑的主目录不同的主目录。

<a id="next-steps"></a>

## 后续步骤

- 访问[AGENTS.md](https://agents.md)官方网站了解更多信息。
- 查看 [提示Codex](../prompting.zh-CN.md)，了解与持续指导完美搭配的对话模式。