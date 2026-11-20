# 营销文案提示词（广告 / 落地页 / 社媒 / 邮件内容 / SEO）

> 一级：B B2B/商业 · 二级：营销文案
> 素材：素材/github/AiCMO-Marketing-Prompt-Collection/（03_Content-&-Creative、04_Demand-&-Lead-Generation-&-Growth 代表篇、07 Humanizer、PROMPT_TEMPLATE.md）；素材/github/gtm-skills/（linkedin/README.md、SWIPE_FILE.md）；素材/web/quickref.me/chatgpt-cheatsheet.md（Paid Ads、Email、SEO、Amazon FBA 段）
> 来源：AICMO/AiCMO-Marketing-Prompt-Collection（Apache-2.0）；Prospeda/gtm-skills（MIT）；QuickRef.ME（内容未声明许可证）

## 1. 概述

解决"从策略 brief 到可投放文案"的生成问题：广告（PPC/信息流）、落地页、社媒钩子与帖子、营销邮件内容、SEO 内容、电商 listing。素材量大（AiCMO 2400+ 篇）但风格高度模式化，可归纳为五个流派：

- **框架派**：以经典文案框架为叙事引擎——AIDA/PAS/BAB 三选一（落地页生成器让用户在 brief 里选定），价值主张用 Osterwalder 画布，钩子用"心理触发 × 表达形式"矩阵（Curiosity×Question、FOMO×Problem-Solution 等五组合各出 2 条）。
- **平台规格派**：把渠道硬约束写进提示——Google Ads 15 条标题×30 字符、4 条描述×90 字符；LinkedIn 连接请求 ≤300 字符；冷邮件 ≤75 词。规格即约束槽位。
- **病毒机制派（gtm SWIPE_FILE）**：不生成而是**拆解**爆款——收集→打分（钩子 25%/互动 20%/结构 20%/可分享 15%/相关 20%，≥3.5 入库）→提取机制→换角度重写。核心公式 `THEIR FORMAT + OUR ANGLE + OUR EXAMPLES = Original post`，明确"偷结构不偷词句"。
- **速查指令派（quickref）**：一句话级广告/SEO 指令（60+ 条 SEO：关键词/内链/外链/竞品/移动与国际化策略），适合做引擎的轻量任务入口。
- **去 AI 味派（Humanizer）**：反向提示词——把机器腔改写成职业人腔，规则逐条针对 AI 写作弱点（主动语态、删套话隐喻、禁 em dash、句长变速、结尾不总结）。

## 2. 共性结构与模式

1. **双版本制**：AiCMO 每篇标配 Quick Copy-Paste Version（填 4-6 个槽即跑）+ Advanced Customizable Version（ROLE/CONTEXT/OBJECTIVE/CONSTRAINTS/OUTPUT 全展开），外加 Example I/O、Success Metrics、Related Prompts（工作流链）、Integration Tips（HubSpot/Salesforce）、Troubleshooting。这是**提示词作为产品文档**的完整形态（见 PROMPT_TEMPLATE.md）。
2. **专家人设+资历数字**：`world-class PPC specialist with 15+ years … $100M in ad spend`。资历描述是风格锚而非事实约束，引擎可参数化为 {{角色}}+{{经验锚}}。
3. **Brief 与指令分离**：落地页生成器分 Part 1（用户填的战略 brief：产品/受众/UVP/唯一转化目标/社会证明/竞品/框架选择/AB 假设）与 Part 2（AI 角色与输出结构）。**输入表单与系统指令解耦**是最值得引擎借鉴的结构。
4. **转化纪律条款**：1:1 Attention Ratio（页面只留一个 CTA，无导航无多链接页脚）、Message Match（标题与上游广告一致）、Benefit-Oriented（写收益不写功能）、CTA 第一人称动词（"Start My Free Trial" 而非 "Submit"）。
5. **变体矩阵**：广告要求 5-10 个版本做 Headline×Description×CTA 组合 AB；钩子要求 10 条且每条标注所用心理组合——**生成即带实验设计**。
6. **负面清单**：禁 "hope this finds you well"、禁 buzzwords、禁 clickbait、禁 humble brag、LinkedIn 帖默认无 hashtag。负面约束比正面要求更能控风格。
7. **成功指标内嵌**：每篇附 Engagement >5%、CTR >3% 等目标值（注意：多为作者宣称，非实证）。

