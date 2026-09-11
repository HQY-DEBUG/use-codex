> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/integrated-terminal.md)。

<a id="integrated-terminal"></a>

# 集成终端

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT 桌面应用程序中的每个聊天都包含一个范围仅限于其当前项目或工作树的终端。从应用程序右上角的终端图标打开它，或按<kbd>Ctrl</kbd>+<kbd>`</kbd>.


  

> 插图：集成终端抽屉在 ChatGPT 聊天下方打开




<a id="run-and-validate-your-project"></a>

## 运行并验证您的项目

使用终端验证更改、运行脚本并执行 Git 操作，而无需切换应用程序。 ChatGPT 可以读取当前的终端输出，因此它可以在与您一起工作时检查正在运行的开发服务器或引用失败的构建。

常用命令包括：

- `git status`
- `git pull --rebase`
- `pnpm test` 或 `npm test`
- `pnpm run lint` 或其他特定于项目的检查

<a id="create-reusable-actions"></a>

## 创建可重复使用的动作

如果您定期运行命令，请在 [当地环境](environments/local-environment.zh-CN.md#actions) 中定义操作。操作在 ChatGPT 桌面应用程序中显示为快捷方式，并在集成终端中运行。

<kbd>Cmd</kbd>+<kbd>K</kbd>打开应用程序命令面板；它不会清除终端。要清除终端，请按<kbd>Ctrl</kbd>+<kbd>L</kbd>.