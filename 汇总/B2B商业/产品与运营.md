# 产品营销与运营提示词（战略 / 定位 / 营销科学 / 客户运营 / AI 团队治理）

> 一级：B B2B/商业 · 二级：产品设计/PRD（营销侧）、工作流/自动化（营销运营侧）
> 素材：素材/github/AiCMO-Marketing-Prompt-Collection/（01_CMO-&-Leadership、02_Product-Marketing、05_Analytics-&-Marketing-Operations、05_Analytics-&-Performance、06_Customer-Success-&-Retention、06_Market-Specific-Playbooks、07_Hybrid-AI-Team-Management、Autonomy-Evaluation-Framework.md、PROMPT_TEMPLATE.md）；素材/github/gtm-skills/competitive/README.md；素材/web/quickref.me/chatgpt-cheatsheet.md（Amazon FBA、Spreadsheet 段）
> 来源：AICMO（Apache-2.0）；Prospeda/gtm-skills（MIT）；QuickRef.ME（未声明许可证）

## 1. 概述

覆盖"营销作为一个经营系统"的提示词：CMO 级年度规划与预算、产品定位与价值主张、营销科学（MMM/归因/LTV:CAC/实验）、客户全生命周期运营（churn 预防/扩展收入/QBR/赢回）、行业打法、以及管理 AI 营销团队本身的元提示词。AiCMO 按 CMO 职能组织 2400+ 篇，本篇归纳其**系统设计型**提示词（区别于 营销文案.md 的内容生成型）。四个流派：

- **经营文档派**：输入一组 SaaS 经营指标（ARR/NRR/ACV/churn/CAC/转化率/预算），输出董事会可用的年度计划——情境分析、MQL→SQL→Pipeline→ARR 分季转化模型、分渠道预算（%+$+理由）、季度 OKR（先行/滞后指标）、Top5 战略押注及预期 ROI、编制与 MarTech 优先级、三段式董事会叙事。
- **营销科学派**：MMM（贝叶斯框架、adstock 衰减、饱和曲线、基线 vs 增量分解、边际 ROAS、渠道协同）、增量实验与因果归因、LTV:CAC 优化、预测性收入预测。特点：要求用户提供 12-24 个月真实数据，提示词负责分析框架而非编数。
- **客户生命周期派**：churn 风险评分（10 个先行指标+加权公式+红/黄/绿三级+自动升级触发事件）→分层干预 playbook（责任人/触达序列/给什么/升级路径）→扩展信号（8 个行为+firma 信号、"When [event], send [message] with [offer]" 触发逻辑）→QBR 自动备料→30/60/90 天赢回。目标锚定 NRR>110%。
- **AI 团队治理派（07+框架）**：把营销团队本身当作被设计对象——agent 角色重设计、交接协议、治理与风险框架、提示词工程优化（DIAGNOSE→ENGINEER→TEST CASES→ITERATION LOG）、内容自治度 0-5 级评估框架（作者判断：市面工具多宣称 L5，实际最好 L3、多数 L2）。

## 2. 共性结构与模式

1. **指标上下文块**：系统设计型提示一律先要经营数字（ARR、NRR、logo churn、ACV、销售周期、MQL→SQL→Opp→Close 各级转化、pipeline coverage、blended CAC、LTV:CAC）。**没有数字不让模型空谈战略**，是与内容生成型提示的最大区别。
2. **组件化交付**：输出不是文章而是"系统"——编号组件（评分模型/分层 playbook/触发逻辑/序列文案/仪表盘指标），每个组件要求"可在 spreadsheet 或 CS 平台落地"的精度（加权公式、字段、SLA 天数）。
3. **评分-分层-触发三件套**：churn 与 ABM 账户优先级共用模式：指标清单→加权打分→Red(14天行动)/Yellow(30天)/Green(月度监控)→无视分数直接升级的触发事件→每层责任人与动作。gtm advanced 的信号合成同构（六类信号 1-10 分→购买阶段→时机→行动→信号缺口）。
4. **ROLE/CONTEXT/OBJECTIVE/CONSTRAINTS/OUTPUT SCHEMA/QUALITY GATE**：07 类提示词工程篇给出的六段生产级结构，含"至少 5 条显式禁止规则"和"回复前自评清单"——这是 AiCMO 全库最接近工程规范的骨架。
5. **双版本+配套文档**：Quick/Advanced 双版本、Example I/O、Success Metrics、Related Prompts 链（ICP→Value Prop→GTM→Landing Page→Email）、Integration Tips、Troubleshooting（见 PROMPT_TEMPLATE.md）。
6. **赢单输单复盘式（gtm competitive）**：固定五问——为什么选我们/对方哪里好/决定性因素/差点输在哪/可复制什么；输单版加"换个定位能赢吗/这是模式吗"；多单聚合找赢输模式→定位调整→该追/不该追的交易→产品缺口。
7. **自治度分级与人类责任占比**：Autonomy-Evaluation-Framework 按级别写明人类在策略与执行上的责任百分比（L2：人 70-80% 策略/50-70% 执行；L4：人 10-20%/5-10%），可直接用作引擎的人机分工配置表。

