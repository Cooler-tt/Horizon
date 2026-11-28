# Horizon 每日速递 - 2026-11-28

> 从 68 条内容中筛选出 9 条重要资讯。

---

**AI 应用与 Agent**
1. [datasette-charts 0.4a0 钩子改良](#item-ai-apps-1) ⭐️ 4.0/10

**科技新闻**
1. [Unkernel 将真实 Linux 内核编译为 WebAssembly](#item-tech-news-1) ⭐️ 8.0/10
2. [Google DeepCrossAttention 声称质量相当且最高提速 3 倍](#item-tech-news-2) ⭐️ 8.0/10
3. [Linux nfsd 曝 9.8 分预认证远程 root 漏洞](#item-tech-news-3) ⭐️ 8.0/10
4. [Convex 技能被指未披露上传用户应用构想](#item-tech-news-4) ⭐️ 7.0/10
5. [Michael Nielsen 下架《Neural Networks and Deep Learning》在线版](#item-tech-news-5) ⭐️ 7.0/10
6. [WebRTC 信用卡盗刷脚本现身热门购物网站](#item-tech-news-6) ⭐️ 7.0/10
7. [「假」的计算机检验证明与检查器的可靠性缺陷](#item-tech-news-7) ⭐️ 7.0/10
8. [《我的世界》基岩版资源包漏洞影响 26.52 至 26.63，26.65 已修复](#item-tech-news-8) ⭐️ 7.0/10

---

## AI 应用与 Agent

<a id="item-ai-apps-1"></a>
### [datasette-charts 0.4a0 钩子改良](https://simonwillison.net/2026/Nov/27/datasette-charts/) ⭐️ 4.0/10

datasette-charts 0.4a0 已发布，这是 Datasette 的图表插件的一个 alpha 版本。核心改动是调整「让其他插件定义新图表类型」的 plugin hook，针对先前试用中暴露的摩擦点做改进，但发布说明里没有给出具体 API 变更清单或迁移说明。同版本上线了独立站点 charts.datasette.io，包含面向插件作者的教程和一个 gallery，把折线、堆叠柱状、直方图、散点等核心图表与四类标注（x 值竖线、y 值参考线、区间阴影、点标注）逐个链接到 Datasette Lite 上的可交互 demo。用得上的是已经在写 Datasette 插件、或想在 Datasette 表格页里嵌自定义图表的人；不涉及模型调用、价格或 AI 工作流，普通 AI 工具用户可跳过。安装时需要显式指定 alpha 版本号。

rss · Simon Willison · 11月27日 23:57

**「怎么用」** 运行 \`pip install datasette-charts==0.4a0\` 安装，不写版本号很可能装到旧稳定版；随后照常启动 \`datasette\`，表格页就会出现图表入口。要新增图表类型，需要实现这个插件的 hook，写法见 charts.datasette.io/docs/plugins/ 的教程。如果不想在本地装环境，可以直接点 gallery 里每张卡片的 “Try this demo yourself”，在浏览器内的 Datasette Lite 里跑通再决定要不要落地。标注属于图表 spec 的一部分，会随分享链接一起保留。

**「真实反馈」** 目前的实测信息只有作者自述：他让 Claude Opus 5.5 现写了一个实验性图表插件来测试这个 hook，这次发布修的摩擦点就来自那次试用。没有第三方评测或用户评论，alpha 阶段的稳定性与手感仍属未知。

**「为什么现在看」** 这不是新能力，而是 alpha 阶段的易用性调整和文档补充：hook 之前就已存在，只是没人真正用过；只有你正在写 Datasette 图表插件才值得现在跟进，否则等稳定版更划算。

**标签**: `#开源项目`, `#工作流`, `#工具实测`

---

## 科技新闻

<a id="item-tech-news-1"></a>
### [Unkernel 将真实 Linux 内核编译为 WebAssembly](https://unkernel.dev/) ⭐️ 8.0/10

Unkernel 把真实 Linux 内核编译成 WebAssembly，目标是在浏览器里启动完整的 Linux 环境。项目演示强调快速启动，社区用户也报告 Wayland 合成器运行流畅；这些目前是项目展示和试用反馈，并非独立基准测试。现有材料没有给出内核版本、浏览器兼容范围或可复现的性能数据，说明它仍更像早期系统工程项目。

hackernews · nreece · 11月28日 03:19 · [社区讨论](https://news.ycombinator.com/item?id=50674218)

**「背景」** Unkernel 的核心思路是把 Linux 内核本身编译成 WebAssembly，而不是在浏览器里另写一个 Linux 兼容层。按 Pooya Parsa 的描述，用户程序运行在一个配有 wasm JIT 的 RISC-V CPU 上，这套运行时可在 Node.js、浏览器和 iOS 上使用。

**「影响」** 对希望在浏览器内运行 Linux 或构建相关工具的开发者，Unkernel 提供了可复用的内核级基础：已有开发者表示基于它做出了 unlab 并在浏览器中运行。若要用于生产，仍需自行验证浏览器兼容性和性能，因为当前缺少公开的版本与基准信息。

**「社区讨论」** 有评论者称 Unkernel 启动很快、Wayland 合成器性能“疯狂”，并说自己基于它构建了 unlab；这属于个人体验，不是统一基准。另一位开发者则主张，面向小型专用 agent 沙箱时，POSIX WASM 运行时仍有优势，因为多数场景不需要完整 Linux 内核。

<details><summary>参考链接</summary>
<ul>
<li>Pooya Parsa on X: &quot;Run any Linux program on your device</li>

</ul>
</details>

**标签**: `#WebAssembly`, `#Linux kernel`, `#browser runtimes`, `#systems emulation`, `#Wayland`

---

<a id="item-tech-news-2"></a>
### [Google DeepCrossAttention 声称质量相当且最高提速 3 倍](https://research.google/blog/supercharging-transformer-residual-connections/) ⭐️ 8.0/10

Google Research 提出 DeepCrossAttention（DCA），一种用于 transformer 残差连接的深度方向跨注意力方法，让 token 可以查询此前各层的表示。该研究声称在模型质量与基线相当的情况下实现最高 3 倍加速，并且可以加装到已预训练的 transformer 上。现有材料只呈现 Google Research 的博客声明，没有给出独立测量结果，因此“质量相当”和“最高 3 倍”应视为待复现的结论。

hackernews · ComputedCamp995 · 11月27日 23:06 · [社区讨论](https://news.ycombinator.com/item?id=50672565)

**「背景」** Transformer 的标准残差连接是固定加法：每层输出直接加到输入上，跨层信息如何混合并不由注意力机制决定。此前已有研究把残差连接重新表述为“深度方向的注意力”（如 AttnRes），让模型按层动态路由信息，并指出深度方向 softmax 可使各层输出幅度更受约束。Google Research 的 DCA 延续这一“跨层注意力”思路，用深度维度的 cross-attention 替代固定残差连接。

**「影响」** 如果该改造方法在真实工作负载中成立，开发者可能不必从零重训，就能在已有预训练 transformer 上尝试获得质量提升或效率收益；但部署前仍需在自己的模型和任务上验证加速比与质量，因为当前只有 Google Research 的声明，且“最高 3 倍”并非普遍保证。

**「社区讨论」** HN 评论中，londons\_explore 对在预训练模型上改造 DCA 并短暂继续训练的方向表示期待，认为中途改变模型形状和维度很有前景；pollorollo 则批评博客用折线图连接不同方法缺乏依据。amluto 还猜测跨层注意力可能有助于字母计数类推理问题，但这只是推测。

<details><summary>参考链接</summary>
<ul>
<li>Understanding Attention Residuals in Transformer Models | Satya Mallick ...</li>
<li>Kimi&#x27;s Attention Residuals: What If Depth Had Attention Too? - Medium</li>

</ul>
</details>

**标签**: `#transformers`, `#attention-mechanisms`, `#model-efficiency`, `#google-research`, `#machine-learning`

---

<a id="item-tech-news-3"></a>
### [Linux nfsd 曝 9.8 分预认证远程 root 漏洞](https://opensec.in/blog/nfs2root-cve-2026-99362) ⭐️ 8.0/10

据 OpenSec 披露并经 Telegram 频道转述，Linux 内核 NFS 服务器组件 nfsd 存在 CVE-2026-99362 预认证堆溢出漏洞，CVSS 评分 9.8；攻击者无需登录，只需向 tcp/2049 发起连接即可越界写入内核堆并取得 root shell。该漏洞自 2014 年的 Linux 3.16 起存在，影响启用 NFSv4.1 或 4.2 的文件服务器、NAS 与集群存储节点。修复版本为 6.12.117、6.18.61、7.3.2 和 7.4-rc1，升级后需重启；无法立即升级者可先通过防火墙限制 tcp/2049 访问。CVE 于 2026 年 11 月 20 日公开，上述细节仍需以上游内核公告为准。

telegram · zaihuapd · 11月27日 12:57

**「背景」** nfsd 是 Linux 内核内的 NFS 服务器；OpenSec 的技术说明指出，该漏洞是堆缓冲区溢出，攻击者可通过一条 TCP 连接在某个内核堆对象之后越界写入 61 字节，串联利用后可取得 root shell。BleepingComputer 报道称，这一缺陷自 2014 年 3 月发布的 Linux 3.16 起就已存在。

**「暴露面与处置约束」** 对运维方而言，这一漏洞的关键约束是修复必须升级内核并重启才能生效；无法立即重启的文件服务器、NAS 和集群存储节点，只能在此期间通过防火墙限制 tcp/2049 访问来缩小暴露面。由于仅需一条可达 2049 端口的 TCP 连接即可越界写入内核堆，并串联获得 root shell，任何对外提供 NFSv4.1/4.2 服务且端口可达的节点都应视为可直接被攻击的目标。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://opensec.in/blog/nfs2root-cve-2026-99362">NFS2ROOT ( CVE - 2026 - 99362 ): pre-auth root in Linux nfsd | OpenSec</a></li>
<li><a href="https://www.bleepingcomputer.com/news/security/new-nfs2root-linux-flaw-can-let-attackers-gain-root-privileges/">New NFS2ROOT Linux flaw can let attackers gain root privileges</a></li>
<li><a href="https://opensec.in/blog/nfs2root-cve-2026-99362">NFS2 ROOT ( CVE - 2026 - 99362 ): pre-auth root in Linux nfsd | OpenSec</a></li>
<li><a href="https://www.getpagespeed.com/server-setup/security/nfs2root-cve-2026-99362-centos-7">NFS2 ROOT ( CVE - 2026 - 99362 ) on CentOS 7: Fixed Kernel</a></li>

</ul>
</details>

**标签**: `#linux-kernel`, `#security-vulnerability`, `#nfs`, `#remote-exploit`, `#patching`

---

<a id="item-tech-news-4"></a>
### [Convex 技能被指未披露上传用户应用构想](https://github.com/convex/convex-backend-skill/issues/64) ⭐️ 7.0/10

一项针对 Convex \`convex-backend-skill\` 的 GitHub issue 指控，该编程技能会把用户的应用构想上传到远程服务器，却没有清楚告知用户，引发隐私与同意方面的担忧。Convex CEO Jamie 在 Hacker News 评论中确认存在提示上传，称它用于 Convex 的 “quickstart” 功能；该功能是最近关闭的端到端 vibe-coding 产品 Convex Chef 的继任者。现有回应未详细说明用户如何被告知或能否选择退出，相关指控也尚无独立验证。

hackernews · MickyTheMouse · 11月27日 18:02 · [社区讨论](https://news.ycombinator.com/item?id=50669765)

**「背景」** 背景上，Convex 是一个提供数据/代码托管和 AI gateway 的开发平台，其 CEO 在讨论中称公司曾推出端到端“vibe-coding”产品 Convex Chef，但近期已关闭。此次涉事的上传属于 quickstart skill：相关 SKILL.md 会把一句话项目想法 POST 到 basic-anteater-667.convex.site，并在注释中标记为 telemetry；CEO 称 quickstart 是 Chef 的精神继任者。

**「影响」** 对使用该技能的开发者而言，若在提示中写入尚未公开的产品构想，内容可能被发送到 Convex 服务器；在披露和退出机制明确前，团队应审查该插件的数据流，或避免输入敏感构想。

**「社区讨论」** 评论者提出了不同角度：有人主张长期应转向自托管开放权重模型，也有人指出提示注入比模型主动发起恶意工具调用更常见，但表示不能据此断定此次是恶意行为。另有评论以“想法很廉价”淡化应用构想泄露的影响。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://github.com/convex/convex-backend-skill/issues/64">Quickstart skills send the user&#x27;s idea to a convex .site host and run...</a></li>

</ul>
</details>

**标签**: `#privacy`, `#AI coding tools`, `#developer tooling`, `#prompt handling`, `#Convex`

---

<a id="item-tech-news-5"></a>
### [Michael Nielsen 下架《Neural Networks and Deep Learning》在线版](https://neuralnetworksanddeeplearning.com/) ⭐️ 7.0/10

Michael Nielsen 已将其免费在线教材《Neural Networks and Deep Learning》从原站点下架，影响了依赖该入口学习神经网络的读者。社区讨论普遍将此举解读为一次抗议，但现有材料未提供 Nielsen 的完整公开说明或下架公告。讨论中已有人贴出网页存档链接，作为原站内容不可用后的替代获取方式。

hackernews · brandonb · 11月27日 19:31 · [社区讨论](https://news.ycombinator.com/item?id=50670757)

**「背景」** 被下架的《Neural Networks and Deep Learning》是 Michael Nielsen 编写的免费在线书，网站标注的日期为 2019 年 12 月，内容涵盖神经网络与深度学习的基础。该书此前以公开可访问的在线形式存在，因此此次撤下并非普通商业教材停售，而是直接改变了读者获取这一入门材料的途径。

**「影响」** 对把该书当作免费入门材料的读者和课程维护者来说，直接后果是原站链接不再提供正文，只能转向网页存档或第三方镜像，并需注意这些副本可能与作者版本不一致。

**「社区讨论」** 有评论认为下架是失策，理由是让外部人理解大公司内部如何训练模型，有助于公众预判相关后果；也有人认为这更像象征性抗议，并指出该书已广泛流传，删除反而可能提升关注。另有读者回忆，这本十多年前读到的书在正式 AI 课程之外真正让自己理解了神经网络，并影响了其职业方向。

<details><summary>参考链接</summary>
<ul>
<li><a href="http://neuralnetworksanddeeplearning.com/">Neural networks and deep learning</a></li>

</ul>
</details>

**标签**: `#neural networks`, `#machine learning education`, `#open access`, `#community discussion`, `#deep learning`

---

<a id="item-tech-news-6"></a>
### [WebRTC 信用卡盗刷脚本现身热门购物网站](https://www.infohawk.com/news/2026-11-27-black-friday-webrtc-skimmer) ⭐️ 7.0/10

据 Infohawk 报道，热门购物网站上发现了使用 WebRTC 的信用卡盗刷脚本，攻击者试图利用这一通道规避检测，并把移动用户作为重点目标。报道将此事与黑色星期五购物高峰联系起来，风险集中在嵌入第三方脚本的电商页面。由于未提供受影响站点名单、样本数量或拦截数据，这仍是一份待独立验证的威胁报告，而非已量化的攻击结果。

hackernews · scanr · 11月27日 15:40 · [社区讨论](https://news.ycombinator.com/item?id=50668145)

**「背景」** 信用卡盗刷脚本通常以 JavaScript 形式注入电商结账页，Magecart 式攻击会截取付款表单数据并外传；这类脚本还常经过混淆，以绕过检测。WebRTC 是浏览器内建的实时通信接口，第三方脚本可以直接调用，因此它也可能被滥用作数据外传或规避监控的通道。

**「影响」** 对电商运营者而言，直接后果是需要审查支付页面加载的第三方脚本，并关注 WebRTC 相关流量；只依赖既有脚本检测可能不足以及时发现这类盗刷活动。

**「社区讨论」** 评论区的核心观点是，问题不只在 WebRTC 本身：有用户指出该协议只是较少被监控，攻击者同样可以用普通 HTTP 请求实现，真正风险来自站点嵌入第三方脚本（afavour）。另有用户从广告拦截和“浏览器不应执行陌生代码”的角度主张收紧第三方代码，也有人报告黑五期间在海外网站正常付款后银行卡被银行即时拦截并换卡（NordStreamYacht）。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.bleepingcomputer.com/news/security/caramel-credit-card-stealing-service-is-growing-in-popularity/">Caramel credit card stealing service is growing in popularity</a></li>
<li><a href="https://cybersecuritynews.com/magecart-hackers/">MageCart Hackers Steal Customer Credit Card Data from Camera...</a></li>

</ul>
</details>

**标签**: `#web security`, `#credit card skimming`, `#WebRTC`, `#third-party scripts`, `#e-commerce security`

---

<a id="item-tech-news-7"></a>
### [「假」的计算机检验证明与检查器的可靠性缺陷](https://cr.yp.to/proofs/falseccp.html) ⭐️ 7.0/10

cr.yp.to 上的一篇文章讨论「假的计算机检验证明」，即那些通过了证明检查器、但实际上并不成立的证明，并把问题追溯到检查器自身的可靠性（soundness）实现缺陷。文章以 Lean、Metamath 等系统为背景，涉及多重独立验证器的做法以及对形式验证的实际信任程度。该条目没有附带页面正文，因此文中列举的具体案例、受影响的检查器与版本、以及最终结论细节无法在此核实。

hackernews · \_tk\_ · 11月27日 15:26 · [社区讨论](https://news.ycombinator.com/item?id=50667989)

**「背景」** 所谓“计算机检查过的证明”，是指由证明检查器逐条核验形式化证明的每一步，而不是依赖人工审阅；因此一旦检查器本身存在 soundness bug，它就可能接受实际上不成立的证明。cr.yp.to 的这篇文章讨论的正是这类虚假的计算机检查证明与检查器可靠性问题，文中还提到 2022 年曾用计算机检查一个独立 HOL/OpenTheory 证明检查器的正确性。

**「实际影响」** 对使用形式证明的团队而言，实际影响是不能把某一种检查器的接受当作结论正确的充分条件：关键结论要么经过多个独立实现的交叉验证，要么保留人工复核。Metamath 主库的现行做法就是要求同一证明通过五个独立开发、分别用不同语言实现的检查器，因此单个检查器的健全性缺陷本身不足以让错误证明进入 set.mm 或 iset.mm。

**「社区讨论」** 评论区的核心分歧在于单个检查器的漏洞究竟意味着多大的实际风险：twotwotwo 认为，要让数学家在实质结论上被误导，需要多个检查器同时出错、且证明在非形式审阅下不显得可疑，并提到有 DeepMind 研究者利用 Lean 与当时刚发布一周的 nanoda 各自的不同缺陷，构造出一份两边都通过的「证明」。dwheeler 补充说 Metamath 的 set.mm 与 iset.mm 要求证明通过 5 个用不同语言独立编写的验证器，gr\_norm 则认为这些检查器数十年积累的可靠性缺陷「一条推文就能写完」，对绝大多数软件足够用；andai 另提出一类问题——被验证的实现可能只是符合了验证方对需求本身的错误理解。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://cr.yp.to/proofs/falseccp.html">False computer - checked proofs (djb)</a></li>

</ul>
</details>

**标签**: `#formal verification`, `#proof checkers`, `#soundness`, `#Lean`, `#Metamath`

---

<a id="item-tech-news-8"></a>
### [《我的世界》基岩版资源包漏洞影响 26.52 至 26.63，26.65 已修复](https://www.zaikei.co.jp/article/20261119/878386.html) ⭐️ 7.0/10

《我的世界》基岩版存在资源包漏洞：含 content.zipe 文件的资源包可以改动游戏界面框架 Ore UI 的文件并运行隐藏脚本。附加组件开发者 8Crafter 于 11 月 15 日在 Reddit 公开该问题，称已确认影响 26.52 至 26.63 版本，更早版本也可能受影响，覆盖 PC、手机和主机，Java 版不受影响。玩家加入服务器或 Realm 时若开启资源包自动下载，可能无需手动安装就收到问题文件；被列举的利用方式包括删除全部存档、获取玩家 IP，以及在 Windows 上特定条件下无提示打开任意 URI、启动资源包内的批处理文件下载恶意软件。Mojang 已在 26.65 版修复；AxmBro 发布的 Windows 只读检测工具可查找 content.zipe，但不能判定资源包是否恶意。

telegram · zaihuapd · 11月27日 18:04

**「背景」** 该漏洞的入口在于：资源包可以携带 content.zipe 文件，并改写基岩版界面框架 Ore UI 的文件，从而运行隐藏脚本；玩家加入服务器或 Realm 时，若开启了资源包自动下载，就可能被动收到这类文件。报道显示，8Crafter 于 11 月 15 日在 Reddit 公开该问题，称 26.52 至 26.63 版本受影响，更早版本也可能受影响，而 Java 版不受影响。

**「对玩家与服务器的处置要求」** 仍停留在 26.52 至 26.63 的基岩版玩家，需要把客户端升级到 26.65 才能消除这条通过自动下载资源包触发脚本执行的路径；在无法立即升级时，关闭加入服务器或 Realm 时的资源包自动下载，是唯一能自行控制暴露面的办法，因为问题包只有玩家主动加入他人服务器后才会被送达。8Crafter 公布的利用方式已包括删除全部存档与获取玩家 IP，Windows 上还可触发批处理文件下载恶意软件，因此受影响版本的存档与网络信息应按已暴露处理，而不只是当作显示异常。AxmBro 发布的 Windows 检测工具只能查出资源包内是否存在 content.zipe 文件，无法判定其是否恶意，所以它适合用来筛查可疑包，不能替代升级，也不能作为服务器方判断资源包安全与否的依据。

**标签**: `#Minecraft Bedrock`, `#security vulnerability`, `#resource packs`, `#malware`, `#patch`

---

