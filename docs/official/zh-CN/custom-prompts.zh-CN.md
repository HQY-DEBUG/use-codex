> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/custom-prompts.md)。

<a id="custom-prompts"></a>

# 旧版自定义提示词

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

自定义提示已被弃用。使用 [技能](build-skills.zh-CN.md) 作为 Codex 可以显式或隐式调用的可重用指令。

自定义提示（已弃用）允许您将 Markdown 文件转换为可重用的提示，您可以在 Codex CLI 和 Codex IDE 扩展中作为斜杠命令调用。

自定义提示需要显式调用并位于本地 Codex 主目录（例如 `~/.codex`）中，因此它们不会通过您的仓库共享。如果您想共享提示（或希望 Codex 隐式调用它），请使用 [使用技巧](build-skills.zh-CN.md)。

1. 创建提示目录：

```bash
   mkdir -p ~/.codex/prompts
```

2. 使用可重用指导创建 `~/.codex/prompts/draftpr.md`：

```markdown
   ---
   描述：准备分支、提交并打开草稿 PR
   argument-hint: [FILES=<paths>] [PR_TITLE="<title>"]
   ---

   为此工作创建一个名为 `dev/<feature_name>` 的分支。
   如果指定了文件，则首先暂存它们：$FILES。
   提交分阶段的变更并发出明确的信息。
   在同一分支上打开草稿 PR。供货时使用 $PR_TITLE；否则你自己写一个简洁的总结。
```

3. 重新启动 Codex，以便加载新的提示（重新启动 CLI 会话，并重新加载 IDE 扩展（如果您正在使用它））。

预期：在斜线命令菜单中键入 `/prompts:draftpr` 会显示您的自定义命令以及前面的描述，并提示文件和 PR 标题是可选的。

<a id="add-metadata-and-arguments"></a>

## 添加元数据和参数

Codex 读取提示元数据并在下次会话启动时解析占位符。

- **说明：** 显示在弹出窗口中的命令名称下方。在 YAML Front Matter 中将其设置为 `description:`。
- **参数提示：** 使用 `argument-hint: KEY= 记录预期参数<value>`.
- **位置占位符：** `$1` 到 `$9` 从命令后提供的以空格分隔的参数扩展。 `$ARGUMENTS` 包括所有这些。
- **命名占位符：** 使用 `$FILE` 或 `$TICKET_ID` 等大写名称，并提供 `KEY=value` 等值。用空格引用值（例如，`FOCUS="loading state"`）。
- **美元符号字面意思：** 写入 `$$` 以在扩展提示中发出单个 `$`。

编辑提示文件后，重新启动 Codex 或打开新聊天以便加载更新。 Codex 忽略提示目录中的非 Markdown 文件。

<a id="invoke-and-manage-custom-commands"></a>

## 调用和管理自定义命令

1. 在 Codex（CLI 或 IDE 扩展）中，键入 `/` 打开斜杠命令菜单。
2. 输入 `prompts:` 或提示名称，例如 `/prompts:draftpr`。
3. 提供所需的参数：

```text
   /prompts:draftpr FILES="src/pages/index.astro src/lib/api.ts" PR_TITLE="Add hero animation"
```

4. 按 Enter 键发送扩展指令（当不需要时跳过任一参数）。

预期：Codex 扩展 `draftpr.md` 的内容，用您提供的参数替换占位符，然后将结果作为消息发送。

通过编辑或删除 `~/.codex/prompts/` 下的文件来管理提示。 Codex 仅扫描该文件夹中的顶级 Markdown 文件，因此将每个自定义提示直接放在 `~/.codex/prompts/` 下，而不是放在子目录中。