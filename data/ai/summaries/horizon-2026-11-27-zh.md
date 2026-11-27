# Horizon 每日速递 - 2026-11-27

> 从 88 条内容中筛选出 12 条重要资讯。

---

**AI 应用与 Agent**
1. [腾讯开源 DFly2 复用目标 KV](#item-ai-apps-1) ⭐️ 8.0/10
2. [Clay 重写 GTM Agent](#item-ai-apps-2) ⭐️ 7.0/10
3. [llama.cpp AMD 跑分面板](#item-ai-apps-3) ⭐️ 6.0/10

**科技新闻**
1. [Google Workspace MCP：Gmail 初期不能发邮件](#item-tech-news-1) ⭐️ 7.0/10
2. [荷兰大臣反对美国 MATCH Act 限制 ASML 对华销售](#item-tech-news-2) ⭐️ 7.0/10
3. [微软 CEO 承认应届生招聘放缓：先花完 AI 预算再招人](#item-tech-news-3) ⭐️ 7.0/10

**财经新闻**
1. [国常会部署完善资本市场稳市机制，优化中央汇金类“平准基金”功能](#item-finance-news-1) ⭐️ 8.0/10
2. [中国双 11 智能手机销量同比降 17%，苹果居首、华为增长](#item-finance-news-2) ⭐️ 7.0/10
3. [蜜雪冰城 5 亿元入股 DeepSeek](#item-finance-news-3) ⭐️ 6.0/10
4. [SpaceX 向美国儿童投资账户捐股，马斯克等额配捐](#item-finance-news-4) ⭐️ 4.0/10
5. [美国国土安全部称 ICE 在感恩节前逮捕多名涉暴力与毒品犯罪人员](#item-finance-news-5) ⭐️ 3.0/10
6. [节日问候：西方市场因感恩节休市，帖文征集黑五购物意向](#item-finance-news-6) ⭐️ 1.0/10

---

## AI 应用与 Agent

<a id="item-ai-apps-1"></a>
### [腾讯开源 DFly2 复用目标 KV](https://www.marktechpost.com/2026/11/26/tencent-dfly2-speculative-decoding-sparse-models/) ⭐️ 8.0/10

腾讯已开源 DFly2：一个 0.85B 参数、5 层的推测解码草稿模型，面向 DeepSeek-V4.1-Flash、GLM-5.3-Flash 和自家 Hy4 flash 这类原生稀疏模型。它不再单独维护草稿 KV cache，而是复用目标模型稀疏层的 latent KV 和 indexer 已选出的 top-k 位置，因此草稿阶段不需要 prefill，每个草稿 token 最多看约 2K 个 key。论文自报在 DeepSeek-V4.1-Flash 的 SWE 64K 场景、匹配单用户速度时吞吐最高提升 81%；最差是 Hy4 flash 通用任务在 temperature 0 下仅比 DSpark 高 3.9%。

rss · MarkTechPost · 11月27日 03:31

**「怎么用」** 第一步是从 GitHub 的 Tencent/AngelSpec 仓库取实现，确认目标模型在支持列表内，再按 vLLM 的 Hy4-flash recipe 起服务。接入方式不是改目标模型权重，而是把 drafter 的每个 draft 层与目标模型的稀疏注意力层配对，让它读同一份 latent KV 和 indexer 的 top-k 选择。没有多卡 serving 环境的话，先不要按单机脚本评估。

**「成本与限制」** DFly2 开源代码本身不收费，成本主要在 GPU：论文测试用单实例 8×H20、TP8，Hy4 flash 的 vLLM recipe 默认 8×H200，单卡或小显存没有现成路径。它目前针对列表里的原生稀疏目标；“省掉草稿 cache”的具体账是，常规 3 层 drafter 会在 DeepSeek-V4.1-Flash 上每 token 多加 3.4 KiB（目标 KV 的 1.76 倍），GLM-5.3-Flash 加 3.0 KiB（1.26 倍），Hy4 flash 加 3.4 KiB（1.20 倍），32K 下同一显存池可多放 1.73 倍请求。

**「为什么现在」** 值得现在看，是因为代码、arXiv 论文和 vLLM recipe 已一起放出，能直接对照实现与 benchmark 条件，而不是停留在厂商口径。

**标签**: `#开源项目`, `#成本优化`, `#工具实测`, `#能力边界`

---

<a id="item-ai-apps-2"></a>
### [Clay 重写 GTM Agent](https://latent.space/p/how-clay-builds-gtm-agents-jeff-barg-head-of-ai/) ⭐️ 7.0/10

Clay 的 AI 负责人 Jeff Barg 在 Latent Space 访谈中说明，其 agent 产品 Sculptor 已从「一堆工具循环调用」重构成编码 Agent：跑在 Pi harness 上，在云沙箱里执行 Clay CLI，并新增 Audiences 数据层把 CRM 和 Gong 的记录对齐，让 agent 能同时看到两类数据。配套的 Clay Agent Plugin（CLI 与 API）已经发布，Claude Code 和 Codex 可以直接调用 Clay 的工具与 building blocks；Clay 自述其自有 agent 与外部 harness 的使用量已接近 50/50。成本方面，他给出的是口头估算而非实测：用 TypeSafe 的 Jev 分类器把库里约 4000 万家公司全部按 ICP 打一遍分约需 1500 美元，但 Jev 尚未进入生产环境。此外 Clay 即将上线自动模型路由，模型选择器仍会保留。

rss · Latent Space · 11月26日 23:02

**「怎么用」** 如果只想在现有工作流里调用 Clay，按官方 changelog 安装 Clay Agent Plugin（CLI 与 API），然后在 Claude Code 或 Codex 里直接让它读写 Clay 的表和工具，不需要进入 Clay 界面。如果想自建同类系统，可参考的架构是：用 BullMQ 做 agent 任务队列，Postgres 与 ClickHouse 存产品数据，Snowflake 做分析，接入 LangSmith 看 trace，并自己实现请求限流和供应商限流时的暂停/恢复。

**「成本与天花板」** 按 Jeff 的估算，全量 4000 万家公司做 ICP 分类约 1500 美元，计算依据是每家公司 LinkedIn 页面约 1000 token 加 ICP 描述约 500 token；他给出的合理获客预算是每条合格线索 1–10 美元、每次约到通话 10–100 美元。自建模型后训练的省钱门槛，他估计在推理支出达到 1000 万至 1 亿美元之间，低于这个量级省下的钱不足以抵消投入。已知失败模式包括：用户按行跑 agent，一张 5 万行的表会瞬间打满推理配额；模型会拿相同输入反复重试同一条 CLI 命令；agent 仍无法读长 trace 定位真正的出错步骤，只会标记耗时最长的那一步，所以 Clay 的技能大多是手写的。

**「真实反馈」** 以下均为受访者自述，没有第三方评测。Clay 内部的 GTM 团队习惯留在 Cursor，不肯搬走，这直接促成了 CLI 的诞生；其销售团队现在在 Codex 里做 deal prep，使用量在自有 agent 与外部 harness 之间接近五五开。Clay 也承认客户看到模型选择器会抱怨——他们没有精力为每个新模型重跑自己场景的评测，这正是自动路由要解决的问题。

**「为什么现在看」** 线索打分这类任务对模型已经商品化，Jeff 称 Clay 的评测分数大约一年没有实质性提升，所以真正的新东西只有两块：打通 CRM 与 Gong 的 Audiences 数据层，以及即将上线、按任务自动选模型的 router；把 agent 重写成编码 Agent 更像是把已有能力换了个壳。

**标签**: `#Agent框架`, `#成本优化`, `#工作流`, `#能力边界`

---

<a id="item-ai-apps-3"></a>
### [llama.cpp AMD 跑分面板](https://huggingface.co/blog/ggml-org/llamacpp-amd-bench) ⭐️ 6.0/10

llama.cpp / ggml 团队在 Hugging Face 博客上线了一个面向 AMD Ryzen 与 Radeon 硬件的 benchmark 仪表盘，把 llama.cpp 在这两类 AMD 平台上的推理性能数据集中到一个公开页面。页面直接访问即可，不需要安装、订阅或 API key，主要服务打算用 AMD CPU 或显卡跑本地模型的用户。需要说明的是，目前可确认的只有公告本身：具体测了哪些模型、什么量化格式、多长 prompt、用的是 ROCm 还是 Vulkan 后端，以及 token/s 数字，均未在可见内容中给出，选型前仍需自行打开页面核对。

rss · Hugging Face Blog · 11月26日 11:37

**「怎么用」** 入口就是 Hugging Face 博客上的这个公开页面，打开浏览器即可查看，无任何前置条件。它是数据展示面板而不是可安装组件，llama.cpp 本身的获取方式不变：按发行版从源码编译，或取对应平台的预编译包。看的重点是与你计划部署的模型参数量、量化位数和 GPU 架构最接近的那几行——硬件档位不同的数据不能直接外推。

**「成本与上限」** 这个面板本身免费公开，挂在 Hugging Face 上，不需要 API key 或订阅，因为它衡量的是你自己机器上的本地推理，花钱的地方是 AMD 硬件而不是调用计费。它按 llama.cpp 构建版本、模型和量化分别记录 prompt processing 与文本生成速度，但本条目所附材料里没有任何具体 token/s、延迟或显存占用的数值，实际天花板需要打开面板按你的 Ryzen/Radeon 型号和量化档位自行查表。

**「为什么现在看」** 这次没有新模型也没有新后端，价值在于把原本分散的 AMD 跑分整理成可横向对比的公开页面——对正在挑 Ryzen / Radeon 配置的人是省事，而不是新能力。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://huggingface.co/blog/ggml-org/llamacpp-amd-bench">Introducing the llama.cpp benchmark dashboard for AMD Ryzen ...</a></li>
<li><a href="https://emergent.sh/news/llama-cpp-benchmark-dashboard-amd-ryzen">Llama.cpp Benchmark Dashboard for AMD Ryzen and Radeon</a></li>
<li><a href="https://techplugged.com/llama-cpp-benchmark-dashboard-amd-ryzen-radeon/">ggml-org launches llama.cpp benchmark dashboard for AMD Ryzen ...</a></li>

</ul>
</details>

**标签**: `#工具实测`, `#开源项目`, `#能力边界`, `#模型发布`

---

## 科技新闻

<a id="item-tech-news-1"></a>
### [Google Workspace MCP：Gmail 初期不能发邮件](https://techstackups.com/articles/google-workspace-ai-agents-mcp/) ⭐️ 7.0/10

据报道，Google 将为 Workspace 的 AI 代理提供 MCP 服务器，但 Gmail 集成在最初阶段不能发送邮件。报道把这一限制与人工确认、API 设计以及是否仅向合作伙伴开放等问题联系起来，但没有给出完整的服务器清单、上线时间或独立验证。社区评论中有人称 Gmail API 现已支持发送，并指出 MCP 的 elicitation 机制本可暂停工具调用并要求用户确认，因此限制发送功能更像是商业决策。

hackernews · ritzaco · 11月26日 09:44 · [社区讨论](https://news.ycombinator.com/item?id=50653477)

**「背景」** MCP 是一套让 AI 代理通过统一接口调用外部工具和数据的协议。据该报道，Google 正把 Workspace 的各项服务封装成 MCP 服务器，供 AI 代理调用；其中 Gmail 服务器最初不支持发送邮件，这也成为讨论其权限控制与高风险操作限制的背景。

**「接入代价与权限边界」** 对开发者来说，接入 Gmail MCP 服务器的前提，是先在 Google Cloud 中创建项目，并配置面向 AI agent 的 OAuth 2.0 客户端；授权范围直接决定 agent 能读取和搜索邮件、检索会话与标签，还是在条件允许时发信。因此团队在把 agent 接入收件箱前，应按最小权限原则收紧 scope，并把发信这类不可撤销操作单独纳入人工确认或额外门槛；工具结果也显示，早期报道中“不能发信”的说法可能已随文档更新而变化，实际能力应以当前官方配置指南为准。

**「社区讨论」** 有评论认为，禁止 Gmail 发送只会促使用户让代理操作浏览器发信，过程更繁琐且浪费；另有评论指出，若用 MCP elicitation 在发送前要求用户确认，就不必把发送工具限定为合作伙伴专用。还有用户报告 Gmail API 现在已支持发送，并担心 API 转向按用量计费会限制代理式数据提取。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://chatforest.com/reviews/gmail-mcp-servers/">Gmail MCP Servers — Your Inbox Is Now an Agent ... — ChatForest</a></li>
<li><a href="https://developers.google.com/workspace/gmail/api/guides/configure-mcp-server">Configure the Gmail MCP server | Google for Developers</a></li>

</ul>
</details>

**标签**: `#MCP`, `#Google Workspace`, `#AI agents`, `#Gmail API`, `#API design`

---

<a id="item-tech-news-2"></a>
### [荷兰大臣反对美国 MATCH Act 限制 ASML 对华销售](https://www.bloomberg.com/news/articles/2026-11-26/netherlands-plans-to-fight-rollout-of-proposed-us-curbs-on-asml) ⭐️ 7.0/10

荷兰外贸与发展合作大臣 Sjoerd Sjoerdsma 11 月 26 日在议会表示，荷兰不会接受美国进一步限制 ASML 对华销售，并明确反对 MATCH Act 的域外管辖效力，称荷兰“不会任人摆布”。荷兰正与欧盟委员会协调，并联合德国、法国展开游说，争取把该法案从美国年度国防政策法案中剔除。MATCH Act 由美国两党共同提出，要求荷兰、日本等盟友对华实施更严格的出口管制，否则美方将单方面限制其半导体设备出口；中国约占 ASML 今年销售额的 20%。据外界预计，国会将在 12 月中旬表决，通过的可能性很大。

telegram · zaihuapd · 11月26日 14:56

**「背景」** MATCH Act 是美国两党提出的法案，要求荷兰、日本等盟友对华实施更严格的半导体设备出口管制，否则美方将单方面限制相关设备；据 ioplus 11 月 18 日报道，荷兰政府已正式拒绝该法案，认为其域外管辖侵犯国家主权。11 月 26 日大臣在议会的表态是对这一立场的再次公开重申。另据 TrendForce 报道，该法案还可能影响已安装在中国工厂的设备维修和软件更新。

**「影响」** 如果这一结果成立，受影响的不只是 ASML 本身：按照法案设计，如果荷兰未按要求收紧对华管制，美国就会单方面限制荷兰半导体设备的出口，荷兰政府与企业因此需要在 12 月中旬表决前通过欧盟及德法渠道争取修改或删除相关条款。鉴于中国约占 ASML 今年销售额的 20%，这一营收敞口是荷兰方面评估让步成本的核心数字。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.trendforce.com/news/2026/11/27/news-netherlands-opposes-match-acts-tighter-curbs-on-asmls-china-sales-eyes-eu-countermeasures/">[News] Netherlands Opposes MATCH Act’s Tighter Curbs on ASML ...</a></li>
<li><a href="https://ioplus.nl/en/posts/dutch-reject-us-match-act-over-asml-export-controls">Dutch reject US MATCH act over ASML export controls</a></li>

</ul>
</details>

**标签**: `#ASML`, `#export controls`, `#semiconductors`, `#Netherlands`, `#US-China tech`

---

<a id="item-tech-news-3"></a>
### [微软 CEO 承认应届生招聘放缓：先花完 AI 预算再招人](https://www.windowslatest.com/2026/11/27/lets-add-the-next-human-satya-nadella-admits-new-grad-hiring-slowed-as-companies-rebuild-around-ai-and-says-every-company-will-add-its-next-person-after-it-burns-through-its-ai-budget/) ⭐️ 7.0/10

微软 CEO 萨提亚·纳德拉在 The Pragmatic Engineer 播客中承认，公司应届生招聘已经放缓，现在的做法是先把现有员工调配到合适岗位，之后才补招新人。他同时表示，AI 原生公司会“先用完 AI 预算，再招下一个人类”，并称未来每家公司都会如此。这一表态与微软的人数变化同时出现：截至今年 6 月 30 日，微软员工数为 22.3 万，一年减少 5000 人，为 2016 年以来首次年度下滑。帖文还引用 MIT 与微软研究院的一篇论文称，采用 GitHub Copilot 的企业季度技术岗招聘减少 29%，初级岗位受影响最大；上述数字与该论文结论均来自这篇聚合帖的转述，并非一手报道或独立复核。

telegram · zaihuapd · 11月27日 03:03

**「背景」** 纳德拉今年初在达沃斯还表示“仍然相信校园招聘”，因此这次在 The Pragmatic Engineer 播客上承认应届生招聘放缓，构成其公开口径的明显转变。微软截至 6 月 30 日的员工数为 22.3 万，较一年前减少 5000 人，这是 2016 年以来首次年度下滑，为上述表态提供了公司层面的背景。

**「影响」** 对应届生和初级软件工程师而言，直接后果是入门岗位的开放节奏被推后：按纳德拉描述的顺序，微软只有在内部调配无法填补时才会对外补招，外部新人并非首选。帖文引用的研究进一步把技术岗招聘收缩的主要压力指向初级岗位，这意味着以校招为主要入口的求职者面临的空缺数量可能继续少于资深岗位。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://tsecurity.de/nl/4374116/podcasts-audio-briefings/lets-add-the-next-human-satya-nadella-admits-new-grad-hiring-slowed-as-companies-rebuild-around-ai-and-says-every-company-will-add-its-next-person-after-it-burns-through-its-ai-budget/">“Let’s add the next human”: Satya Nadella admits new-grad ...</a></li>

</ul>
</details>

**标签**: `#AI labor impact`, `#Microsoft`, `#tech hiring`, `#GitHub Copilot`, `#software engineering careers`

---

## 财经新闻

<a id="item-finance-news-1"></a>
### [国常会部署完善资本市场稳市机制，优化中央汇金类“平准基金”功能](https://www.tmtpost.com/nictation/8220182.html) ⭐️ 8.0/10

国务院总理李强 11 月 27 日主持召开国务院常务会议，会议要求持续完善资本市场稳市机制，加力发挥“国家队”稳市作用，进一步优化中央汇金公司的类“平准基金”功能并提供充足流动性保障，同时完善保险、企业年金、社保基金等中长期资金入市的引导支持政策。会议还提出推动上市公司加大分红回购力度、依法强化资本市场全链条监管，但未公布具体规模、工具安排或时间表。

telegram · zaihuapd · 11月27日 05:35

**「背景」** 中央汇金公司增持 ETF 此前被市场解读为“中国版平准基金”入场稳市（tool-1-3）；“国家队”指这类在必要时入场提供流动性的国有资金力量。

**「影响」** 若相关机制落地，中央汇金通过增持股票 ETF 等方式稳市的操作可能更常态化，直接影响持有 A 股 ETF 的投资者；保险、企业年金、社保基金等中长期资金的入市节奏也可能随之加快。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://c.m.163.com/news/a/JSK3C1QH05198CJN.html">“ 中 国 版 平 准 基 金 ”横空出世， 稳 定 市 场将如何发挥作用？ 央 行最新表态</a></li>
<li><a href="https://h5.ifeng.com/c/vivoArticle/v0023sNtM7OKWKId-_ACcAnlZw7Iiz72Ri55n5RHXyKXkyAQ__?isNews=1&amp;showComments=0">信号明确！ 国家队1.28万亿 持 仓动向公开，透露A股哪些深远布局</a></li>
<li><a href="https://www.tkfunds.com.cn/contents/2025/4/21-90425635b8f14babbd8ba24aabd68994.html">多维发力 稳 市 机制建设步 入 新阶段 - 泰康基 金 官方网站</a></li>

</ul>
</details>

**标签**: `#中国资本市场`, `#国务院常务会议`, `#中央汇金`, `#平准基金`, `#中长期资金入市`

---

<a id="item-finance-news-2"></a>
### [中国双 11 智能手机销量同比降 17%，苹果居首、华为增长](https://counterpointresearch.com/en/insights/china-double-11-smartphone-sales-drop-17-percent-on-weak-demand) ⭐️ 7.0/10

Counterpoint Research 数据显示，2026 年 10 月 12 日至 11 月 13 日双 11 促销期，中国智能手机销量同比下降 17%；苹果销量同比下降 8%但仍居第一，华为是唯一增长的主要厂商，销量同比增长 29%。

telegram · zaihuapd · 11月26日 09:38

**「背景」** 双 11 是中国年度电商促销季；Counterpoint Research 称，品牌折扣、优惠券和以旧换新补贴未能抵消组件成本上涨导致的涨价影响。

**「影响」** Counterpoint Research 调查还显示，81%计划半年内购机的中国消费者注意到涨价，更倾向推迟换机而不是接受更高价格，部分换新需求可能因此延后。

**标签**: `#China smartphone market`, `#Double 11 sales`, `#Apple`, `#Huawei`, `#consumer demand`

---

<a id="item-finance-news-3"></a>
### [蜜雪冰城 5 亿元入股 DeepSeek](https://m.sohu.com/a/1107027659_115479) ⭐️ 6.0/10

据搜狐网报道，DeepSeek 完成第二轮融资，规模约 1000 亿至 1055 亿元，投后估值约 6155 亿元；蜜雪冰城股份有限公司通过砺思资本管理的天津砺思星深基金认缴 5 亿元，该报道未获相关公司官方确认。作为参照，蜜雪集团 2025 年营收 335.6 亿元、归母净利约 58.9 亿元。

telegram · zaihuapd · 11月27日 06:33

**「背景」** 据媒体援引《The Information》报道，DeepSeek 已于 2026 年 6 月完成约 510 亿元人民币的首轮融资，因此本次约千亿元融资被报道为第二轮。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.ithome.com/1/020/251.htm">曝 DeepSeek 第 二 轮 融 资 1055 亿 元 ： 估 值 6155 亿 元 ， vivo ...</a></li>

</ul>
</details>

**标签**: `#DeepSeek`, `#蜜雪冰城`, `#融资`, `#AI`, `#风险投资`

---

<a id="item-finance-news-4"></a>
### [SpaceX 向美国儿童投资账户捐股，马斯克等额配捐](https://ir.spacex.com/updates/releases-details/2026/SpaceX-to-Invest-in-the-Next-Generation-of-Americans-2026-r_LYQ5cH94/default.aspx) ⭐️ 4.0/10

SpaceX 宣布将向每个当前有效的 Trump Account（帮助美国儿童积累长期储蓄的税收优惠投资账户）捐出一股新发行的 A 类普通股；马斯克称将按公司捐出的股数等额配捐，使每个孩子共获得两股。SpaceX 总裁兼 COO 格温·肖特韦尔另行宣布个人捐赠超过 200 万股 SpaceX 股票。

telegram · zaihuapd · 11月26日 08:31

**「背景」** Trump Account 是依据《One Big Beautiful Bill Act》设立、于 2026 年 7 月 4 日推出的儿童税收优惠投资账户，用于帮助美国儿童积累长期储蓄。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.sofi.com/learn/content/trump-accounts-for-kids/">Trump Savings Accounts for Kids: Rules &amp; Eligibility (2026)</a></li>

</ul>
</details>

**标签**: `#SpaceX`, `#charitable donation`, `#children&\#x27;s investment accounts`, `#equity`, `#telegram`

---

<a id="item-finance-news-5"></a>
### [美国国土安全部称 ICE 在感恩节前逮捕多名涉暴力与毒品犯罪人员](https://x.com/DHSgov/status/2125673655594090926) ⭐️ 3.0/10

美国国土安全部（DHS）在社交平台 X 上称，移民及海关执法局（ICE）于 11 月 24 日、感恩节前一日逮捕了恋童癖者、强奸犯和毒贩等“公共安全威胁”人员。该帖未公布被捕人数、具体个案或指控细节，相关说法也未获独立核实。

telegram · zaihuapd · 11月26日 14:25

**「背景」** 美国国土安全部于 11 月 25 日发布新闻稿，部长马克韦恩·马林（Markwayne Mullin）在稿中重复了这一说法，并称尽管主张“庇护”政策的政客不断指责，ICE 仍在逮捕并遣返有犯罪记录的无证移民。此类以“最恶劣中的最恶劣”（worst of the worst）为标题的逮捕汇总稿并非首次出现，例如 11 月 6 日也发布过一篇类似稿件。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.dhs.gov/news/2026/11/25/worst-worst-ice-arrests-pedophiles-rapists-and-drug-dealers">WORST OF THE WORST: ICE Arrests Pedophiles, Rapists, and Drug ...</a></li>
<li><a href="https://www.dhs.gov/news/2026/11/06/worst-worst-ice-arrests-pedophiles-sexual-assailants-and-drug-traffickers">WORST OF THE WORST: ICE Arrests Pedophiles, Sexual Assailants ...</a></li>

</ul>
</details>

**标签**: `#immigration enforcement`, `#DHS/ICE`, `#social-media report`, `#US policy`, `#unverified`

---

<a id="item-finance-news-6"></a>
### [节日问候：西方市场因感恩节休市，帖文征集黑五购物意向](https://telegram.me/zaihuapd/45398) ⭐️ 1.0/10

该帖为感恩节问候，指出当天西方放假、稿件不多，并询问读者在黑五（感恩节后的促销日）有什么想入手的商品、哪家活动值得关注；帖文本身未包含任何公司、政策或市场数据方面的实质新闻。

telegram · zaihuapd · 11月27日 04:37

**「背景」** 美国股市在感恩节当天（2026 年 11 月 26 日）休市，纽交所和纳斯达克均暂停交易，债市同样休市；次日“黑色星期五”（11 月 27 日）恢复交易但提前收盘。感恩节后的“黑五”也是美国零售促销的传统起点。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.naplesnews.com/story/news/2026/11/25/stock-markets-schedule-thanksgiving-black-friday/92938770007/">Are stock markets closed on Thanksgiving, Black Friday? All ...</a></li>
<li><a href="https://economictimes.indiatimes.com/world/us/is-the-us-stock-market-open-on-thanksgiving-and-black-friday-2026-nyse-nasdaq-and-bond-market-hours-closures-and-early-trading-schedule/articleshow/136128926.cms">US stock market Thanksgiving and Black Friday hours: Is the ...</a></li>

</ul>
</details>

**标签**: `#holiday market closure`, `#Black Friday`, `#retail promotions`, `#non-news`

---

