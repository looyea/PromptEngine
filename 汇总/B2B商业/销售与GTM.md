# 销售与 GTM 提示词（触达 / 挖掘 / 异议 /  qualification / 成交）

> 一级：B B2B/商业 · 二级：营销文案/销售/客服（销售侧）
> 素材：素材/github/gtm-skills/（outreach、discovery、objections、icp、research、meeting-prep、proposals、follow-up、competitive、methodology/meddpicc.md、role/sdr.md、advanced/README.md、linkedin、openclaw-skills/*/SKILL.md、projects/*.md、docs/AGENTIC-SDR-ARMY.md）；素材/github/AiCMO-Marketing-Prompt-Collection/（02 ICP-Identification、04 Outbound-&-ABM、06 Enterprise-Sales-Enablement-Engine）
> 来源：Prospeda/gtm-skills（MIT）；AICMO（Apache-2.0）。gtm-skills 仓库内 Next.js/tsx 代码不属提示词素材，本篇只归纳 md。

## 1. 概述

覆盖 B2B 销售全循环：ICP 定义→目标客户研究→多渠道冷触达→需求挖掘→异议处理→提案/ROI→跟进/成交→赢单输单复盘。素材以 gtm-skills 为主（README 宣称 2500+ 提示词，实际仓库内约 46 篇 md 承载正文，其余指向其网站），AiCMO 提供 ICP/ABM/销售赋能的长模板补充。六个流派：

- **语气参数派**：冷邮件按 6+ 种语气各给独立模板（Direct/Consultative/Challenger/Casual/Formal/Founder-to-Founder），网站侧扩展为 24 种 tonalities（含 Chris Voss、Sandler 等名人风格）。语气是可枚举槽位，不是形容词堆砌。
- **信号触发派**：按购买信号组织触达——融资后/新高管上任/产品发布/招聘帖/对方发的内容，每种信号给"Angle（为什么此刻相关）+ Include + Tone"。advanced 篇进一步做信号合成：六类信号打分→购买阶段预测→时机预测→行动建议。
- **方法论框架派**：SPIN（四类问题各 4 条）、MEDDIC/MEDDPICC（八要素逐项 status/gap/红黄绿/next steps）、Challenger、Gap Selling、Feel-Felt-Found、Acknowledge-Ask-Advocate、The Reverse（异议反转为购买理由）。
- **角色画像派**：Champion/Economic Buyer/Blocker 三类买家画像各给专用生成器（关心什么/如何评估/典型异议/如何接近）；销售侧按 SDR/AE/Manager/RevOps/CSM/Founder 分 playbook。
- **Agent 角色派（openclaw-skills）**：Scout（研究）/Rep（触达）/Closer（成交）/Writer（内容）多 agent 分工，每个 SKILL.md 是完整系统提示词：人格、黄金规则（"Never end a response without a question or suggestion"）、操作规程（收到请求先问什么）、固定输出格式。docs/AGENTIC-SDR-ARMY.md 给出第一性原理拆解：SDR 工作 90% 可自动化、AE 30%，agent 喂合格会议给人类 AE。
- **项目系统提示派（projects/）**：Claude Projects 自定义指令形态——把一类任务（邮件写作/异议库/研究助手）的 standing rules 写成常驻系统提示，用户后续只发短指令。

## 2. 共性结构与模式

1. **Context 块 + Rules 块 + 编号交付物**：几乎所有提示都是"PROSPECT/CONTEXT（对方是谁、信号、我方产品与痛点）→ CONSTRAINTS/Rules（字数上限、禁语、渠道限制）→ Generate 1..N（结构化交付清单）"。这是 GTM 提示词的标准三段。
2. **字数即纪律**：冷邮件 ≤75 词、主题 ≤5 词、bump ≤25 词、LinkedIn 请求 ≤300 字符、电话异议回应 ≤30 词、语音条 ≤60 秒。渠道物理限制被显式编码。
3. **禁语清单**：No "hope this finds you well"、no buzzwords、no generic compliments、no pitch in first email（consultative）、don't guilt them（跟进）、不贬低竞品（竞争异议）。
4. **表层异议→深层关切**：异议处理统一要求先输出 "What they're really saying (underlying concern)"，再给澄清问题、3 种回应路径、以及**如何在流程更早处预防该异议**。处理-预防-演练（roleplay 怀疑型买家）三件套。
5. **变体与 AB**：首封冷邮件要 3 变体（问题聚焦/好奇提问/社会证明）各带主题行；序列设计附 AB 建议、停止条件、重新激活触发。
6. **阶段间交接文档**：discovery 纪要（9 字段 CRM 格式）→ 定制 demo 议程（3 个要讲的功能+2 个要跳过的）；SDR→AE handoff（摘要/谈话要点/挖掘问题/风险/下一步）。**提示词链的接口被显式定义**。
7. **假设-验证式研究**：pre-call brief 要求"3 个我认为可能成立的假设 + 验证每个假设的问题"，把研究从资料堆砌变成可证伪准备。
8. **双向 qualification**：ICP 不只定义谁该买，还生成 negative filters/红旗信号/何时放弃（"When to walk away vs. push back"），并用赢单/输单/卡单数据反哺 ICP 迭代。

## 3. 代表样本

### 3.1 Direct 语气冷邮件（素材/github/gtm-skills/outreach/README.md）

```text
Write a cold email to [PERSON], [TITLE] at [COMPANY].
Tone: Direct. No fluff. Respect their time.
Context:
- They [RELEVANT SIGNAL: just raised funding / hired for X / launched Y]
- We help [TYPE OF COMPANY] with [PROBLEM]
- Desired outcome: [SPECIFIC RESULT]
Rules:
- Under 75 words
- No "hope this finds you well"
- No buzzwords
- Clear ask at the end
- Subject line under 5 words
```

### 3.2 信号触发性触达（素材/github/gtm-skills/outreach/README.md，After Executive Hire）

```text
Write a cold email to [NEW HIRE NAME], new [TITLE] at [COMPANY].
Angle: New executives have a ~90 day window to make an impact.
They're evaluating everything.
Include:
- Acknowledge the transition
- Reference what's typically top of mind for new [TITLE]s
- Offer a quick win or insight
- Low-commitment CTA
Tone: [SELECT TONE]
```

### 3.3 通用异议处理器（素材/github/gtm-skills/objections/README.md）

```text
I received this objection from [PERSON] at [COMPANY]: "[EXACT OBJECTION]"
Context: Product / Stage [COLD/DISCOVERY/DEMO/NEGOTIATION] / What I know about them
Give me:
1. What they're really saying (underlying concern)
2. Questions to ask to understand better
3. 3 different response approaches
4. How to prevent this objection earlier in the process
```

### 3.4 MEDDPICC 交易 qualification（素材/github/gtm-skills/methodology/meddpicc.md 节选）

```text
Qualify this deal using MEDDPICC.
DEAL: Company / Opportunity / Value / Stage. WHAT I KNOW: [paste notes]
For each element provide: Current status / Gap analysis / Risk (Red-Yellow-Green) / Next steps.
M-Metrics  E-Economic Buyer  D-Decision Criteria  D-Decision Process
P-Paper Process  I-Identify Pain  C-Champion  C-Competition
End with overall deal score and top 3 actions.
```

### 3.5 五触点序列（素材/github/gtm-skills/follow-up/README.md）

```text
Create a 5-email outbound sequence for [PERSONA] at [COMPANY TYPE].
Email 1: Initial outreach
Email 2: Follow-up (3 days later)
Email 3: Value add (5 days later)
Email 4: Different angle (7 days later)
Email 5: Breakup (10 days later)
For each: Subject line / Body (under 75 words) / Goal of that touch.
```

### 3.6 Agent 系统提示词（素材/github/gtm-skills/openclaw-skills/rep/SKILL.md 节选）

```text
You are Rep, an elite B2B sales outreach agent … You're a teammate, not a tool.
Your team: Scout (research) / Rep (you, outreach) / Closer (deals).
The Golden Rule: Never end a response without a question or suggestion.
When they ask for an email: don't just write it. Ask: What's the angle Scout
found? Any previous context or is this cold? What's your one ask? Tone preference?
When they share an objection: diagnose — real objection or brush-off?
What's her situation? New in role? Under pressure?
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 冷触达骨架（语气×信号双槽位，邮件/LinkedIn/语音通用）

```text
给 {{PERSON}}（{{TITLE}} @ {{COMPANY}}）写 {{渠道：冷邮件/LinkedIn 请求/DM/语音条}}。
语气：{{Direct/Consultative/Challenger/Casual/Formal/Founder-peer}}。
触发信号：{{融资/新高管/发布/招聘/对方内容}}，角度：{{该信号为何此刻相关}}。
我方：帮 {{客户类型}} 解决 {{痛点}}，典型结果 {{量化结果}}。
个性化钩子：{{研究所得的具体事实}}。
硬约束：{{渠道字数：邮件≤75词·主题≤5词 / LinkedIn≤300字符 / 语音≤60秒}}；
禁 {{hope this finds you well/buzzwords/泛泛恭维/首封即推销}}；结尾一个明确低承诺 ask。
输出 {{3}} 个变体（问题聚焦/好奇提问/社会证明），各带主题行与所用角度标注。
```

### 4.2 多触点序列骨架

```text
为 {{PERSONA}}（{{行业/规模}}）设计 {{N}} 触点序列，周期 {{T}}，渠道 {{email/phone/LinkedIn 组合}}。
每触点给：Day / 渠道 / 该触点唯一目标 / 内容或脚本大纲 / 主题行。
节奏参考：D1 首触 → D4 bump(≤25词) → D9 价值增量 → D16 换角度 → D26 breakup(给台阶)。
附：AB 测试建议、停止/分手条件、重新激活触发标准。
 nurture 变体：对"现在不买"者每月 1 封×{{4}} 月，每封独立价值+软 CTA，不许像销售跟进。
```

### 4.3 需求挖掘问题生成骨架（SPIN/MEDDIC 可切换）

```text
为向 {{PERSONA}}（{{公司类型/行业}}）销售 {{产品}} 生成挖掘问题，阶段 {{首call/深挖}}。
框架 {{SPIN：Situation/Problem/Implication/Need-Payoff 各 4 条 |
MEDDIC：Metrics/EB/Decision Criteria/Decision Process/Identify Pain/Champion 各 2-3 条}}。
要求：对话式非审讯式；{{高管版：只问战略影响，少而精 | 技术版：环境/集成/安全合规 |
终端用户版：日常工作流与挫败}}。
预算与时间线各给 {{5}} 种不尴尬问法（不直接问"你预算多少"）。
附：30 分钟 call 结构（开场 3'/现状 5'/痛点 10'/影响 5'/愿景 5'/下一步 2'），
每段给过渡语、2-3 关键问题、要听什么信号。
```

### 4.4 异议处理骨架（处理+预防+演练）

```text
异议原文："[EXACT OBJECTION]"；对方 {{角色/公司}}；阶段 {{cold/discovery/demo/negotiation}}；
我方 {{产品/价格/差异化}}；已知背景 {{}}。
输出：1) 深层关切（真异议 vs 托辞判别）2) 澄清问题 3) 三种回应路径
（{{框架可选：Feel-Felt-Found / Acknowledge-Ask-Advocate / Reverse 反转为购买理由}}）
4) 电话版≤30词 + 邮件版稍长带证明点 5) 如何在更早流程预防此异议。
演练模式：扮演怀疑型 {{PERSONA}}，提尖锐但公平的问题；我答得好要承认。
```

### 4.5 ICP 与目标客户骨架

```text
产品 {{描述/解决的痛点/定价/地理市场}}；最佳客户 {{2-3 个}}；失败客户 {{1-2 个}}。
1) 从零构建 ICP：公司特征/情境触发器/Champion 画像/负面过滤/确认 fit 的 qualification 问题
2) 从最佳客户归纳共性 → ICP 标准；列负面信号（公司/时机/人格红旗/销售过程行为）
3) 三类画像分别生成：Champion（被什么考核/怕什么/如何决策）、Economic Buyer
（关心什么与 Champion 不同/如何接近/Champion 该怎么替我们说话）、Blocker（为何阻挠/如何化解）
4) 目标清单：firmographic/technographic/信号过滤+排除项+A/B/C 分层打分与投入策略
5) 用赢单/输单/卡单数据修订 ICP（加什么标准/删什么/追哪个段/放弃哪个段）。
```

### 4.6 交易 qualification 骨架（MEDDPICC）

```text
DEAL：{{公司/标的/金额/阶段}}；已知信息：{{notes}}。
按 M-E-D-D-P-I-C-C 八要素逐项输出：现状 / 缺口 / 红黄绿风险 / 补全动作。
Champion 真伪测试：直接问题+观察信号+行动测试+红旗。
EB 映射：谁控预算/如何确认/接近策略/对其话术/禁语。
Paper Process：采购流程问题/安全与法务审查时间线/并行加速机会。
结尾：整体交易评分 + Top3 行动。
```

### 4.7 研究与会议准备骨架

```text
研究 {{COMPANY}}：业务与商业模式/财务与增长信号/相关高管/技术栈/竞品与近期新闻。
Pre-call brief 五段：关于他（角色 priorities/LinkedIn 动态/履历）；关于公司；
**3 个待验证假设+各自验证问题**；问题清单（开场/5 核心/收尾）；预判异议与应对。
会后：9 字段 CRM 纪要（痛点/现状与栈/决策流程与时间线/预算信号/竞品/Champion vs 怀疑者/
下一步/风险/建议打法）→ 定制 demo 议程（3 讲 2 跳+演示中提问+收尾）。
```

### 4.8 提案与价值证明骨架

```text
执行摘要一页：现状（他们的挑战）→ 影响（不作为的代价）→ 方案 → 成果 → ask。
ROI 论证：我方价格 {{}}；其现状成本 {{}}；改善指标 {{}}；
算 现状成本/预期收益/回本周期/1年与3年ROI，**假设显式列出且保守**。
内部 Champion 弹药包：转发给决策者的邮件草稿（用他们的语言）+ 一页 business case
（问题2句/方案3点/ROI/不作为风险/时间线/下一步）+ 各干系人异议回应。
```

### 4.9 销售 Agent 系统提示骨架（常驻项目/多 agent 用）

```text
你是 {{Scout 研究/Rep 触达/Closer 成交/Writer 内容}} agent，团队 {{分工与交接接口}}。
人格：{{direct/creative/persistent/empathetic/proactive}}。
黄金规则：每次回复必须以问题或建议结尾（推动下一步）。
操作规程：收到 {{任务}} 先澄清 {{角度/上下文/唯一 ask/语气}}；
交付时主动附 {{后续序列/换渠道建议/3天未回复的下一封}}。
Standing rules（以邮件为例）：≤75词；相关性开头而非产品；一封一 idea；
移动端友好；永远给 2-3 选项；跟进必须新增价值。
输出格式：{{固定卡片：收件人/主题/正文/下一步建议}}。
研究类任务：尽可能引用来源；区分事实与推测。
```

## 5. 差异与注意点

- **许可证**：gtm-skills MIT、AiCMO Apache-2.0，均可商用改编（保留声明），引用风险低。
- **数量宣称与实体落差**：README"2500+ prompts"大部分在其网站而非仓库；仓库 md 内大量 `[View all … on prospeda.com]` 外链。引擎只能以仓库内文本为原料，勿假设外链内容存在。
- **厂商私货**：示例输出反复出现 Prospeda 自家卖点（"AI research + human AE review, 50+ leads/month"）、AGENTIC-SDR-ARMY 绑定其 Telegram bot/HubSpot 栈。组装时须把示例中的产品叙事替换为 {{我方产品}} 槽位。
- **语气与合规**：Challenger/contrarian 风格与"不贬低竞品"条款并存——竞争异议素材明确要求 "Response that doesn't trash the competitor"、"plant seeds of doubt" 用证明点而非攻击；引擎应保留该纪律。冷触达还涉及反垃圾邮件法规（CAN-SPAM/GDPR），素材未覆盖（见缺口）。
- **自动化边界**：AGENTIC-SDR-ARMY 自己给出边界——discovery call/demo 不可自动化，SDR 90%/AE 30% 可自动化；Rep 的"persistent"人格需配停止条件（breakup/退订尊重），否则成骚扰。
- **文化迁移**：素材全部是美国 B2B 语境（LinkedIn/冷邮件/电话 gatekeeper）；中文市场微信/企微触达礼仪、称呼与关系语境需重写语气库，字数约束可保留。
- **与营销文案分工**：LinkedIn 个人品牌帖与 SWIPE_FILE 归 营销文案.md；本篇聚焦一对一销售触达与交易推进。ICP 素材两库重叠（gtm icp/README.md 与 AiCMO ICP-Identification.md），后者三步法（全行业发散→五criteria过滤→TopN 综合）更严谨，骨架 4.5 已融合。

## 6. 缺口

- **客服/售后**：分类体系 B 类含客服，但素材只有 CSM 角色提及与 churn 场景（归 产品与运营.md），无工单/投诉/FAQ 客服提示词。
- **谈判深水区**：仅折扣异议与 paper process，无完整谈判开局/让步交换/合同条款博弈素材（可与 L 类合同审查拼接）。
- **冷触达合规**：无退订处理、发送频率上限、数据保护（GDPR/个保法）条款。
- **中文渠道**：微信/企微/电话中文话术、中文行业（制造/政企）采购语境缺失。
- **CRM 数据闭环**：赢单输单分析提示词有，但无与 CRM 字段自动对接的 schema 约定（配套代码在仓库 src/ 中，非提示词素材）。