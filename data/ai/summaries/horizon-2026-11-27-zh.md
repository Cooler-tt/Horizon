# Horizon 每日速递 - 2026-11-27

> 从 86 条内容中筛选出 12 条重要资讯。

---

**AI 应用与 Agent**
1. [腾讯开源 DFly2 投机解码](#item-ai-apps-1) ⭐️ 8.0/10
2. [llama.cpp AMD 基准看板](#item-ai-apps-2) ⭐️ 7.0/10
3. [20 个 Agent 调优查询引擎](#item-ai-apps-3) ⭐️ 6.0/10
4. [Clay 把 GTM Agent 改成编码 Agent](#item-ai-apps-4) ⭐️ 6.0/10
5. [GPT-6.2 计费修复，额度消耗加快](#item-ai-apps-5) ⭐️ 5.0/10
6. [Sakana 发布 Namazu Small](#item-ai-apps-6) ⭐️ 5.0/10
7. [微软公开 ARTEX 攻击架构](#item-ai-apps-7) ⭐️ 4.0/10
8. [豆包工作接入剪映插件](#item-ai-apps-8) ⭐️ 4.0/10

**科技新闻**
1. [谷歌拟让 AI 代理使用其 MCP 服务器，Gmail 不能发信](#item-tech-news-1) ⭐️ 7.0/10
2. [阿斯麦 CEO 反对对华全面断供光刻设备](#item-tech-news-2) ⭐️ 7.0/10
3. [荷兰大臣反对美国 MATCH Act 限制 ASML 对华销售](#item-tech-news-3) ⭐️ 7.0/10
4. [传智谱 GLM-5.5 与 Kimi K4 发布延期](#item-tech-news-4) ⭐️ 7.0/10

---

## AI 应用与 Agent

<a id="item-ai-apps-1"></a>
### [腾讯开源 DFly2 投机解码](https://www.marktechpost.com/2026/11/26/tencent-dfly2-speculative-decoding-sparse-models/) ⭐️ 8.0/10

腾讯混元 AI Infra 团队开源了 DFly2：一种面向原生稀疏 LLM 的投机解码 drafter（草稿模型）。它的核心改动是 drafter 不再单独维护 KV cache，而是每个 draft 层绑定目标模型的一个稀疏注意力层，直接读取该层的 latent KV 和 indexer 已选出的 top-k 位置，并用目标层权重初始化。随腾讯自家 Hy4 flash 发布的版本为 0.85B 参数、5 层，官方称适配 DeepSeek-V4.1-Flash、GLM-5.3-Flash 和 Hy4 flash 三个目标模型。代码与配方在 GitHub 的 Tencent/AngelSpec 仓库和 vLLM recipe 页面（recipes.vllm.ai/tencent/Hy4-flash）可获取，论文编号 arXiv 2611.34776。所有吞吐与接受率数字均来自项目和论文自测，目前没有第三方复现。

rss · MarkTechPost · 11月27日 03:31

**「怎么用」** 入口是 GitHub 的 Tencent/AngelSpec 仓库，drafter 权重随 Hy4 flash（HuggingFace tencent/Hy4-flash）一起发布，vLLM 侧有现成 recipe。接入时需要按论文把每个 draft 层与目标模型对应的稀疏层配对，共享 KV 与 indexer 选择结果，并把目标最后一层 hidden state 注入 draft block；官方强调 drafter 跳过 prefill，prefill 到 decode 只多传一个 last-layer hidden state。这不是 pip 装完即用的组件：要改 vLLM 的模型配置与注意力路径，作者自己的测试环境是 H20、TP8、单实例。

**「成本与瓶颈」** 论文 Table 2 给出的对比是：常规 3 层 drafter 每 token 额外占 3.4 KiB（DeepSeek-V4.1-Flash，目标 KV 仅 4.4 KiB，涨到 1.76 倍）、3.0 KiB（GLM-5.3-Flash，1.26 倍）、3.4 KiB（Hy4 flash，1.20 倍）；去掉这块缓存后，32K 上下文下同一内存池能装 1.73 倍请求。每个 draft token 最多只注意约 2K 个 key，因此 draft 成本基本不随上下文长度增长。已知表现较差的情况是 Hy4 flash 通用任务、temperature 0：只比 DSpark 高 3.9%；官方称长上下文与 agentic 场景相对 DSpark 的领先为 10.0%–31.5%。

**「为什么现在看」** 投机解码在长上下文下一直受“draft 自己也要存全前缀 KV”拖累，DFly2 是用目标模型已有的稀疏选择结果把这个成本压平的新机制，不是旧方案换名；但收益仍停留在论文自测阶段，先按“值得在自己流量上验一遍”对待。

**标签**: `#开源项目`, `#成本优化`, `#工具实测`, `#模型发布`

---

<a id="item-ai-apps-2"></a>
### [llama.cpp AMD 基准看板](https://huggingface.co/blog/ggml-org/llamacpp-amd-bench) ⭐️ 7.0/10

Hugging Face 上出现了一个 llama.cpp 基准测试仪表盘，专门汇总 AMD Ryzen 处理器与 Radeon 显卡上的本地推理性能数据，目的是让人在挑硬件或调参前先看到可对比的实测结果。它本身不是新模型也不是新功能，而是一个数据汇集页面，任何在 AMD 平台上跑本地模型的人都能直接查阅。由于目前只有标题和简介可确认，页面的具体覆盖范围——收录了哪些模型、哪些量化格式（如 GGUF 的 Q4\_K\_M 一类）、测试用的是 prompt 处理还是 token 生成速度、样本量多少——都还没有核实，实际数字需要打开页面自行查看。

rss · Hugging Face Blog · 11月26日 11:37

**「怎么用」** 入口就是 Hugging Face 博客里的这个仪表盘页面，无需账号或付费，打开后按自己的 CPU/GPU 型号与想跑的模型规模去筛对应条目即可。想验证是否适用于自己的机器，最直接的办法是用 llama.cpp 自带的 benchmark 工具在本地跑一遍同样的模型与量化版本，再和看板上的数字对照。如果你还没装 llama.cpp，需要先从源码编译或走包管理器安装，AMD 显卡路径涉及对应的加速后端，具体编译选项以项目文档为准。

**「为什么现在看」** 没有新能力，本质是把原本散落在论坛帖、个人博客和 issue 里的 AMD 本地推理测试数据集中成一个可对比的看板；价值在于省去自己搜零散数字的时间，但可信度取决于它的测试方法是否公开。

**标签**: `#工具实测`, `#开源项目`, `#能力边界`

---

<a id="item-ai-apps-3"></a>
### [20 个 Agent 调优查询引擎](https://www.qbitai.com/2026/11/513163.html) ⭐️ 6.0/10

据量子位报道，20 个 AI Agents 在 4 天里跑了 350 次实验，把 Quickwit 的查询总耗时降低 31%，单条查询最高降 87%，并称结果超过 ClickBench 最佳公开成绩。原文正文只有一句话，没有给出 Quickwit 的版本号、硬件配置、数据集规模、对比基线以及 Agent 的具体分工方式。因此，「超越 ClickBench 最佳公开成绩」目前只是报道方的说法，第三方无法据此复现或核实。对使用 Quickwit 做日志检索的人，这是一个值得跟进的方向，但还不是可以照抄的结论。

rss · 量子位 QbitAI · 11月27日 03:53

**「怎么上手」** 目前没有可执行路径：报道未提供 Agent 框架、提示词、评测脚本，也没有给出对 Quickwit 配置文件或查询计划的改动 diff。如果要在自己的场景中验证同样的思路，第一步是在 ClickBench 官方仓库找到对应的查询集和硬件口径，先用当前 Quickwit 版本跑一遍自己的基线耗时，再决定是否值得引入多 Agent 自动调优。

**「成本与上限」** 官方博客和转载报道都没有给出支持这次 4 天 350 次实验的 API 费用或算力开销，因此无法估算单次优化尝试的成本。可以量化的是产出率：约 350 次实验中只有 6 项优化通过多智能体评审被保留，其余全部丢弃。收益也限定在同一批 hot query 上——总耗时 40.5 s 降到 27.9 s，hot geomean 0.403 s 降到 0.304 s，报道未说明换用其他数据集、冷查询或不同索引配置后是否仍然成立。

**「为什么值得看」** 「多个 Agent 自动做性能调优」的案例正在变多，但决定它是否可用的关键是能否拿到改动内容和复现配置；这篇目前只给了结果数字，能力增量还有待验证。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.besthub.dev/articles/20-ai-agents-autonomously-optimize-quickwit-beat-clickbench-sota-in-4-days-aa79b97b2079">20 AI Agents Autonomously Optimize Quickwit, Beat ClickBench ...</a></li>
<li><a href="https://www.tabbyml.com/blog/beating-clickbench-sota-with-a-team-of-agentic-systems-engineers/">Beating ClickBench SOTA with a team of agentic systems ...</a></li>

</ul>
</details>

**标签**: `#工具实测`, `#工作流`, `#能力边界`, `#成本优化`

---

<a id="item-ai-apps-4"></a>
### [Clay 把 GTM Agent 改成编码 Agent](https://latent.space/p/how-clay-builds-gtm-agents-jeff-barg-head-of-ai/) ⭐️ 6.0/10

Clay 的 AI 负责人 Jeff Barg 在访谈中说明，Sculptor agent 已从「工具循环」重写为跑在 Pi harness 上的编码 agent：在云沙箱里执行 Clay CLI，并新增 Audiences 数据层，把 CRM 与 Gong 记录按同一实体对齐，让 agent 能同时看到客户数据和活动结果。Clay 还在做按任务自动选模型的路由器，官方称「即将上线」，未给日期；手动模型选择器保留给代理机构和大客户。成本口径是访谈里的粗算：用 TypeSafe 的 Jev 对 Clay 库中约 4000 万家公司逐一做 ICP 分类约需 1500 美元，每条合格线索的 enrichment 预算 1-10 美元，每次成功预约通话 10-100 美元。

rss · Latent Space · 11月26日 23:02

**「最小路径」** 要复刻这条路径，从 Clay 的 CLI/API 入口开始：在 Claude Code 或 Codex 里接入 Clay Agent Plugin（CLI &amp; API），把候选公司表整表交给 agent 做批量筛选和 enrichment，而不是在 Clay UI 里逐行操作。Clay 内部 GTM 团队就是在 Cursor 里用这套 CLI 做准备，销售团队用 Codex。如果只需要判断「这家公司是否符合 ICP」，可以跳过 agent 链路，直接拿 LinkedIn 页面文本（约 1000 token）加 500 token 的 ICP 描述喂给分类器做二分类。

**「成本与上限」** Jeff Barg 的粗略估算（Jev 尚未进入生产）：把 Clay 库里约 4000 万家公司逐家对照 ICP 打一遍分，账单约 1500 美元；他给的获客基准是每条合格线索 1-10 美元、每次成功预约通话 10-100 美元。第三方会议记录称 Clay 每月跑 3.5 亿个 GTM agent，Jev 1.13 的公开定价是每百万输入 token 0.042 美元、输出 token 免费（2026 年 9 月 21 日查询）。已知的失败模式是“每行一个 agent”：有人对 5 万行表格发起运行时，推理需求会瞬间冲高，Clay 靠请求限流和供应商限速时的暂停/恢复兜住，Jeff 自己也不确定 BullMQ 能否再撑十倍增长。

**「实测与边界」** Barg 自述 Clay 内部用量已接近 50/50 分布在自有 agent 和外部 harness 之间，比他自己预期更偏外部工具；早期 Sculptor 擅长加列，但「约到十个合格会议」很差，且用 LLM 评判工具调用的 eval 漏掉了分析表格、百万行企业表这类真实用法。他还说 agent 目前读不懂长 trace，模型倾向把耗时最长的一步当成出错点，Clay 大量 skill 仍靠手写，模型也会用相同输入反复重试同一条 CLI 命令。以上均为作者（Clay AI 负责人）自述，无第三方评测。

**「为何现在看」** 现在值得看的是工程取舍，不是新能力：用编码 agent 替掉自建工具循环、用 BullMQ 加 Postgres/ClickHouse/Snowflake 而不引入 Temporal 或 LangGraph、以及「推理支出达到 1000 万至 1 亿美元量级才值得后训练」的门槛判断，都能直接对照自己的栈。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://docs.typesafe.ai/models">Models - TypeSafe</a></li>
<li><a href="https://www.typesafeai.org/guides/jev-pricing">Jev pricing, without the guesswork - TypeSafe ai</a></li>
<li><a href="https://8thlight.com/insights/production-is-the-new-prototype-notes-from-langchain-interrupt-2026">8th Light | Agents Work. Now the Hard Part: Notes from LangChain</a></li>

</ul>
</details>

**标签**: `#Agent框架`, `#成本优化`, `#工作流`, `#能力边界`

---

<a id="item-ai-apps-5"></a>
### [GPT-6.2 计费修复，额度消耗加快](https://www.techmeme.com/261126/p16#a261126p16) ⭐️ 5.0/10

OpenAI 承认 GPT-6.2 Astra 与 Sol 上线时的计费逻辑有错，把用量打了不该打的折扣；修复后，同样的工作量会吃掉更多套餐额度——Astra 用户触及上限的速度比修复前快 40%，Sol 快 25%。作为补偿，订阅用户获得一次额外的额度重置，商业/企业用户在今年年底前可免费使用 Fast Mode。OpenAI 同时表示正在测试把 Chat 与 Work 两种模式合并。报道没有给出受影响的具体套餐名单、修正后的 token 或消息计费率，也没有说明修复的具体生效日期，因此无法据此重算自己的月度成本。

rss · Techmeme · 11月26日 13:01

**「现在能做什么」** 先确认自己日常跑的是 Astra 还是 Sol：需要省额度的任务优先切到 Sol，它的消耗增幅是 25% 而不是 40%。订阅用户可以直接用掉这次额外重置，商业版用户在年底前把 Fast Mode 打开不会有额外计费。除此之外没有可改的配置项——这次是服务端计费口径的变更，客户端参数调不动它。

**「配额与补偿」** 这次修正的是用量计费方式，并非调价：同样的工作量在 Astra 上会多消耗最多 40% 的套餐额度，在 Sol 上多消耗 25%，受影响的是付费 ChatGPT 订阅用户。作为补偿，订阅用户获得一次额外的用量重置，企业版用户到年底前可免费使用 Fast Mode。OpenAI 未公布按套餐划分的具体额度、新费率或生效日期，因此无法据此重算月度预算。

**「为什么现在看」** 这不是新能力，而是把上周「便宜」的假象收回去：之前的低消耗是 bug，不是降价，所以本月剩余额度的消耗节奏会突然变快，而补偿（额外重置、免费 Fast Mode）带年底这个时间窗口。

<details><summary>参考链接</summary>
<ul>
<li>OpenAI Fixes Billing Error, Tightens Usage Limits on New Models</li>
<li>&#x27;We&#x27;ve fixed this now, so the same work will use more of your plan&#x27;s usage allowance&#x27;: OpenAI admits it made a mistake when launching GPT-6.2 - TechRadar</li>

</ul>
</details>

**标签**: `#成本优化`, `#模型发布`

---

<a id="item-ai-apps-6"></a>
### [Sakana 发布 Namazu Small](https://www.marktechpost.com/2026/11/26/sakana-ai-namazu-small-30b-nemotron-3-nano/) ⭐️ 5.0/10

Sakana AI 在 11 月 26 日公布 Namazu Small，一个 30B 总参数、3B 激活（官方图表标注 30B-A3B）的推理模型：它从 NVIDIA Nemotron 3 Nano 的预训练基座（而非 NVIDIA 后训练版本）出发，由 Sakana 自行做继续预训练与后训练，主打网页搜索和长文档问答。Sakana 自报在 25 项基准上平均 60.1 分，高于 Nemotron 3 Super（120B-A12B，56.2）和 Nemotron 3 Nano（50.2），其中 BrowseComp 73.5 约为 Super 36.1 的两倍。但权重尚未发布，许可证、上下文长度、硬件要求和价格全部未披露，官方称本地部署细节要等到 2026 年 12 月，因此现在没有人能下载或调用它。

rss · MarkTechPost · 11月26日 22:06

**「怎么用上」** 目前没有可安装的包、可调用的 API 或可下载的权重，唯一能做的是盯住 Sakana 的 Namazu Small 官方页面，等 12 月的权重与部署说明。如果现在就需要同量级能力，可以直接取 Hugging Face 上的 Nemotron 3 Nano 基座：30B-A3B 混合 Mamba-Transformer MoE，上下文最高 1M token，NVIDIA Nemotron Open Model License，NVIDIA 标注面向 DGX Spark、H100 和 B200。注意 Sakana 是在该基座的预训练版本上二次训练的，官方没有说明 Namazu Small 本身能否在这些硬件上跑起来。

**「成本与天花板」** 价格、许可证、上下文长度均未公开，无法给出任何单位成本估算。能力边界倒是给得比较清楚：闭卷知识是明显短板，SimpleQA Verified 只有 6.8（Nemotron 3 Nano 9.1、Super 18.1），日文 SimpleQA 8.0（Super 24.9），HealthBench 40.6 低于 Nano 的 47.5，Sakana 自己承认仅依赖内部知识的任务提升不大。基座 Nemotron 3 Nano 的上下文上限是 1M token，Namazu Small 是否保留这一长度未说明。

**「为什么现在看」** 值得记下的信号是“30B-A3B 加针对性后训练，在搜索与长文档任务上压过 120B 模型”这条路线，但所有分数均为 Sakana 自报、无第三方复现、也没有权重可验证，现阶段它只是信息，不是能上手的东西。

**标签**: `#模型发布`, `#能力边界`, `#工具实测`

---

<a id="item-ai-apps-7"></a>
### [微软公开 ARTEX 攻击架构](https://the-decoder.com/microsoft-details-how-state-backed-hackers-use-the-artex-ai-framework-to-automate-cyberattacks/) ⭐️ 4.0/10

微软公开了 ARTEX 的技术架构：这是一个自 2026 年 7 月起被广泛使用的 AI 框架，由多个专用 agent 分工跑完攻击流程，人工介入接近于零。微软称，中国和朝鲜背景的国家级组织以及犯罪团伙都已在用它。微软给出的防御方向是蜜罐、伪造成功信号这类欺骗手段，但对能自我学习调整的 agent 能撑多久，微软自己也没有给出答案。公开材料里没有 agent 的工具调用链、所用模型与参数、运行成本，也没有可写入检测规则的指标，因此读者目前无法据此修改任何配置。

rss · The Decoder · 11月26日 11:30

**「为什么现在看」** 这次真正新增的是「厂商首次公开架构」这一披露动作，并非新技术能力；这些能力自 2026 年 7 月起就已在被使用，而披露深度仍停留在概念层，不足以支撑防御动作。

**标签**: `#Agent框架`, `#能力边界`, `#工作流`

---

<a id="item-ai-apps-8"></a>
### [豆包工作接入剪映插件](https://36kr.com/newsflashes/4087610228046208) ⭐️ 4.0/10

豆包工作宣布「独家」接入剪映官方插件：用户把视频素材交给豆包工作，用一句话描述剪辑需求即可调用剪映完成剪辑，之后能继续用对话改，也可以打开多轨草稿手动细调。官方口径只到功能层面，未公布价格与计费方式、开放范围（全量还是灰度）、是否需要付费订阅、支持的剪映版本、单次可处理的素材时长与分辨率上限，也没有给延迟或成片效果数据。因此目前只能确认「宣布接入」，不能确认普通账号现在就能用上。

telegram · zaihuapd · 11月27日 07:45

**「怎么用上」** 入口在豆包工作内部，不是在剪映里：在豆包工作中选用剪映插件，上传素材，用一句话写清剪辑意图（例如时长、节奏、要保留的片段），生成后继续对话修改，或直接打开多轨草稿做时间线级调整。由于官方没说明灰度规则，能否看到这个插件入口取决于你的账号是否在放开范围内；看不到就是还没轮到，不是操作问题。

**「为什么现在看」** 这更像入口整合，而非新能力：剪辑和对话改片的能力此前都已存在，变化只是把剪映作为官方插件接进豆包工作的对话流程，并允许回写多轨草稿；在开放范围和效果数据出来之前，它值不值得用主要取决于你的账号能不能真的调起这个插件。

**标签**: `#工作流`, `#能力边界`

---

## 科技新闻

<a id="item-tech-news-1"></a>
### [谷歌拟让 AI 代理使用其 MCP 服务器，Gmail 不能发信](https://techstackups.com/articles/google-workspace-ai-agents-mcp/) ⭐️ 7.0/10

据这篇报道，谷歌计划通过 MCP 服务器让 AI 代理使用 Workspace 服务。报道同时指出，Gmail 的 MCP 集成不能发送邮件，这一限制及其原因引发讨论。该消息描述的是平台和 API 层面的接入安排，现有材料没有说明它是否已全面上线，也没有说明哪些账户可以使用。

hackernews · ritzaco · 11月26日 09:44 · [社区讨论](https://news.ycombinator.com/item?id=50653477)

**「背景：MCP 与人工确认」** MCP（Model Context Protocol）是让 AI 代理以统一方式连接外部工具的开放协议；其 elicitation 机制允许服务器在工具调用无法继续时暂停流程，向用户请求确认或补充信息。Google 的 Workspace MCP 服务器文档则说明，Gmail 服务器会在代理执行发送等敏感操作前，通过 MCP 表单式 elicitation 触发由服务器驱动的人工确认。

**「实际影响」** 评论者指出，需要让代理发送邮件的开发者可能转向浏览器自动化等绕行方案，而不是直接调用 MCP 工具，这会增加流程复杂度和资源开销。

**「社区讨论」** 有评论者认为，Gmail 不能发信更像商业决策而非安全考虑，因为 MCP 的 elicitation 机制可以在发送前暂停工具调用并要求用户确认。也有人称 Gmail API 现已支持发送，认为文章信息可能已过时；另有评论认为，MCP 至少迫使谷歌补齐长期缺失的 Workspace API 能力，并预计想走全代理流程的用户会找到绕行办法。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://developers.google.com/workspace/gmail/api/guides/configure-mcp-server">Configure the Gmail MCP server | Google for Developers</a></li>
<li><a href="https://developers.google.com/workspace/guides/configure-mcp-servers">Configure the Google Workspace MCP servers</a></li>
<li><a href="https://www.langchain.com/blog/mcp-in-langchain-stateless-protocol-elicitation-and-more">MCP in LangChain: Stateless Protocol , Elicitation , and More!</a></li>
<li><a href="https://dev.to/kachurun/mcp-elicitation-human-in-the-loop-for-mcp-servers-m6a">MCP Elicitation : Human -in-the-Loop for MCP... - DEV Community</a></li>

</ul>
</details>

**标签**: `#Model Context Protocol`, `#Google Workspace`, `#AI agents`, `#API design`, `#platform policy`

---

<a id="item-tech-news-2"></a>
### [阿斯麦 CEO 反对对华全面断供光刻设备](https://www.semafor.com/article/11/25/2026/asml-makes-the-case-for-selling-chips-to-china) ⭐️ 7.0/10

阿斯麦 CEO 富凯在 Semafor《Future Proof》节目中表示，出口管制应保护最先进的技术，但不该把中国完全排除在外；全面断供只会促使中国在自研上“加倍甚至三倍投入”。他称，阿斯麦卖给中国的设备落后最先进制程十多年，维持这一代差反而让中国继续依赖西方设备；近 30 年来，阿斯麦在中国光刻机市场的份额接近 100%。美国正推动 MATCH 法案，拟把管制范围从 EUV 扩大到浸没式 DUV 和维护服务，荷兰政府和阿斯麦均表示反对。

telegram · zaihuapd · 11月26日 12:19

**「背景：EUV 管制与 MATCH 法案的边界」** 光刻机是芯片制造的核心设备，阿斯麦在先进光刻机上占据主导地位：EUV 用于最先进制程，早已被纳入对华出口管制；而浸没式 DUV 主要服务成熟制程，此前仍在许可销售范围内。美国推动的 MATCH 法案争议点正在于此——是否把管制从 EUV 进一步延伸到浸没式 DUV 设备及其维护服务。

**「对在华晶圆厂的影响」** 若美国推动的 MATCH 法案将管制从 EUV 扩大到浸没式 DUV 及维护服务，中国境内依赖阿斯麦 DUV 设备的晶圆厂将同时面临新机采购受限，以及已装机台服务和技术支持中断的风险，扩产与现有产线维护都会受到直接影响。相关企业需提前评估备件与服务替代方案；该法案目前仍是提案，荷兰政府和阿斯麦均已表示反对。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.thedefensenews.com/US-Pushes-Bill-to-Compel-Netherlands-to-Halt-Remaining-ASML-Machine-Sales-and-Service-to-China/">US Pushes Bill to Compel Netherlands to Halt Remaining ASML ...</a></li>
<li><a href="https://techwireasia.com/2026/04/match-act-semiconductor-export-controls-china-asia/">The MATCH Act Wants to Cut Off China&#x27;s Last Semiconductor ...</a></li>

</ul>
</details>

**标签**: `#semiconductor-export-controls`, `#ASML`, `#China-chip-industry`, `#lithography`, `#tech-policy`

---

<a id="item-tech-news-3"></a>
### [荷兰大臣反对美国 MATCH Act 限制 ASML 对华销售](https://www.bloomberg.com/news/articles/2026-11-26/netherlands-plans-to-fight-rollout-of-proposed-us-curbs-on-asml) ⭐️ 7.0/10

荷兰外贸与发展合作大臣 Sjoerd Sjoerdsma 11 月 26 日在议会表示，荷兰不会接受美国进一步限制 ASML 对华销售，并反对 MATCH Act 的域外管辖效力。荷兰正与欧盟委员会协调，并联合德国、法国游说，争取将该法案从美国年度国防政策法案中剔除。MATCH Act 由美国两党共同提出，要求荷兰、日本等盟友实施更严格的对华出口管制，否则美方将单方面限制其半导体设备；中国约占 ASML 今年销售额的 20%，外界预计美国国会 12 月中旬表决，通过可能性很大。

telegram · zaihuapd · 11月26日 14:56

**「背景：MATCH Act 的域外机制与立法载体」** MATCH Act 由美国两党共同提出，其核心机制是把美国的出口管制延伸到盟友：要求荷兰、日本等国家对华实施更严格的半导体设备出口限制，否则美方将单方面限制这些国家的设备出口。该法案被搭载进美国年度国防政策立法审议，这也解释了荷兰为何选择与欧盟委员会协调、联合德国和法国游说，把相关条款从这部国防法案中剔除，而不是去否决一部独立的专门法案。

**「影响」** 若法案按现文本通过，美国可能绕过荷兰出口许可制度，单方面限制 ASML 对华设备销售；中国市场约占 ASML 今年销售额的 20%，相关限制将直接压缩这部分业务。当前的关键节点是荷兰在 12 月中旬美国国会表决前，能否通过欧盟及德法协调将相关条款从国防政策法案中移除。

**标签**: `#ASML`, `#export controls`, `#semiconductors`, `#Netherlands`, `#US-China tech policy`

---

<a id="item-tech-news-4"></a>
### [传智谱 GLM-5.5 与 Kimi K4 发布延期](https://www.reuters.com/world/china/us-chip-curbs-distillation-crackdown-threaten-chinese-ai-model-development-2026-11-26/) ⭐️ 7.0/10

据 Telegram 频道转述路透社报道，智谱原计划在 2026 年 11 月发布参数规模约 3 万亿的旗舰模型 GLM-5.5，但受反蒸馏防护措施和算力限制等因素影响，发布时间可能推迟至 2027 年第一季度或更晚；月之暗面的下一代旗舰 Kimi K4 也可能推迟至 2027 年年中。报道还称，月之暗面内部预计，若完全失去海外算力，其重大版本发布间隔将从 6 个月拉长到一年以上；同时，Anthropic 与 OpenAI 的反蒸馏措施使其获取训练数据更困难，该公司已转向蒸馏谷歌和 Meta 的模型。上述说法来自简短转述，网传朋友圈截图显示智谱方面对此否认，相关延期尚未获得独立证实。

telegram · zaihuapd · 11月26日 15:14

**「背景」** 所谓蒸馏，通常指用能力更强的模型的输出作为训练数据，以较低成本提升自有模型；Anthropic 此前指控中国 AI 实验室通过大量 Claude 交互实施违规蒸馏，其中阿里巴巴相关活动在 5 月至 7 月涉及逾 1.51 亿次交互。彭博社 2026 年 11 月 25 日还报道，美国新的芯片限制已令涉及字节跳动、阿里巴巴和腾讯的数十亿美元亚洲数据中心贷款受阻，显示算力获取正同步收紧。

**「对开发者的影响」** 对依赖智谱和月之暗面旗舰模型 API 的开发者而言，最直接的后果是升级与兼容规划可能被拉长：GLM-5.5 尚未发布并可能推迟到 2027 年第一季度或更晚，而月之暗面内部预计，若失去海外算力，重大版本间隔会从 6 个月拉长到一年以上，Kimi K4 可能推迟到 2027 年中。因此，基于现有版本构建的产品需要预留更长的支持周期；不过智谱已否认相关说法，延期仍属未经独立证实的报道。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.bloomberg.com/news/articles/2026-11-25/billions-in-data-center-deals-grind-to-halt-after-ai-chip-curbs">US AI Chip Curbs Stall Billions in Asia Data Center Loans... - Bloomberg</a></li>
<li><a href="https://qz.com/anthropic-chinese-ai-labs-distillation-alibaba-deepseek-moonshot-091126">Anthropic accuses Chinese AI labs of illicit distillation attacks</a></li>
<li>EXCLUSIVE: US chip curbs, distillation crackdown threaten Chinese AI model development, sources say | Reuters</li>
<li>Nov 26 (Reuters) - China&#x27;s Z.AI has fallen behind schedule on its next flagship AI model ... - Facebook</li>

</ul>
</details>

**标签**: `#AI models`, `#China AI`, `#distillation`, `#compute constraints`, `#industry news`

---