## 3. 代表样本

### 3.1 年度营销计划（素材/github/AiCMO-Marketing-Prompt-Collection/01_CMO-&-Leadership/Strategy-&-Planning/AI-Powered-B2B-Annual-Marketing-Plan-Budget-Allocation-&-Board-Ready-Strategy-Intelligence-Engine.md，Quick 版节选）

```text
You are a CMO with 20 years of B2B SaaS experience … $10M to $200M ARR.
Company / Industry / Current ARR / Growth Target / headcount / budget /
Primary ICP / Top 3 competitors / Biggest pipeline challenge: [slots]
Produce: 1 Situation analysis (SWOT, competitive gaps)
2 Revenue model (MQL→SQL→Pipeline→ARR targets by quarter)
3 Budget allocation by channel with rationale (% and $)
4 Quarterly OKRs with leading and lagging indicators
5 Top 5 strategic bets with expected ROI
6 Headcount plan  7 MarTech priorities
8 Board summary narrative (where we are / where we're going / what we need)
Include specific dollar amounts, percentages, quarterly milestones throughout.
```

### 3.2 营销组合建模 MMM（素材/github/AiCMO-Marketing-Prompt-Collection/05_Analytics-&-Marketing-Operations/Advanced-Marketing-Intelligence/Marketing-Mix-Modeling-Framework.md，Quick 版节选）

```text
You are a marketing analytics consultant specializing in MMM.
My data: business type / monthly revenue (last 12 months) / monthly channel spend /
external factors (seasonality, promotions, launches, competitive events) / total budget.
Analyze: 1 decompose baseline vs incremental revenue per channel
2 ROAS and marginal ROAS  3 diminishing-returns thresholds per channel
4 optimized budget allocation at current total spend  5 channel synergy effects.
Deliver: contribution table, ROAS by channel, saturation analysis, revised budget
with projected revenue impact.
```

### 3.3 Churn 预防与扩展系统（素材/github/AiCMO-Marketing-Prompt-Collection/06_Customer-Success-&-Retention/Churn-Prevention-&-Expansion/Churn-Prevention-Expansion-Revenue-Engine.md，Quick 版节选）

```text
Context: ARR / accounts / ACV / current NRR / churn rate / CRM / product analytics tool.
Deliver: 1 CHURN RISK SCORING — 10 leading indicators (usage, engagement, support,
sentiment, payment, relationship), weighted formula implementable in a spreadsheet,
tiers Red(act 14d)/Yellow(30d)/Green(monthly), auto-escalation trigger events.
2 INTERVENTION PLAYBOOK per tier — owner, production-ready outreach sequence,
what to offer, escalation path if no response in X days.
3 EXPANSION — 8 upgrade-readiness signals, trigger logic "When [event], send
[message] with [offer]", 3-email expansion sequence.
4 QBR prep brief (data 48h before; Value Delivered→Adoption Gaps→Expansion Roadmap).
5 WIN-BACK at 30/60/90 days post-churn.
```

### 3.4 价值主张画布（素材/github/AiCMO-Marketing-Prompt-Collection/02_Product-Marketing/Positioning-&-Messaging/Value-Proposition-Canvas.md 节选）

```text
RIGHT SIDE: CUSTOMER PROFILE
Customer Jobs: Core / Related / Enabling / Emotional / Social jobs.
Job Statement: "When I [situation], I want to [motivation], so I [expected outcome]"
Job Context / Importance / Satisfaction with current solutions.
（左侧对应 Pains & Gains → Pain Relievers & Gain Creators 的匹配）
```

### 3.5 营销提示词工程包（素材/github/AiCMO-Marketing-Prompt-Collection/07_Hybrid-AI-Team-Management/Prompt-Engineering-Best-Practices/AI-Powered-Marketing-Prompt-Engineering-&-Agent-Instruction-Optimization-Intelligence-Engine.md，Quick 版节选）