## 3. 代表样本

### 3.1 病毒钩子五组合（素材/github/AiCMO-Marketing-Prompt-Collection/03_Content-&-Creative/Viral-Content-Ideation-Creation-Transformation/Generate-Viral-Hooks.md，Advanced 版节选）

```text
Generate 10 unique hooks … under 15 words … Generate TWO hooks for each:
Combination 1: Curiosity x Question — provocative question opening a curiosity gap
Combination 2: FOMO/Urgency x Problem-Solution
Combination 3: Social Proof x Bold/Contrarian Claim
Combination 4: Emotional Arousal (Aspiration) x 1st-Person Story
Combination 5: Relatability/Identity x Problem-Solution
Output: numbered list, each hook labeled with its psychological combination.
```

### 3.2 落地页核心原则（素材/github/AiCMO-Marketing-Prompt-Collection/04_Demand-&-Lead-Generation-&-Growth/Inbound-Marketing/Landing-Page-Generation/Landing-Page-Generator.md，Part 2 节选）

```text
Act as a world-class CRO specialist, direct-response copywriter, and marketing strategist.
Non-negotiable core principles:
- 1:1 Attention Ratio: NO main navigation, NO multi-link footers, NO off-page links;
  only the primary CTA and essential utility links (e.g., Privacy Policy)
- Benefit-Oriented Copy: outcomes and transformations, not feature lists
- Message Match: headline must align with the upstream ad the user clicked
- Clarity & Scannability: short paragraphs, subheadings, bullets, bold
Output EXACTLY: Headline / Sub-headline / Hero creative brief / Body (chosen framework)
/ Social Proof / CTA / Form field recommendations / Footer / Design & UX tips.
```

### 3.3 PPC 通用结构（素材/github/AiCMO-Marketing-Prompt-Collection/03_Content-&-Creative/Ad-&-Website-Copywriting/PPC-Ad-Copy-Generation.md 节选）

```text
HEADLINE: benefit promise | pain acknowledgment | curiosity question | urgency
DESCRIPTION: specific benefit + social proof + USP + risk reversal/guarantee
CTA: action instruction + urgency + benefit reinforcement + friction reduction
Variations: 5-10 versions mixing Headline A/B × Description A/B × CTA A/B
Platform notes: character limits, format specs, landing page message match,
quality score factors. Google: up to 15 headlines ×30 chars, 4 descriptions ×90 chars.
```

### 3.4 去 AI 味改写（素材/github/AiCMO-Marketing-Prompt-Collection/07_Hybrid-AI-Team-Management/Prompt-Engineering-Best-Practices/Humanizer.md，全文要点）

```text
Rewrite so it reads like one incisive professional talking to another.
Keep every idea and fact. Active voice. Paragraphs ≤3 short sentences.
Vary sentence length; avoid a metronome rhythm. Swap jargon for plain words;
use contractions. Delete clichés, filler adverbs, stock metaphors (navigate,
journey, roadmap). No bullet points unless essential. No summary footer;
end on a crisp final line. Never use em dashes. Inject dry humour if it fits.
Second pass: highlight any sentence that still feels machine-made and fix it.
Return only the rewritten text. TEXT: [YOUR TEXT]
```

### 3.5 爆款改编纪律（素材/github/gtm-skills/SWIPE_FILE.md 节选）

