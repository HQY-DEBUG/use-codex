> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/cli-customization.md)。

<a id="cli-customization"></a>

# 命令行界面定制

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex CLI 提供了特定于终端的选项，用于说明交互式会话的外观以及输入命令和提示的方式。

<a id="syntax-highlighting-and-themes"></a>

## 语法突出显示和主题

终端 UI (TUI) 语法突出显示受隔离的 Markdown 代码块和文件差异。运行 `/theme` 打开主题选择器，预览主题，并将您的选择保存到 `$CODEX_HOME/config.toml` 中的 `tui.theme`。

要添加自定义主题，请将 `.tmTheme` 文件放置在 `$CODEX_HOME/themes` 中，然后从主题选择器中选择它。

<a id="shell-completions"></a>

## 壳牌完井

为 Bash、Z shell、Fish 或 PowerShell 生成完成脚本：

```bash
codex completion zsh
```

从 shell 配置加载脚本。对于 Z shell，添加：

```bash
eval "$(codex completion zsh)"
```

如果 Z shell 报告 `command not found: compdef`，请在加载 Codex 补全之前初始化其补全系统：

```bash
autoload -Uz compinit && compinit
eval "$(codex completion zsh)"
```

重新启动 shell，输入 `codex`，然后按<kbd>Tab</kbd>以验证完成情况。

<a id="prompt-editor"></a>

## 提示编辑器

对于更长的提示，请按<kbd>Ctrl</kbd>+<kbd>G</kbd>在编辑器中打开由 `VISUAL` 配置的编辑器，或在未设置 `VISUAL` 时打开由 `EDITOR` 配置的编辑器。保存并关闭编辑器，以便在发送文本之前将文本返回给输入框。

有关交互式键盘控件以及完整的命令和选项列表，请参阅 [命令](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-interactive-shortcuts)。