```text
1 DIAGNOSE — 3 specific failure modes in typical prompts for this task
2 ENGINEER — production prompt with: ROLE (credentials + mental model) /
CONTEXT / OBJECTIVE (single measurable task) / CONSTRAINTS (≥5 explicit don'ts) /
OUTPUT SCHEMA (exact format & length) / QUALITY GATE (self-eval checklist)
3 TEST CASES — 3 diverse validation inputs
4 ITERATION LOG — template recording what changed and why between versions.
```

### 3.6 赢单输单复盘（素材/github/gtm-skills/competitive/README.md 节选）

```text
Analyze this competitive win/loss: Lost to/Won against [COMPETITOR], deal details.
1 Why did they choose us/them?  2 What did they like about the other side?
3 What was the deciding factor?  4 What almost lost us the deal?
5 What can we repeat / what should we change?  Is this a pattern?
聚合版：wins/losses 模式 → 定位调整 → 该追与不该追的交易 → 产品缺口。
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 年度/季度营销规划骨架

```text
你是 {{CMO/营销战略顾问}}（{{阶段经验锚}}）。
经营输入：ARR {{}} / 目标 {{}} / NRR {{}} / churn {{}} / ACV {{}} / 销售周期 {{}} /
各级转化率 {{MQL→SQL→Opp→Close}} / pipeline coverage {{}} / CAC {{}} / LTV:CAC {{}} /
预算 {{}} / 编制 {{}} / ICP {{}} / Top3 竞品与差异化 {{}} / 最大管道挑战 {{}}。
输出董事会级文档：1 情境与 SWOT 2 分季收入模型（各级转化目标）
3 分渠道预算（%+$+理由）4 季度 OKR（先行+滞后指标）5 Top{{5}} 战略押注+预期 ROI
6 编制与组织建议 7 MarTech 优先级 8 三段式董事会叙事（现状/去向/所需支持）。
所有建议给具体数字与里程碑；假设显式标注；数据不足处列"需补数据"而非编造。
```

### 4.2 营销科学分析骨架（MMM/归因/LTV:CAC 通用）

```text
你是营销科学顾问（{{MMM/因果推断/单位经济}}专长）。
数据：{{12-24 个月月度收入 + 分渠道花费 + 外部变量：季节/促销/发布/竞争/宏观}}。
分析：1 基线 vs 增量分解（绝对额+占比）2 各渠道 ROAS 与边际 ROAS
3 adstock 衰减与饱和阈值（花到多少开始低效）4 渠道协同效应
5 当前总预算下的最优再分配+预计收入影响。
输出：贡献表/曲线参数估计/再分配方案；**写明模型假设与置信局限，
数据不足以支撑的结论显式降级为假设**；给验证实验设计（geo holdout 等）。
```

### 4.3 客户健康与扩展运营骨架

```text
业务输入：{{ARR/账户数/ACV/NRR/churn/CRM 与产品分析工具}}。
1 风险评分：{{10}} 个先行指标（使用/互动/工单/情绪/付款/关系）+加权公式（可落地表格）
+红(14天)/黄(30天)/绿(月监控)三级+无视分数直接升红的触发事件。
2 分层干预：每层责任人/现成触达序列（主题+正文+电话要点）/给什么（成功回顾/培训/高管致电/商务补偿）/X 天无响应升级路径。
3 扩展：{{8}} 个升级就绪信号+"When {{event}}, send {{message}} with {{offer}}"触发逻辑+3 封扩展序列。
4 QBR 备料：会前 48h 拉数清单+叙事（已交付价值→采用缺口→扩展路线图）+5 个不推销的扩展探询问题。
5 赢回：churn 后 30/60/90 天节奏与话术。
```

### 4.4 定位与价值主张骨架（JTBD 画布式）

```text
产品 {{}}；目标客户段 {{}}；市场与竞争 {{}}；阶段 {{}}。
客户侧：功能性/情感性/社交性 Jobs（"When I {{情境}}, I want to {{动机}}, so I {{结果}}"）
+ Pains（严重度排序）+ Gains（期望程度排序）。
我方侧：Pain Relievers / Gain Creators 逐条对应；拟合度检查（哪些 job 没被覆盖）。
输出：一句话价值主张 / 三句版 / 30 秒电梯稿 / 按 {{Champion/用户/EB/技术}}  persona 的
语言变体（各自关心什么、用什么证明点格式、会提什么异议）。
```

### 4.5 竞争复盘与定位迭代骨架

```text
单笔：赢/输给 {{竞品}}，交易 {{规模/时间线}}，[NOTES]{{过程记录}}[/NOTES]。
五问：为何选此方/对方哪里好/决定性因素/差点翻车点/可复制点（输单加：换定位能赢吗）。
聚合：近 {{N}} 笔赢输 → 赢的模式/输的模式 → 定位调整 → 该追/不该追的交易画像 → 产品缺口清单。
```

### 4.6 AI 营销团队治理骨架（元层）

```text
团队 {{规模/职能/在用 AI 工具}}；目标自治度 {{L0-L5，参照人类责任占比表}}。
1 角色重设计：哪些任务给 agent（研究/初稿/分发/报表）、哪些留人（策略/判断/关系/危机）
2 agent 交接协议：输入输出 schema、升级条件、人类审批门
3 治理：品牌与合规红线、错误回滚、审计日志、按自治度设监控频率
4 提示词资产库：六段结构（ROLE/CONTEXT/OBJECTIVE/CONSTRAINTS≥5条/OUTPUT SCHEMA/QUALITY GATE）
+3 个测试输入+版本迭代日志（改了什么/为什么/指标变化）
5 度量：修订时间、一次通过率、人审占比；每 {{季度}} 复评自治级别。
```

### 4.7 电商运营轻量骨架（quickref FBA/Spreadsheet 升维）

```text
{{写产品描述：卖点+场景+规格，平台 {{Amazon/独立站}} 字符与合规约束}}；
{{供应商开发邮件：身份/量级/询问 MOQ·报价·交期，专业克制}}；
{{捆绑建议：基于 {{产品清单}} 给互补组合与理由}}；
{{把产品与定价信息整理成表格：列 {{SKU/成本/售价/毛利/库存}}}}；
{{申诉信改写：事实+整改+诉求，语气有据有节}}。
```

## 5. 差异与注意点

- **许可证**：AiCMO Apache-2.0、gtm MIT——可商用改编，保留声明；quickref 未声明，只吸收句式。
- ** hype 剥离**：篇名大量 "Intelligence Engine / Revenue Intelligence" 后缀与 "NRR>110%、300% ROI" 式承诺是包装话术；引擎应取其**组件结构**（评分/分层/触发/序列/指标），丢弃承诺数字。
- **伪代码装饰**：Advanced 版常见 Python class 伪代码与 ASCII 架构图，不可执行；组装时转为文字组件清单。
- **数据前提**：MMM/归因/预测类提示强依赖用户真实数据；无数据时模型会编造曲线参数——骨架 4.2 的"数据不足显式降级"条款必须保留。
- **目录重叠与规模误判**：05 两套 Analytics（60+553 篇）、06 两套（49+14 篇）主题大量重复；02 内 Positioning/Messaging、Pricing 多目录并存。按目录数估算素材量会严重高估独立内容。
- **厂商植入**：07 与 workflows 多处导向 Ruli 类平台或 AME 自家叙事（"We fired our marketing team"），引用时剥离。
- **自治度框架的用途**：Autonomy-Evaluation-Framework 是**评估量表**而非提示词，最适合做引擎的人机分工配置与验收标准（L2/L3 为当前现实上限），不要当成生成模板。
- **与 S 类边界**：PRD/产品功能设计素材本库 B 类未覆盖（AiCMO 是 product *marketing* 而非 product management），产品设计/PRD 二级暂空，见缺口。

## 6. 缺口

- **PRD/产品设计**：无需求文档、用户故事、优先级排序类提示词（分类体系 B/S 交叉项），需另收素材或用已有 product-management 技能对照。
- **定价**：02 有 Pricing-&-Packaging/Pricing-Strategy/Pricing-Testing 目录但本次未精读，定价弹性/分层设计骨架待补。
- **客服与工单运营**、**供应链/库存运营**（FBA 仅轻量指令）覆盖薄。
- **中文经营语境**：指标口径（GMV/复购/私域）、国内渠道（抖音/小红书/微信生态）的预算与归因素材缺失。
- **营销合规**：广告法/数据合规（cookieless 归因篇涉及隐私但无中国个保法视角）需与 L 类拼接。
- 05_Analytics-&-Performance 553 篇仅抽样，细分渠道分析（联盟/活动/内容 SEO 分析）骨架未展开，后续可按需增补。