```text
When you find a winner, extract the MECHANICS, not the WORDS.
Steal: hook structure / engagement mechanics / rhythm / proof elements.
Change: actual words / story & context / examples / voice.
THEIR FORMAT + OUR ANGLE + OUR EXAMPLES = Original post.
If someone could put the original and yours side-by-side and say
"this is copied" — you didn't adapt enough.
```

### 3.6 轻量 SEO/广告指令（素材/web/quickref.me/chatgpt-cheatsheet.md，SEO / Paid Ads 段）

```text
Find best keywords for [topic] with low competition
Generate list of keywords targeting multiple geographic locations
Find opportunities for internal linking related to [topic]
Find authoritative websites to acquire backlinks for [topic] content
Research competitor strategies related to [topic]
Create headlines, body text, and calls to action for new fitness program ad
Recommend target Facebook audience for clothing line ad campaign
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 广告文案骨架（平台参数化）

```text
你是 {{平台}} 资深投放文案（{{经验锚}}）。
产品/offer：{{描述}}；受众：{{人群+心理}}；目标：{{awareness/traffic/leads/sales/retargeting}}；
落地页信息：{{URL 或摘要}}（文案必须与其 message match）。
平台硬约束：{{标题条数×字符数、描述条数×字符数、禁用词、格式规范}}。
结构：标题={{利益承诺/痛点/好奇/紧迫 四选}}；正文={{具体收益+社会证明+USP+风险逆转}}；
CTA={{动作动词+降摩擦}}。
输出 {{N}} 个变体（标题×正文×CTA 组合），每个标注测试假设；附关键词嵌入与质量分要点。
```

### 4.2 落地页骨架（brief/指令分离式）

```text
[Part 1 用户 brief]
产品一句话：{{}}；受众角色/目标/最大痛点：{{}}；UVP：{{}}；offer：{{}}；
唯一转化目标：{{}}；品牌语气：{{}}；上游广告信息：{{}}；
社会证明 1-3 条（真实）：{{}}；竞品及卖点：{{}}；真实稀缺/紧迫元素：{{}}；
漏斗阶段：{{ToFu/MoFu/BoFu}}；叙事框架：{{AIDA/PAS/BAB 三选一}}；AB 假设：{{}}。
[Part 2 系统指令]
角色：CRO 专家+直复文案+策略师。原则：1:1 注意力比（无导航/无外链）、
收益导向、message match、可扫读、信任/社会证明/紧迫的心理动线。
输出固定 9 段：标题/副标题/主视觉 brief/框架化正文+3-5 收益 bullet/
社会证明段/CTA 段（第一人称按钮文案）/表单字段建议（按漏斗阶段论证长度）/
页脚占位/3-4 条设计 UX 建议。另生成 1 个 AB 变体并说明差异假设。
```

### 4.3 社媒钩子/帖子骨架

```text
主题/内容摘要：{{}}；受众（人口+心理+痛点+渴望）：{{}}；期望动作：{{}}；平台：{{}}。
生成 {{N}} 条钩子，≤{{15}} 词，覆盖心理组合矩阵 {{好奇×提问 / FOMO×问题方案 /
社会证明×反常识 / 情绪×第一人称故事 / 身份认同×问题方案}}，每条标注组合。
帖子版：首行钩子、短句分行、≤{{150-200}} 词、结尾互动提问、默认无 hashtag；
语气 {{casual/professional/provocative}}；禁 humble brag、禁说教。
```

### 4.4 营销邮件内容骨架（ nurture 向；冷触达见 销售与GTM.md）

```text
业务类型 {{}}；列表规模 {{}}；平台 {{}}；目标 {{nurture/conversion/retention/reactivation}}。
设计：1) 分层策略（行为+人口+生命周期+活跃度）2) 欢迎序列 {{5-7}} 封
3) 培育序列 {{8-12}} 封/{{60}} 天 4) 沉睡唤醒 {{3-5}} 封 5) 行为触发（{{弃购/浏览/里程碑}}）。
每封给：主题行、正文（≤{{N}} 词）、该触点的唯一目标、触发条件与间隔、成功指标。
一封一个 idea；每次跟进必须新增价值，不许纯 "checking in"。
```

### 4.5 SEO 内容骨架（quickref 指令升维）

```text
主题 {{}}；目标市场/语言 {{}}；搜索意图 {{信息/商业/交易}}。
1) 关键词：低竞争主词+地理变体+同义词簇 2) 内容结构：H2/H3 大纲覆盖意图
3) 内链机会 {{站点现有页面}} 4) 外链目标：权威站点清单+理由
5) 元标题/元描述（字符限制 {{}}）6) 竞品内容差距 {{竞品 URL}}。
输出可执行清单，标注优先级与预期影响。
```

### 4.6 去 AI 味终检骨架（可挂在任何文案生成之后）

```text
对以下文案做第二遍改写：保留全部事实与观点；主动语态；段落≤3 短句；
句长变速；删套话隐喻（navigate/journey/roadmap 类）、填充副词、陈词滥调；
{{渠道适配：B2B 禁 emoji/全大写；社媒可保留}}；不用 em dash；结尾不写总结段。
自查一遍仍显机器腔的句子并修复。只输出改写后文本。
[TEXT]{{draft}}[/TEXT]
```

### 4.7 爆款拆解-改编骨架（SWIPE_FILE 式）

```text
分析这条爆款内容：[POST]{{原文}}[/POST]
1) 按 {{钩子强度/互动比/故事结构/可分享性/与我方相关度}} 打分（1-5，加权）
2) 提取机制：钩子类型、节奏（长短句模式）、证明元素（数字/名字/结果）、CTA 样式
3) 用公式"对方结构 + 我方角度 {{}} + 我方例证 {{}}"重写为原创帖；
验收：与原文并排不能被指认为抄袭。只偷机制，不偷词句与故事。
```

## 5. 差异与注意点

- **许可证**：AiCMO Apache-2.0（可商用改编，保留声明）；gtm-skills MIT（低风险）；quickref 内容**未声明许可证**——其指令句式属通用表达可吸收，成段文案需改写。
- **宣称数字不可信**："engagement 300%+"、"300% higher ROI"、"20+ years / $100M ad spend" 是营销话术与风格锚，**不是实证指标**；引擎引用时应剥离或标注为作者宣称，Success Metrics 段可留作目标槽但需用户校准。
- **装饰性伪代码**：Email-Campaign-Automation 等 Advanced 版内嵌 Python 伪代码（class EmailCampaignAutomationEngine…），不可执行、纯示意，组装时应剔除，保留其文字框架。
- **渠道语气冲突**：病毒钩子素材偏 B2C（emoji、全大写、"CRYING/SOBBING" 式情绪词），与 B2B 冷静语气互斥——引擎必须按 {{渠道×受众}} 选择语料分支，不可混用。
- **目录冗余**：AiCMO 存在同职能双目录（Positioning-&-Messaging 与 Messaging-&-Positioning、05 两套 Analytics、06 两套），内容大量重叠，引用时以精读篇为准，勿按目录数估计独立素材量。
- **真实性约束**：落地页 brief 强调社会证明须真实、稀缺须 genuine——引擎应默认加"不得编造客户证言/数据/奖项"条款（广告法合规交叉见 法律/法律研究与合规.md 缺口）。

## 6. 缺口

- 无**中文平台规格**（小红书/公众号/抖音的信息流文案字数、标签、审核敏感词）素材。
- 无**品牌声音一致性**的可执行校验提示词（01 类有 Brand-Governance 长篇但未提供轻量 voice check 骨架）。
- 无文案**合规审查**环节（极限词/虚假宣传/医疗金融广告限制），需与 L 类拼接。
- 视频脚本/视觉资产生成（03 类有目录）本次未精读，AI 绘画交叉部分应归 A 类。
- 缺投放数据回流→文案迭代的闭环提示词（只有 AB 变体生成，无结果解读与下一轮假设）。