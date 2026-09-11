> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/web-search.md)。

<a id="web-search"></a>

# 网页搜索

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT 包含第一方网络搜索工具。将所有网络结果视为不受信任的输入。

<ContentModeSwitch group="codex-surface" id="app">

在 ChatGPT 桌面应用程序中，在聊天中询问当前信息。 ChatGPT 在脚本中记录其他工具调用的搜索活动。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

在 ChatGPT 网站中，询问当前信息或来源。当 ChatGPT 使用网络搜索时，搜索结果和引文会显示在聊天中。工作区设置可以限制搜索是否可用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在 CLI 中，传递 `--search` 以获取一次运行的实时结果：

```bash
codex --search "Summarize the latest release notes for this dependency"
```

搜索在交互式脚本和 `codex exec --json` 输出中显示为 `web_search` 项目。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

在 IDE 扩展中，要求 Codex 在编辑器中工作时进行搜索。分机使用所连接的 Codex 主机的搜索模式。搜索活动显示在聊天记录中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="configure-local-web-search"></a>

## 配置本地网络搜索

对于本地 Codex 聊天，Codex 默认启用缓存搜索。缓存模式使用 OpenAI 维护的索引，而不是实时获取任意页面，这会降低（但不会消除）提示注入风险。

Web 搜索是一种托管工具，独立于沙箱本地命令网络。它不使用权限配置文件的网络代理或域白名单，并且在禁用命令网络访问时它可以保持可用。根据需要使用 `web_search`、`tools.web_search.allowed_domains` 和托管 `allowed_web_search_modes` 配置搜索。搜索域过滤器不限制本地命令流量、应用程序、连接器或 MCP 服务器。

当您的任务取决于最新信息时，请使用实时搜索。将 `web_search = "live"` 设置为 `config.toml`。设置 `web_search = "disabled"` 以关闭工具。 `"indexed"` 模式仅当搜索索引控制请求时才允许外部 Web 访问。当 Codex 以完全访问权限运行时，网络搜索默认为实时结果。有关配置文件位置和优先级，请参阅 [配置基础知识](config-file/config-basic.zh-CN.md)。

<a id="search-with-a-custom-model-provider"></a>

### 搜索自定义模型提供商

当自定义模型提供程序支持兼容的搜索端点时，可以选择独立的 Web 搜索：

```toml
model_provider = "custom"
web_search = "live"

[model_providers.custom]
name = "Custom Responses provider"
base_url = "https://example.com/v1"
env_key = "CUSTOM_RESPONSES_API_KEY"
supports_standalone_web_search = true
```

自定义提供程序默认为 `supports_standalone_web_search = false`。独立网络搜索仍在开发中，默认情况下处于关闭状态。设置此提供程序功能不会启用该功能：提供程序、所选模型和运行时还必须支持独立搜索。工作区和托管搜索限制仍然适用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

有关适用于 Codex 云环境的网络边界，请参阅 [互联网接入](cloud/internet-access.zh-CN.md)。

</ContentModeSwitch>