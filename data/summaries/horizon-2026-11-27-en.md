# Horizon Daily - 2026-11-27

> From 41 items, 8 important content pieces were selected

---

**Technology News**
1. [Nvidia Reportedly Pauses AI Server Shipments to Four Southeast Asian Nations](#item-tech-news-1) ⭐️ 8.0/10
2. [Report: Zhipu GLM-5.5 and Moonshot Kimi K4 May Slip to 2027](#item-tech-news-2) ⭐️ 8.0/10
3. [AMD-specific fork of llama.cpp targets high-bandwidth local inference](#item-tech-news-3) ⭐️ 7.0/10
4. [Google to expose Workspace MCP servers, but Gmail can&\#x27;t send email](#item-tech-news-4) ⭐️ 7.0/10
5. [Nextcloud Announces Desktop Office App Based on Euro-Office Fork](#item-tech-news-5) ⭐️ 7.0/10
6. [Dutch minister says Netherlands will not accept US bill restricting ASML China sales](#item-tech-news-6) ⭐️ 7.0/10

**Technology Blog**
1. [Who Gets to Decide Whether Something Is AI-Written?](#item-tech-blog-1) ⭐️ 4.0/10

**Financial News**
1. [Semiconductor exports overtake oil and gas for first time](#item-finance-news-1) ⭐️ 8.0/10

---

## Technology News

<a id="item-tech-news-1"></a>
### [Nvidia Reportedly Pauses AI Server Shipments to Four Southeast Asian Nations](https://www.ft.com/content/7838a715-07d4-4605-aa60-21f5af2b2370) ⭐️ 8.0/10

Nvidia has reportedly told major manufacturing partners to pause AI server shipments to Malaysia, Singapore, Thailand, and Indonesia, according to the Financial Times. Dell, Supermicro, and other assemblers that package Nvidia chips into complete servers have told buyers in the four countries that orders cannot leave the factory, the report said. Three people familiar with the matter linked the freeze to a new round of US export controls and said it is unclear whether the measure is temporary; the US Commerce Department last week issued rules requiring regional cloud providers to obtain an export license within 90 days and verify that customers do not include restricted Chinese entities.

telegram · zaihuapd · Nov 26, 07:38

**「Export-control context」** The controls at issue build on the existing US ban on direct exports of Nvidia&\#x27;s most advanced processors to mainland China, which had made Malaysia, Singapore, Thailand, and Indonesia common transshipment points for Chinese AI buyers. The Commerce Department&\#x27;s latest rule adds a 90-day licensing requirement for regional cloud providers and obliges them to verify that customers do not include restricted Chinese entities, tightening the route Nvidia is now pausing shipments through.

**「Impact」** For AI server buyers and cloud operators in the four countries, orders now face delivery uncertainty, and regional cloud providers must meet a 90-day licensing deadline while verifying that customers do not include restricted Chinese entities.

<details><summary>References</summary>
<ul>
<li><a href="https://www.ft.com/content/7838a715-07d4-4605-aa60-21f5af2b2370?syn-25a6b1a6=1">Nvidia halts chip shipments to south-east Asia</a></li>

</ul>
</details>

**Tags**: `#export controls`, `#Nvidia`, `#AI hardware`, `#supply chain`, `#US-China tech`

---

<a id="item-tech-news-2"></a>
### [Report: Zhipu GLM-5.5 and Moonshot Kimi K4 May Slip to 2027](https://www.reuters.com/world/china/us-chip-curbs-distillation-crackdown-threaten-chinese-ai-model-development-2026-11-26/) ⭐️ 8.0/10

Reuters reported, citing unnamed sources, that Zhipu&\#x27;s planned flagship GLM-5.5 — a model of roughly 3 trillion parameters that was expected this month — has not shipped and could slip to the first quarter of 2027 or later, partly because anti-distillation defenses are becoming more effective. Moonshot AI reportedly projects internally that if it loses access to overseas compute entirely, its gap between major releases would stretch from six months to more than a year, pushing the next flagship Kimi K4 to around mid-2027. The report attributes the training-data difficulty to anti-distillation measures from Anthropic and OpenAI, saying Chinese labs have shifted toward distilling Google and Meta models. A screenshot circulating on social media purportedly shows Zhipu denying the report, so the timing remains unconfirmed by the companies.

telegram · zaihuapd · Nov 26, 15:14

**「Background」** Distillation — training a new model on another model&\#x27;s outputs — has been a way for labs to improve capabilities without matching compute, and the Reuters report cites tightening anti-distillation defenses at Anthropic and OpenAI as one reason Zhipu and Moonshot are finding training data harder to obtain, with Moonshot reportedly shifting to distilling Google and Meta models instead. The affected flagships are successors to models already in circulation: Moonshot&\#x27;s Kimi K3 and Zhipu&\#x27;s GLM-5.2, whose release materials serve as the public baselines the delayed GLM-5.5 and Kimi K4 would replace.

**「Impact」** Developers and enterprises building on GLM or Kimi APIs face a materially longer wait for the next flagship versions, and Moonshot&\#x27;s own projection of release intervals exceeding a year signals slower capability upgrades for downstream products. Labs rerouting distillation toward Google and Meta models also means the behavior and benchmarks of future Chinese models may diverge from those trained on Anthropic or OpenAI outputs.

<details><summary>References</summary>
<ul>
<li><a href="https://github.com/MoonshotAI/Kimi-K3">GitHub - MoonshotAI/ Kimi - K 3: Open Frontier Intelligence · GitHub</a></li>

</ul>
</details>

**Tags**: `#China AI`, `#export controls`, `#model distillation`, `#Zhipu`, `#Moonshot AI`

---

<a id="item-tech-news-3"></a>
### [AMD-specific fork of llama.cpp targets high-bandwidth local inference](https://devnull.land/amd-specific-llama-cpp) ⭐️ 7.0/10

A blog post introduces an AMD-specific fork of llama.cpp for local LLM inference on high-bandwidth AMD GPUs, with Hacker News discussion focused on tuning the stack for those cards. The supplied material does not specify supported GPU models, the upstream llama.cpp version, benchmarks, installation steps, or licensing, so the fork&\#x27;s performance and compatibility remain unverified here. It is presented as a separate community fork rather than an upstream llama.cpp change or an independently measured result.

hackernews · julianlam · Nov 26, 17:11 · [Discussion](https://news.ycombinator.com/item?id=50657265)

**「Background」** The AMD-focused approach described here keeps upstream llama.cpp as a stable base and adds small, targeted patches; the rdna-boosts repository is described as a set of small patches applied to base llama.cpp. One gfx906-specific fork targets MI50, MI60, and Vega VII cards to maximize prompt processing and single-card inference, while ROCm&\#x27;s gfx906 support has been available in nightly builds rather than official stable releases.

**「Who benefits and the ROCm caveat」** The people most likely to benefit are owners of older AMD datacenter and workstation cards such as the MI50 and Radeon VII \(gfx906\), which carry large HBM2 memory buffers but lack the tensor cores and native bf16 support that modern llama.cpp paths assume. The practical catch is toolchain support: ROCm covers gfx906 only in nightly builds, not official stable releases, so users should expect to pin a nightly ROCm version and compile llama.cpp against it rather than relying on a packaged stable release.

**「Community discussion」** Commenters place the fork within a wider, informal ecosystem of AMD-specific llama.cpp tuning: one describes circa-2018 gfx906 datacenter cards with 32 GB of HBM2 and roughly 1 TB/s bandwidth selling for about $200, while others report their own ROCm kernel ports and engine results on MI100 and R9700 hardware. These are individual reports and recommendations, not independent verification of the fork.

<details><summary>References</summary>
<ul>
<li><a href="https://devnull.land/amd-specific-llama-cpp">AMD - specific fork of llama . cpp | devnull . land</a></li>
<li><a href="https://news.ycombinator.com/item?id=50657265">AMD - specific fork of llama . cpp | Hacker News</a></li>
<li><a href="https://github.com/thickprogrammer/llama.cpp-gfx906">thickprogrammer/ llama . cpp - gfx 906 : llama . cpp optimized for AMD ...</a></li>
<li><a href="https://news.ycombinator.com/item?id=50657265">AMD -specific fork of llama . cpp | Hacker News</a></li>

</ul>
</details>

**Tags**: `#llama.cpp`, `#AMD GPUs`, `#local LLM inference`, `#GPU optimization`, `#open source`

---

<a id="item-tech-news-4"></a>
### [Google to expose Workspace MCP servers, but Gmail can&\#x27;t send email](https://techstackups.com/articles/google-workspace-ai-agents-mcp/) ⭐️ 7.0/10

Google plans to let AI agents call Google Workspace capabilities through MCP servers, but its Gmail server will not include a send-email tool. Coverage describes the offering as partner-only, with the send restriction tied to Google&\#x27;s concerns about requiring human confirmation before an email goes out. No source content was supplied, so details such as a launch date, server list, or partner requirements are unavailable.

hackernews · ritzaco · Nov 26, 09:44 · [Discussion](https://news.ycombinator.com/item?id=50653477)

**「Background」** Model Context Protocol \(MCP\) is an interface that lets AI agents call external tools and data sources; a community Google Workspace MCP server already packages Gmail, Calendar, Drive, Docs, and other services into one server. Google&\#x27;s own developer documentation says Workspace MCP servers use server-driven human-in-the-loop approvals through MCP form elicitation before sensitive actions such as sending emails.

**「What developers must work around」** Developers building agents on the Workspace MCP servers will be able to read and draft mail but not send it, so any send-email workflow has to route through the partner integrations Google names or a separate API path. The MCP specification already defines elicitation, which lets a server pause a tool call and ask the user to confirm before proceeding, but a filed spec issue notes servers cannot tell whether a human actually clicked accept or the client answered on its own \(tool-3-3\), so gating a send tool behind elicitation would not by itself deliver the human-confirmation guarantee Google cites.

**「Community discussion」** Commenters argued the restriction will not stop agents from sending email, since they can drive a browser or act like a conventional mail client instead, making the workaround messier and more wasteful. One commenter said MCP already has an elicitation primitive that can pause a tool call for user confirmation, so making send partner-only looks more like a business decision than a safety measure; another warned that usage-based API pricing and terms-of-service enforcement are spreading across employers.

<details><summary>References</summary>
<ul>
<li><a href="https://github.com/aaronsb/google-workspace-mcp">GitHub - aaronsb/ google - workspace - mcp : A Model Context Protocol...</a></li>
<li><a href="https://developers.google.com/workspace/gmail/api/guides/configure-mcp-server">Configure the Gmail MCP server | Google for Developers</a></li>
<li><a href="https://modelcontextprotocol.io/specification/draft/client/elicitation">Elicitation - Model Context Protocol</a></li>
<li><a href="https://github.com/modelcontextprotocol/modelcontextprotocol/issues/3589">Elicitation accept &amp; app-only tools don&#x27;t distinguish human ...</a></li>

</ul>
</details>

**Tags**: `#AI agents`, `#MCP`, `#Google Workspace`, `#Gmail`, `#API access`

---

<a id="item-tech-news-5"></a>
### [Nextcloud Announces Desktop Office App Based on Euro-Office Fork](https://nextcloud.com/blog/euro-office-comes-to-your-desktop-with-the-nextcloud-office-desktop-app/) ⭐️ 7.0/10

Nextcloud announced Nextcloud Office Desktop, a desktop application built on Euro-Office, the community fork of ONLYOFFICE. The supplied announcement gives no release date, pricing, or supported operating systems; one commenter identified the interface as a Chromium Embedded Framework app with a C++ core, though the announcement itself does not confirm this. Another commenter noted that ONLYOFFICE 10.0.0 \(12 October 2026\) lists 46 security fixes while Euro-Office&\#x27;s changelog appears to omit some or all of them.

hackernews · e12e · Nov 26, 11:50 · [Discussion](https://news.ycombinator.com/item?id=50654289)

**「Upstream context」** ONLYOFFICE is an open-source document suite whose DocumentServer component provides the editing backend for both browser-based and desktop clients, and Nextcloud already offers an ONLYOFFICE-based online editor inside its platform, a point raised in the discussion. ONLYOFFICE&\#x27;s own posts from October 14, 2026 state that version 10.0 shipped more than 40 security fixes covering the online editors and the desktop app, setting the upstream baseline that any fork must track.

**「Impact」** For organizations considering deployment, the immediate concern is security maintenance: a commenter found that Euro-Office&\#x27;s changelog appears to omit some or all of the 46 security fixes listed for ONLYOFFICE 10.0.0, so administrators should verify patch coverage before relying on the fork.

**「Community discussion」** nathan\_phoenix warned that Euro-Office is a controversial ONLYOFFICE fork and was skeptical of consortium-led development. ModernTimes, who expected to dislike the CEF-based app, reported that the spreadsheet felt and looked superb and welcomed it as an alternative to OpenOffice.

<details><summary>References</summary>
<ul>
<li><a href="https://www.onlyoffice.com/blog/2026/10/security-fixes-onlyoffice-10">ONLYOFFICE 10.0: A closer look at the latest 40 security fixes</a></li>
<li><a href="https://community.onlyoffice.com/t/onlyoffice-10-0-a-closer-look-at-the-latest-40-security-fixes/21043">ONLYOFFICE 10.0: A closer look at the latest 40 security fixes</a></li>

</ul>
</details>

**Tags**: `#open-source`, `#office-suites`, `#nextcloud`, `#onlyoffice-fork`, `#desktop-apps`

---

<a id="item-tech-news-6"></a>
### [Dutch minister says Netherlands will not accept US bill restricting ASML China sales](https://www.bloomberg.com/news/articles/2026-11-26/netherlands-plans-to-fight-rollout-of-proposed-us-curbs-on-asml) ⭐️ 7.0/10

Dutch trade and development cooperation minister Sjoerd Sjoerdsma told parliament on November 26 that the Netherlands will not accept further U.S. restrictions on ASML&\#x27;s sales to China, rejecting the extraterritorial authority of the proposed MATCH Act and saying the Netherlands &quot;will not be pushed around.&quot; The government is coordinating with the European Commission and lobbying alongside Germany and France to have the measure removed from the U.S. annual defense policy bill. The bipartisan MATCH Act would require allies such as the Netherlands and Japan to impose stricter export controls on China or face unilateral U.S. restrictions on their semiconductor equipment. China accounts for roughly 20% of ASML&\#x27;s sales this year, and a congressional vote is expected in mid-December with passage considered likely.

telegram · zaihuapd · Nov 26, 14:56

**「Background」** The MATCH Act is a bipartisan U.S. bill that would press allies such as the Netherlands and Japan to tighten their own export controls on chipmaking equipment sold to China, with unilateral U.S. restrictions as the fallback if they do not. The Dutch government objected to an earlier version of the proposal in May 2026, when Reuters reported The Hague pushing back on a law that could further restrict ASML&\#x27;s China exports. The current dispute is over procedure as much as substance: the measure is being advanced as part of annual U.S. defense legislation, which the Netherlands, coordinating with the European Commission and lobbying alongside Germany and France, wants stripped out.

**「Impact」** If the provision survives into the defense bill, Dutch and Japanese equipment makers could face unilateral U.S. restrictions on China sales even without agreement from their own governments, putting roughly a fifth of ASML&\#x27;s current revenue at risk. The Dutch lobbying push with Germany, France and the European Commission is aimed at removing that exposure before the expected mid-December vote.

<details><summary>References</summary>
<ul>
<li><a href="https://nltimes.nl/2026/11/26/netherlands-draws-line-us-plan-restrict-asml-exports-china">Netherlands draws line over U.S. plan to restrict ASML ...</a></li>
<li><a href="https://www.reuters.com/world/asia-pacific/dutch-government-objects-proposed-us-law-restricting-asmls-china-exports-2026-05-14/">Dutch government objects to proposed US law restricting ASML ...</a></li>

</ul>
</details>

**Tags**: `#semiconductors`, `#export controls`, `#ASML`, `#Netherlands`, `#technology policy`

---

## Technology Blog

<a id="item-tech-blog-1"></a>
### [Who Gets to Decide Whether Something Is AI-Written?](https://seangoedecke.com/who-gets-to-decide-if-something-is-written-by-ai/) ⭐️ 4.0/10

rss · Sean Goedecke · Nov 27, 00:00

**「Background」** Hardly a week passes without a dispute over whether something was written by AI: a prize-winning story, a manuscript that cost its author a contract after Pangram flagged it, a contested tweet. Sean Goedecke asks why people who loathe AI so often loathe the tools built to expose it.

**「Solution」** His answer, consistent with what he calls his blog&\#x27;s central thesis, is power: the real question is who gets to gatekeep literary prizes, pass or fail students, and exclude people from public discourse. He makes the skeptics&\#x27; case sympathetically — Pangram is culturally part of the same tech sector they distrust, and it is a &quot;tool-shaped solution&quot; in a decades-long drift from messy community processes toward legible, app-mediated ones — but he parts ways with them over the alternative. Without detection tooling, judgment falls to community self-policing, which he argues fails because people who don&\#x27;t use AI can&\#x27;t recognize its tells: on Twitter, obvious AI text slips through while long, polite human tweets get accused. Ordinary investigation doesn&\#x27;t rescue this, since skilled AI writing can come with drafts and contemporaneous conversations, and he sees no plausible cryptographic attestation path, dismissing C2PA as the wrong tool. His prescription is that anyone serious about keeping AI out of art and culture should want the most powerful detectors available, ideally open-source or built in-house by governments rather than left to the crowd.

**「Takeaway」** Goedecke concludes that a world where &quot;was this written by AI?&quot; is settled by community consensus is a world with more AI-written stories, articles, and poetry in it — so the argument should be about who should hold that power, not whether detection is distasteful.

**Tags**: `#AI detection`, `#AI-generated content`, `#content governance`, `#community moderation`, `#Pangram`

---

## Financial News

<a id="item-finance-news-1"></a>
### [Semiconductor exports overtake oil and gas for first time](https://www.cnbc.com/2026/11/26/semiconductor-chips-are-the-new-oil-heres-why.html) ⭐️ 8.0/10

Semiconductor exports overtook oil and gas in global export value for the first time in the January–March quarter of 2026, reaching about $1.61 trillion versus $1.53 trillion on a four-quarter rolling basis, according to a Deutsche Bank analysis of International Trade Centre data.

rss · CNBC Finance · Nov 26, 17:52

**「Background」** The &quot;petrodollar&quot; refers to oil exporters recycling surpluses into U.S. dollar assets; Deutsche Bank&\#x27;s Jim Reid says a &quot;chip dollar&quot; could emerge as Asian chip-export earnings flow back into U.S. technology stocks, startups and data centers.

**Tags**: `#semiconductors`, `#global trade`, `#AI investment`, `#market volatility`, `#macroeconomic flows`

---

