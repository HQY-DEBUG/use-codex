> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/pricing.md)。

<a id="pricing"></a>

# 价格与额度

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

**ChatGPT Work 和 Codex 共享使用。** ChatGPT Work 在 ChatGPT 内的使用使用与 Codex 相同的定价、额度和使用限制。

<h2 class="sr-only">定价选项</h2>

<ContentSwitcher
  id="codex-pricing-plans"
  initialValue="individual"
  options={[
    {
      label: "个人",
      value: "individual",
    },
    {
      label: "商业/企业",
      value: "business-enterprise",
    },
  ]}
>
  

    

      <PricingCard
        name="Free"
        subtitle="探索 Codex 执行快速编码任务的功能。"
        price="$0"
        interval="/month"
        ctaLabel="Get Free"
        ctaHref="https://chatgpt.com/plans/free/"
      />
      <PricingCard
        name="Go"
        subtitle="使用 Codex 执行轻量级编码任务。"
        price="$8"
        interval="/month"
        ctaLabel="Get Go"
        ctaHref="https://chatgpt.com/plans/go"
      />
      <PricingCard
        name="Plus"
        subtitle="每周举办一些重点编码课程。"
        price="$20"
        interval="/month"
        ctaLabel="Get Plus"
        ctaHref="https://chatgpt.com/explore/plus?utm_internal_source=openai_developers_codex"
      >
        - Codex 在 Web、CLI、IDE 扩展和 iOS 上
        - 基于云的集成，例如自动代码审查和 Slack 集成
        - GPT-5.6 模型系列，包括 Sol、Terra 和 Luna
        - GPT-5.6 Luna 对轻量级或大容量工作负载提供更高的使用限制
        - 通过 [ChatGPT 额度](#credits-overview) 灵活扩展使用
        - Plus 计划中的其他 [ChatGPT特点](https://chatgpt.com/pricing)
      </PricingCard>
      <PricingCard
        name="Pro"
        subtitle="选择比 Plus 高 5 倍或 20 倍的速率限制。"
        priceEyebrow="From"
        price="$100"
        interval="/month"
        ctaLabel="Get Pro"
        ctaHref="https://chatgpt.com/explore/pro?utm_internal_source=openai_developers_codex"
        highlight="Everything in Plus and:"
        footnoteLabel="*了解有关两个级别的限制的更多信息。"
        footnoteHref="https://help.openai.com/en/articles/9793128-about-chatgpt-pro-plans"
      >
        - 访问 GPT-5.3-Codex-Spark（研究预览），这是一种用于日常编码任务的快速 Codex 模型
        - Codex 的使用量比 Plus* 多 5 倍或 20 倍
        - 每月 200 美元级别的无限 ChatGPT 语音；任务仍然从您的 Codex 使用预算中提取
        - Pro 计划中的其他 [ChatGPT特点](https://chatgpt.com/pricing)
      </PricingCard>
      <PricingCard
        name="API Key"
        subtitle="非常适合 CI 等共享环境中的自动化。"
        price=""
        interval=""
        ctaLabel="Learn more"
        ctaHref="/codex/auth"
        highlight=""
      >
        - CLI、SDK 或 IDE 扩展中的 Codex
        - 没有基于云的功能（GitHub 代码审查、Slack 等）
        - 模型可用性遵循您的密钥可用的 API 模型
        - 根据[API定价](https://developers.openai.com/api/docs/pricing)支付Codex使用费
      </PricingCard>
    


  


  

    

      <PricingCard
        name="Business"
        subtitle="将 Codex 引入您的初创企业或成长型企业。"
        price="$20"
        interval="/ user / month*"
        ctaLabel="Get Business"
        ctaHref="https://chatgpt.com/team-sign-up"
        footnoteLabel="*2+用户，按年计费。按月计费时，每位用户每月 25 美元。"
      >
        - 跨桌面和移动应用程序访问 ChatGPT 和 Codex
        - 更大的虚拟机可以更快地运行云聊天
        - 通过 [ChatGPT 额度](#credits-overview) 灵活扩展使用
        - 安全、专用的工作区，具有基本的管理控制、SAML SSO 和 MFA
        - 默认情况下不对您的业务数据进行培训。 [了解更多](https://openai.com/business-data/)
        - 作为商业计划一部分的其他 [ChatGPT特点](https://chatgpt.com/pricing)
      </PricingCard>
      <PricingCard
        name="Enterprise & Edu"
        subtitle="通过企业级功能为您的整个组织解锁 Codex。"
        interval=""
        ctaLabel="Contact sales"
        ctaHref="https://chatgpt.com/contact-sales?utm_internal_source=openai_developers_codex"
        highlight="Everything in Business and:"
      >
        - 优先请求处理
        - 企业级安全和控制，包括 SCIM、EKM、用户分析、域验证和基于角色的访问控制 ([RBAC](https://help.openai.com/en/articles/11750701-rbac))
        - 通过 [合规API](https://chatgpt.com/public/admin/api-reference#tag/Codex%20Tasks) 进行审核日志和使用情况监控
        - 数据保留和数据驻留控制
        - 作为企业计划一部分的其他 [ChatGPT特点](https://chatgpt.com/pricing)
      </PricingCard>
    


    

      <PricingCard
        class="codex-pricing-card--span-two"
        name="API Key"
        subtitle="非常适合 CI 等共享环境中的自动化。"
        price=""
        interval=""
        ctaLabel="Learn more"
        ctaHref="/codex/auth"
        highlight=""
      >
        - CLI、SDK 或 IDE 扩展中的 Codex
        - 没有基于云的功能（GitHub 代码审查、Slack 等）
        - 模型可用性遵循您的密钥可用的 API 模型
        - 根据[API定价](https://developers.openai.com/api/docs/pricing)支付Codex使用费
      </PricingCard>
    


  

</ContentSwitcher>

<a id="invite-friends-and-coworkers"></a>

## 邀请朋友和同事

符合条件的用户可以从应用程序左下角的个人资料菜单发送 Codex 邀请。在符合条件的个人计划中选择 **邀请朋友**，或在符合条件的业务工作区中选择 **邀请同事**，输入收件人的电子邮件地址，然后发送邀请。

邀请对话框显示当前奖励、接收者要求、邀请限制以及您的计划或促销活动的奖励何时到期。个人和企业推荐计划有单独的奖励和资格规则。目前不适用于 ChatGPT Enterprise 的推荐。

2026 年 6 月 11 日至 6 月 24 日期间，符合条件的 Plus 和 Pro 用户最多可以邀请三位朋友。当符合条件的收件人发送第一条 Codex 消息时，两人都会收到银行速率限制重置。银行利率限制重置在授予后的 30 天内可用。业务推荐使用单独的共享工作区信用奖励；在发送邀请之前查看 [当前条款](https://help.openai.com/en/articles/20001271)。

<a id="frequently-asked-questions"></a>

## 常见问题

<a id="how-much-does-sites-cost"></a>

### 网站费用是多少？

在公开测试期间，[站点](sites.zh-CN.md) 包含在符合条件的 ChatGPT 计划中。可用性取决于您的计划、区域和工作区设置。

<a id="what-are-the-usage-limits-for-my-plan"></a>

### 我的计划有哪些使用限制？

您可以发送的消息数量取决于所使用的模型、任务的大小和复杂性，以及您是在本地还是在云中运行它们。小脚本或例行函数可能只消耗您的一小部分津贴，而较大的项目、长时间运行的任务或需要智能体保存更多上下文的扩展会话将每条消息使用更多的费用。

看起来相似的任务可能会消耗不同数量的津贴。模型选择、上下文、推理、工具使用、检索和缓存都会影响使用，因此提示长度本身并不是一个可靠的估计。

选择最适合您工作的 GPT-5.6 模型：

- **索尔** 专为最艰巨的工作而打造——复杂推理、模糊问题、高级编码和高风险决策。
- **泰拉** 是执行生产任务、报告、文档分析、编码和需要正确判断的工作的日常主力。
- **露娜** 针对快速、大批量的工作进行了优化，例如路由、分类、提取、支持、后台自动化和集中编码任务。




下面的估计显示每五个小时的本地消息。 ChatGPT 计划上的云聊天使用 GPT-5.6 Sol，并且可能比本地消息使用更多的津贴。这些估计不是固定的消息限制；检查您的 [使用仪表板](#where-can-i-see-my-current-usage-limits) 的电流限制和重置时间。




<TableWrapper class="w-full min-w-[46rem]">
  <thead class="whitespace-nowrap">
    <tr>
      <th scope="col">模型</th>
      <th scope="col" style="text-align:center">
加号
      </th>
      <th scope="col" style="text-align:center">
专业版 5x
      </th>
      <th scope="col" style="text-align:center">
专业版 20 倍
      </th>
      <th scope="col" style="text-align:center">
标准商务
      </th>
      <th scope="col" style="text-align:center">
API密钥
      </th>
    </tr>
  </thead>
  <tbody class="whitespace-nowrap">
    <tr>
      <td>GPT-6 阿斯特拉</td>
      <td style="text-align:center">5-45</td>
      <td style="text-align:center">25-225</td>
      <td style="text-align:center">100-900</td>
      <td style="text-align:center">5-45</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.6溶胶</td>
      <td style="text-align:center">10-100</td>
      <td style="text-align:center">50-500</td>
      <td style="text-align:center">200-2,000</td>
      <td style="text-align:center">10-100</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.6 泰拉</td>
      <td style="text-align:center">25-200</td>
      <td style="text-align:center">125-1,000</td>
      <td style="text-align:center">500-4,000</td>
      <td style="text-align:center">25-200</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.6 露娜</td>
      <td style="text-align:center">250-2,000</td>
      <td style="text-align:center">1,250-10,000</td>
      <td style="text-align:center">5,000-40,000</td>
      <td style="text-align:center">250-2,000</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.5</td>
      <td style="text-align:center">15-80</td>
      <td style="text-align:center">75-400</td>
      <td style="text-align:center">300-1,600</td>
      <td style="text-align:center">15-80</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.4</td>
      <td style="text-align:center">20-100</td>
      <td style="text-align:center">100-500</td>
      <td style="text-align:center">400-2,000</td>
      <td style="text-align:center">20-100</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
    <tr>
      <td>GPT-5.4迷你</td>
      <td style="text-align:center">60-350</td>
      <td style="text-align:center">300-1,750</td>
      <td style="text-align:center">1,200-7,000</td>
      <td style="text-align:center">60-350</td>
      <td style="text-align:center">
[基于使用情况](https://platform.openai.com/docs/pricing)
      </td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="6" style="text-align:center">
本地消息和云聊天共享您的计划的使用限额。每周限制也可能适用。
      </td>
    </tr>
    <tr>
      <td colspan="6" style="text-align:center">
对于具有灵活定价的企业/教育用户，没有固定的费率限制 - 使用范围为 [额度](#credits-overview)。
      </td>
    </tr>
    <tr>
      <td colspan="6" style="text-align:center">
对于大多数功能，没有灵活定价的 Enterprise 和 Edu 计划与 Plus 具有相同的每席位使用限制。
      </td>
    </tr>
  </tfoot>
</TableWrapper>

Business（100 美元）使用 Pro 5x 估算值。

一旦这些功能的定价生效，使用限制将与其他代理功能共享。目前包括 Plus 和 Pro 上的 [ChatGPT Excel 版](https://help.openai.com/articles/20001063)。

速度配置会增加所有适用模型的额度消耗，因此它们也会更快地使用包含的限制。对于支持的模型，快速模式会以更高的速率消耗额度。有关支持的模型和费率，请参阅 [速度](agent-configuration/speed.zh-CN.md)。图像生成还使用包含的限制，平均速度快约 3-5 倍，具体取决于图像质量和大小。 GPT-5.3-Codex-Spark 仅适用于 ChatGPT Pro 用户的研究预览版，发布时在 API 中不可用。由于它在专门的低延迟硬件上运行，因此使用情况受到单独的使用限制的控制，该限制可以根据需求进行调整。

<a id="chatgpt-voice-in-desktop"></a>

### ChatGPT 桌面语音

ChatGPT 桌面版语音使用单独的、与计划相关的津贴，以滚动的五小时窗口来衡量。通过语音启动的任务使用您现有的 Codex 使用预算。当您达到任一限制时，ChatGPT 会通知您。

GPT-Live 管理实时对话。当您在现有 Codex 任务中使用语音时，任务的选定模型将处理工作。有关可用性和设置，请参阅 [ChatGPT 语音](features/voice.zh-CN.md#start-talking)。

- **加：** 大约 15–30 分钟
- **Pro 5x（100 美元/月）：** 约 1–2.5 小时
- **Pro 20x（200 美元/月）：** 无限语音访问
- **业务：** 约45分钟
- **企业/教育（旧版）：** 约45分钟

无限的语音访问并不意味着 Codex 任务不受限制。通过 ChatGPT Voice 启动的任务将继续使用您现有的 Codex 使用预算。

对于采用基于额度或即用即付计费的商业、教育和企业工作区，桌面语音的费用约为每分钟 6 个额度。 ChatGPT 桌面中的语音目前无法通过 API 密钥使用。

<a id="what-happens-when-you-hit-usage-limits"></a>

### 当您达到使用限制时会发生什么？

我们希望您能够完成正在进行的工作。如果您在活动回合中达到使用限制，智能体将能够继续在该回合中工作，但须遵守合理使用限制。

达到使用限制的 ChatGPT Plus 和 Pro 用户可以购买额外的额度以继续工作，而无需升级现有计划。

具有 [灵活定价](https://help.openai.com/en/articles/11487671-flexible-pricing-for-the-enterprise-edu-and-business-plans) 的商业、教育和企业计划可以购买额外的工作区额度以继续工作。

如果您接近使用限制，您还可以切换到较小的模型，以使您的使用限制持续更长时间。

所有用户还可以使用 API 密钥运行额外的本地聊天，使用费为 [标准API费率](https://platform.openai.com/docs/pricing)。

<a id="image-generation-usage-limits"></a>

<a id="how-does-image-generation-count-toward-usage-limits"></a>

### 图像生成如何计入使用限制？

图像生成与本地消息和云聊天具有相同的一般使用限制。使用包含限制的图像生成平均比不生成图像的类似对话轮次快 3-5 倍，具体取决于图像质量和尺寸。达到包含的限制后，图像生成也会从 [额度](#credits-overview) 中提取。

免费套餐不提供图像生成功能。当您将 Codex 与 API 密钥一起使用时，API 定价适用于图像生成，而不是包含的 ChatGPT 使用限制。

<a id="where-can-i-see-my-current-usage-limits"></a>

### 在哪里可以查看我当前的使用限制？

您可以在 [使用仪表板](https://chatgpt.com/codex/settings/usage) 中找到您的当前限制。如果您想在活动的 Codex CLI 会话期间查看剩余限制，可以使用 `/status`。

每隔一两周检查一次仪表板，了解您的配速和剩余容量。如果使用率高于预期，请考虑较小的模型或更严格的任务范围是否仍会产生有用的结果。

<a id="credits-overview"></a>
<a id="what-are-tokens-and-credits"></a>

### 什么是Token和额度？

令牌是 ChatGPT 读取和写入的小信息单元。您的提示、文件、聊天历史记录、工具结果和 ChatGPT 的响应都使用令牌。

额度是用于支付基于额度的计划的合格使用费用的单位。达到包含的限制后，可用额度可让您继续工作。额度购买价格和适用的折扣取决于您的计划或协议。

<a id="token-rates"></a>

#### Token利率

下面的令牌率以每百万个输入令牌、缓存输入令牌和输出令牌的额度表示。 [了解有关Token的更多信息](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)。

快速模式对 Astra 的标准费率应用 2.5 倍的乘数。

一小部分企业客户应继续使用旧价目表，直到我们将您迁移到新的基于令牌的定价。欲了解更多信息，[联系 OpenAI 销售](https://chatgpt.com/contact-sales?utm_internal_source=openai_developers_codex)。



  <table>
    <thead>
      <tr>
        <th scope="col">每 100 万个Token的额度</th>
        <th scope="col" style="text-align:center">
输入令牌
        </th>
        <th scope="col" style="text-align:center">
缓存的输入令牌
        </th>
        <th scope="col" style="text-align:center">
输出Token
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>GPT-6 阿斯特拉</td>
        <td style="text-align:center">250 额度</td>
        <td style="text-align:center">25 额度</td>
        <td style="text-align:center">1,250 额度</td>
      </tr>
      <tr>
        <td>GPT-5.6溶胶</td>
        <td style="text-align:center">100 额度</td>
        <td style="text-align:center">10 额度</td>
        <td style="text-align:center">500 额度</td>
      </tr>
      <tr>
        <td>黎明蓝</td>
        <td style="text-align:center">100 额度</td>
        <td style="text-align:center">10 额度</td>
        <td style="text-align:center">500 额度</td>
      </tr>
      <tr>
        <td>黎明红</td>
        <td style="text-align:center">312.5额度</td>
        <td style="text-align:center">31.25 额度</td>
        <td style="text-align:center">1875 额度</td>
      </tr>
      <tr>
        <td>GPT-5.6 泰拉</td>
        <td style="text-align:center">50 额度</td>
        <td style="text-align:center">5 额度</td>
        <td style="text-align:center">300 额度</td>
      </tr>
      <tr>
        <td>GPT-5.6 露娜</td>
        <td style="text-align:center">5 额度</td>
        <td style="text-align:center">0.5额度</td>
        <td style="text-align:center">30 额度</td>
      </tr>
      <tr>
        <td>GPT-5.5</td>
        <td style="text-align:center">125 额度</td>
        <td style="text-align:center">12.50 额度</td>
        <td style="text-align:center">750 额度</td>
      </tr>
      <tr>
        <td>GPT-5.4</td>
        <td style="text-align:center">62.50 额度</td>
        <td style="text-align:center">6.250 额度</td>
        <td style="text-align:center">375 额度</td>
      </tr>
      <tr>
        <td>GPT-5.4迷你</td>
        <td style="text-align:center">18.75 额度</td>
        <td style="text-align:center">1.875 额度</td>
        <td style="text-align:center">113 额度</td>
      </tr>
      <tr>
        <td>GPT-5.3-Codex-火花</td>
        <td colspan="3" style="text-align:center">
研究预览
        </td>
      </tr>
      <tr>
        <td>GPT-Image-2（图像）</td>
        <td style="text-align:center">200 额度</td>
        <td style="text-align:center">50 额度</td>
        <td style="text-align:center">750 额度</td>
      </tr>
      <tr>
        <td>GPT-图像-2（文本）</td>
        <td style="text-align:center">125 额度</td>
        <td style="text-align:center">31.25 额度</td>
        <td style="text-align:center">250 额度</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="4" style="text-align:center">
GPT-5.6 的使用平均每条消息 5-30 个额度。
        </td>
      </tr>
      <tr>
        <td colspan="4" style="text-align:center">
对于支持的模型，快速模式会以更高的速率消耗额度。有关费率，请参阅 [速度](agent-configuration/speed.zh-CN.md)。
        </td>
      </tr>
      <tr>
        <td colspan="4" style="text-align:center">
Daybreak 访问需要 [网络可信访问](cyber-safety.zh-CN.md#trusted-access-for-cyber) 批准。 Daybreak Blue 使用 GPT-5.6 Sol 信用率。 Daybreak Red 需要单独的批准和配置。
        </td>
      </tr>
    </tfoot>
  </table>



_GPT-5.6 Sol 的促销价格至少在 2026 年 11 月 21 日之前可用。_

速度配置将增加所有适用模型的额度消耗。对于支持的模型，快速模式会以更高的速率消耗额度。有关支持的模型和费率，请参阅 [速度](agent-configuration/speed.zh-CN.md)。

[了解有关 ChatGPT Plus 和 Pro 中额度的更多信息。](https://help.openai.com/en/articles/12642688)

[了解有关 ChatGPT Business、Enterprise 和 Edu 额度的更多信息。](https://help.openai.com/en/articles/11487671-flexible-pricing-for-the-enterprise-edu-and-business-plans)

对于商业和企业/教育额度账单，请使用 [基于额度的价目表](https://help.openai.com/en/articles/11481834-chatgpt-rate-card-business-enterpriseedu-credit-based-pricing)。如果您的企业协议指定以美元为单位按使用量计费，请改用 [企业美元价目表](https://help.openai.com/en/articles/20001415-chatgpt-rate-card-enterprise-token-based-pricing) 和您的协议。工作区管理员还可以查看 [ChatGPT Work用途及费用](enterprise/chatgpt-work-usage-and-cost.zh-CN.md#understand-tokens-and-credits)。

<a id="what-counts-as-code-review-usage"></a>

### 什么算作代码审查使用情况？

仅当 Codex 通过 GitHub 运行审查时，代码审查用法才适用 - 例如，当您在拉取请求中标记 `@Codex` 进行审查或在仓库上启用自动审查时。在本地或 GitHub 之外运行的评论会计入您的一般使用限制。

<a id="what-can-i-do-to-make-my-usage-limits-last-longer"></a>

### 我可以做些什么来延长我的使用限制？

上述使用限额和额度为平均费率。您可以尝试以下提示来最大化您的限制：

- **控制提示的大小。** 向智能体提供的说明要准确，但删除不必要的上下文。
- **限制来源材料。** 仅提供相关文件，并在可能的情况下缩小来源或日期范围。
- **将输出与需求相匹配。** 定义受众、格式和长度，并将所需的工作与可选的改进分开。
- **减小 AGENTS.md 的尺寸。** 如果您处理较大的项目，您可以通过 [将它们嵌套在您的仓库中](agent-configuration/agents-md.zh-CN.md#layer-project-instructions) 控制通过 AGENTS.md 文件注入多少上下文。
- **限制您使用的 MCP 服务器的数量。** 每个 [模型上下文协议](extend/mcp.zh-CN.md) 服务器都会为您的消息添加更多上下文，并使用更多的限制。当您不需要 MCP 服务器时，将其禁用。
- **切换到较小的模型来执行日常任务。** 使用 GPT-5.6 Terra 或 GPT-5.6 Luna 可以扩展本地消息使用限制，具体取决于您切换的模型。

有关选择任务和确定任务范围的指南，请参阅 [高效使用工作](prompting.zh-CN.md#use-work-efficiently)。

<a id="feature-availability"></a>

## 功能可用性

<CodexPlanFeatureMatrix
  client:load
  data={{
    plans: [
      { id: "plus", shortLabel: "Plus", label: "ChatGPT加" },
      { id: "pro", shortLabel: "Pro", label: "ChatGPT专业版" },
      {
        id: "business",
        shortLabel: "Business",
        label: "ChatGPT 商务",
      },
      {
        id: "enterprise",
        shortLabel: "Enterprise",
        label: "企业/教育",
      },
      { id: "api", shortLabel: "API Key", label: "API密钥" },
    ],
    sections: [
      {
        title: "通道和使用界面",
        features: [
          {
            name: "Codex cloud",
            href:"cloud.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "网络上的 ChatGPT Work",
            href:"get-started-with-work.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "ChatGPT 用于本地聊天的桌面应用程序",
            href:"app.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Codex CLI",
            href:"https://learn.chatgpt.com/codex/cli",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "IDE extension",
            href:"https://learn.chatgpt.com/codex/ide",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Codex SDK、`codex exec` 和可编写脚本的工作流程",
            shortName: "Codex SDK and scripting",
            href:"codex-sdk.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Codex 用于可信自动化的访问令牌",
            shortName: "Automation access tokens",
            href:"enterprise/access-tokens.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "ChatGPT Excel 版",
            href:"https://help.openai.com/articles/20001063",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
        ],
      },
      {
        title: "模型和多式联运",
        features: [
          {
            name: "GPT-5.6",
            href:"models.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Fast mode",
            href:"agent-configuration/speed.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Codex-Spark research preview",
            href:"models.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "available",
              business: "unavailable",
              enterprise: "unavailable",
              api: "unavailable",
            },
          },
          {
            name: "图像生成和编辑",
            href:"image-generation.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Voice dictation",
            href:"prompting.zh-CN.md#use-voice-dictation",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "ChatGPT Voice",
            href:"features/voice.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Web search",
            href:"web-search.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
        ],
      },
      {
        title: "当地特色",
        features: [
          {
            name: "使用 `/review` 进行本地代码审查",
            shortName: "Local code review",
            href:"prompting.zh-CN.md#do-a-local-code-review",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "自动审核批准请求",
            href:"sandboxing/auto-review.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "沙箱和权限控制",
            href:"permissions.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "项目和独立计划任务",
            shortName: "Scheduled tasks",
            href:"automations.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Scheduled tasks",
            href:"automations.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "工作树和内置 Git 工具",
            shortName: "Built-in Git tools",
            href:"environments/git-worktrees.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "本地环境和可重复的操作",
            shortName: "Repeatable actions",
            href:"environments/local-environment.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Appshots",
            href:"appshots.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "unavailable",
              api: "available",
            },
          },
        ],
      },
      {
        title: "浏览器和远程控制",
        features: [
          {
            name: "内置浏览器预览和评论",
            shortName: "Built-in browser",
            href:"browser.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "计算机在浏览器中使用",
            href:"https://learn.chatgpt.com/codex/browser?surface=app#app-computer-use-in-the-browser",
            availability: {
              plus: "limited",
              pro: "limited",
              business: "limited",
              enterprise: "limited",
              api: "limited",
            },
          },
          {
            name: "将 ChatGPT 与 Chrome 结合使用",
            shortName: "Chrome browser control",
            href:"chrome-extension.zh-CN.md",
            availability: {
              plus: "limited",
              pro: "limited",
              business: "limited",
              enterprise: "limited",
              api: "limited",
            },
          },
          {
            name: "Computer Use",
            href:"computer-use.zh-CN.md",
            limitedFootnote: "region",
            availability: {
              plus: "limited",
              pro: "limited",
              business: "limited",
              enterprise: "limited",
              api: "limited",
            },
          },
          {
            name: "Record & Replay (macOS)",
            shortName: "Record & Replay",
            href:"extend/record-and-replay.zh-CN.md",
            limitedFootnote: "region",
            availability: {
              plus: "limited",
              pro: "limited",
              business: "limited",
              enterprise: "limited",
              api: "limited",
            },
          },
          {
            name: "SSH remote connections",
            shortName: "SSH remote",
            href:"remote-connections.zh-CN.md#connect-to-an-ssh-host",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Mobile remote control",
            href:"remote-connections.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "ChatGPT Web 中的浏览器",
            href:"browser.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
        ],
      },
      {
        title: "定制和扩展",
        features: [
          {
            name: "`AGENTS.md` 的自定义指令",
            shortName: "Custom instructions",
            href:"agent-configuration/agents-md.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Skills",
            href:"build-skills.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Plugins",
            href:"plugins.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "limited",
            },
            limitedFootnote: "plugins",
          },
          {
            name: "Plugin sharing",
            href:"https://developers.openai.com/plugins/build/plugins#share-a-local-plugin-with-your-workspace",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Connectors",
            href:"plugins.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "MCP",
            href:"extend/mcp.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "子智能体和定制智能体",
            shortName: "Subagents",
            href:"agent-configuration/subagents.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Memories",
            href:"customization/memories.zh-CN.md",
            availability: {
              plus: "limited",
              pro: "limited",
              business: "limited",
              enterprise: "limited",
              api: "limited",
            },
          },
          {
            name: "Computer History",
            href:"customization/computer-history.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
        ],
      },
      {
        title: "云和集成",
        features: [
          {
            name: "Codex cloud chats",
            shortName: "Cloud chats",
            href:"cloud.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "云环境和设置脚本",
            shortName: "Cloud environments",
            href:"environments/cloud-environment.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Cloud agent internet access controls",
            shortName: "Internet controls",
            href:"cloud/internet-access.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Sites",
            href:"sites.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "GitHub 与 `@codex` 的发行和公关授权",
            shortName: "GitHub delegation",
            href:"third-party/github.zh-CN.md#give-codex-other-tasks",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "GitHub 代码审查和自动 PR 审查",
            shortName: "GitHub PR reviews",
            href:"third-party/github.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Slack cloud integration",
            shortName: "Slack integration",
            href:"third-party/slack.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Linear cloud integration",
            shortName: "Linear integration",
            href:"third-party/linear.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
        ],
      },
      {
        title: "管理、安全和分析",
        features: [
          {
            name: "SAML SSO、MFA 和工作区用户管理",
            shortName: "Workspace management",
            href:"enterprise/admin-setup.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "`requirements.toml` managed config",
            shortName: "`requirements.toml` config",
            href:"enterprise/managed-configuration.zh-CN.md",
            availability: {
              plus: "available",
              pro: "available",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Cloud-managed config policies",
            shortName: "Cloud-managed policies",
            href:"enterprise/managed-configuration.zh-CN.md#cloud-managed-requirements",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "available",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "ChatGPT 工作区 RBAC 和自定义角色",
            shortName: "RBAC and roles",
            href:"enterprise/roles-and-workspace-permissions.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "SCIM、EKM 和域验证",
            shortName: "SCIM, EKM, and domains",
            href:"enterprise/admin-setup.zh-CN.md#enterprise-grade-security-and-privacy",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "企业保留和驻留控制",
            shortName: "Retention and residency",
            href:"enterprise/admin-setup.zh-CN.md#enterprise-grade-security-and-privacy",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "No training on API or business data by default",
            shortName: "No default training",
            href:"https://openai.com/business-data/",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "available",
              enterprise: "available",
              api: "available",
            },
          },
          {
            name: "Analytics dashboard",
            href:"enterprise/workspace-analytics.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Analytics API",
            href:"enterprise/analytics-api.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "合规 API 和审核日志",
            shortName: "Compliance and audit logs",
            href:"enterprise/compliance-api.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
          {
            name: "Codex Security 用于连接的 GitHub 仓库",
            shortName: "Codex Security",
            href:"security.zh-CN.md",
            availability: {
              plus: "unavailable",
              pro: "unavailable",
              business: "unavailable",
              enterprise: "available",
              api: "unavailable",
            },
          },
        ],
      },
    ],
  }}
/>

<div
  id="codex-plan-region-limits"
  className="not-prose mt-3 text-sm text-secondary"
>
  <sup>*</sup>该功能目前仅限于特定区域。查看各个功能文档以了解有关地理限制的更多信息。


<div
  id="codex-plan-plugin-limits"
  className="not-prose mt-1 text-sm text-secondary"
>
  <sup>†</sup>某些第一方插件不可用。