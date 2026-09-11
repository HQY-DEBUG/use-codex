> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/agent-configuration/speed.md)。

<a id="speed"></a>

# 运行速度

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

**ChatGPT Work 和 Codex 共享使用。** 两者使用相同的定价、额度和使用限制。详细信息请参见 [Codex 定价](../pricing.zh-CN.md)。

<a id="fast-mode"></a>

## 快速模式

Codex 能够提高模型的速度以增加信用消耗。

对于 GPT-5.6、GPT-5.5 和 GPT-5.4，快速模式可将模型速度提高 1.5 倍。 GPT-5.6和GPT-5.5以标准费率的2.5倍消耗额度； GPT-5.4 以标准费率的 2 倍消耗额度。

GPT-6 Astra 快速模式消耗的额度是可用标准速率的 2.5 倍。有关模型可用性，请参阅 [模型](../models.zh-CN.md)；有关Token汇率，请参阅 [定价](../pricing.zh-CN.md#token-rates)。

在 CLI 中使用 `/fast on`、`/fast off` 或 `/fast status` 更改或检查当前设置。您还可以使用 `service_tier = "fast"` 和 `config.toml` 中的 `[features].fast_mode = true` 保留默认值。当您使用 ChatGPT 登录时，快速模式在 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中可用。快速模式是 ChatGPT 的信用功能。对于 API 密钥，Codex 使用 API 令牌定价，并且 ChatGPT 信用乘数不适用。 API优先级处理有自己的计费费率；对于 GPT-5.6，其成本是标准 API 令牌费率的 2 倍。

<VideoPlayer
  src="/videos/codex/fast-mode-demo.mp4"
  class="[&_video]:mx-auto [&_video]:max-h-[400px] [&_video]:max-w-full [&_video]:w-auto"
/>

<a id="codex-spark"></a>

## Codex-火花

GPT-5.3-Codex-Spark 是一个独立的快速、功能较弱的 Codex 模型，针对近乎即时、实时的编码迭代进行了优化。与快速模式以更高的信用率加速支持的模型不同，Codex-Spark 是它自己的模型选择，并且有自己的使用限制。

在研究预览期间，Codex-Spark 仅适用于 ChatGPT Pro 订阅者。