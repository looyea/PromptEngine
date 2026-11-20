# 法律研究与合规提示词（多法域 / 法规监控 / 法条对比 / 备忘录 / 政策 / 法务运营）

> 一级：L 法律 · 二级：法律合规/合同审查（研究与合规侧）
> 素材：素材/github/legal-ai-architect-toolkit/（prompts/legal-research/ 三篇、prompts/drafting/memo-writing.md、prompts/drafting/policy-drafting.md、prompts/legal-ops/ 三篇、workflows/regulatory-monitor-setup.md、workflows/knowledge-base-architecture.md、frameworks/prompt-architecture-guide.md）
> 来源：OneC0de/legal-ai-architect-toolkit（MIT，作者 Donna Scaffidi）

## 1. 概述

解决"法律知识工作"的四类问题：跨法域比较研究、法规变化监控与分诊、法条版本对比、以及研究成果的交付形态（法律备忘录/公司政策）与法务团队运营系统（知识库/团队 playbook/流程自动化）。与合同审查篇共享同一套架构方法论（上下文分层、质量门、Cascade/Parallel/Loop）。五个流派：

- **比较矩阵派**：多法域分析按 7 维度逐法域展开（管辖法/关键要求/关键术语定义差异/罚则与执法/例外与安全港/近 12 个月修法/日常运营影响），再输出并排矩阵：哪里一致/哪里冲突/**最严法域/最宽法域**，最后给跨法域统一合规建议。法条对比同构：逐节增删改→实质变更（新旧语言并排+实务含义+影响谁+义务扩张还是收窄）→技术性改动单列→**未变条款同样重要**→整体方向（更严/更宽/中性）→歧义与待解释问题。
- **监控分诊派**：每条法规动态 6 字段（改了什么/生效日或状态/影响哪些团队产品运营/法务要做什么及期限/忽视的风险/来源链接），按紧迫度三档分诊：<30 天立即行动、30-90 天需规划、90+ 天仅知悉。配套工作流给三级成熟度：L1 手动跑提示词（周 15-20 分钟）→L2 结构化追踪表+分发列表+法规日历→L3 自动化监控平台（触发条件：10+ 主题×5+ 法域、手动>2h/周、曾漏掉有业务影响的变更、需审计痕迹）。
- **备忘录派（IRAC 变体）**：受众校准先行（读者是律师/高管/混合；用途是决策/合规/风险知悉/董事会），结构：执行摘要 2-3 句给结论与建议动作→Question Presented（精确框定法律问题）→Brief Answer（先答后析）→Background（用户提供事实，AI 只组织）→Analysis（适用法+涵摄+概率×影响的风险评级+**反方论点**）→Recommendations（带 owner 与时限）→Open Items。
- **政策起草派**：10 段固定结构（目的/范围/定义/政策声明/角色职责/程序/例外审批/违规后果/关联政策/复审周期），风格硬约束：**8 年级阅读水平**、祈使句编号（"Employees must…" 而非 "It is recommended…"）、强制要求与最佳实践区分、顶部放速查摘要、标记需法务重点审查处。
- **法务运营派**：知识库架构（分类法/内容类型模板与元数据/治理与退役标准/权限与密级/**AI Readiness**——内容如何结构化才能被 AI 检索与准确应用/迁移计划）；团队 playbook（intake 清单/分步流程含决策分支/标准立场与审批阈值/典型与边缘场景/质检清单）；流程自动化（触发→分诊路由→处理步骤含人机分工与 SLA→审批门与升级→交付归档→指标，输出叙事+ASCII 决策树+自动化机会清单）。

## 2. 共性结构与模式

1. **来源与核实纪律**：监控 6 字段必含 Source；多法域 Tips 明示"AI 能找对框架但法条文本可能滞后，引用必须独立核实"；工作流要求关键发现对照 primary sources 验证。**法律研究提示词把可验证性写进输出 schema**。
2. **不确定性显式化**：要求标记 law unsettled / circuit splits / 近期执法显示解释变化之处；法条对比要求列 Open Questions（新语言歧义、需指引处）；备忘录要求 Open Items。与数据类"拒答条款"同源——给模型合法的"不知道/待确认"出口。
3. **受众校准**：备忘录按读者法律 sophistication 与用途调整；政策按全员可读标准写；多法域矩阵给业务看"Practical Impact"。**同一研究成果按受众出不同交付物**是贯穿模式。
4. **行动导向**：每条监控项要有 owner+deadline（"monitoring without action is just reading the news"）；备忘录建议带 owner 与时限；风险评级用概率×影响；分诊档位直接绑定动作。
5. **范围纪律**：法域必须具体列举（"US states 太宽"）；监控"everything is monitoring nothing"；季度复审监控范围；长法条逐节对比。
6. **人机分工与成熟度分级**：运营类提示统一要求标注每步 human or automated、审批阈值、SLA；监控工作流三级成熟度与内容自治度框架（AiCMO）同构，可作引擎配置维度。
7. **知识沉淀闭环**：playbook 每处理 10-20 单更新；知识库记录 post-matter learnings；监控输出建追踪表；偏离登记反哺标准立场——与合同审查链 Step7 同一思想。

## 3. 代表样本

### 3.1 多法域分析（素材/github/legal-ai-architect-toolkit/prompts/legal-research/multi-jurisdiction-analysis.md 节选）

```text
You are a legal research analyst. Compare [LEGAL TOPIC] across [LIST JURISDICTIONS].
Per jurisdiction: 1 Governing Law  2 Key Requirements  3 Definitions (note differences)
4 Penalties/Enforcement  5 Notable Exceptions (carve-outs, safe harbors)
6 Recent Changes (last 12 months)  7 Practical Impact on daily operations.
Comparison Matrix: where aligned / where conflict / most restrictive / most permissive.
Recommendations: approach for a company operating across all listed jurisdictions;
where local counsel should be consulted.
Important: flag unsettled law, circuit splits, changing enforcement interpretations.
```

### 3.2 法规监控与分诊（素材/github/legal-ai-architect-toolkit/prompts/legal-research/regulatory-monitoring.md，提示词主体）

```text
You are a regulatory analyst monitoring developments for [INDUSTRY/COMPANY TYPE].
Summarize latest developments in [TOPIC AREA] across [JURISDICTIONS]. Per item:
1 What changed  2 Effective date (or pending status)  3 Who it affects (teams/products)
4 Action required and by when  5 Risk if ignored  6 Source (link or citation).
Prioritize by urgency: Immediate (<30 days) / Planning (30-90 days) / Awareness (90+).
```

### 3.3 法条版本对比（素材/github/legal-ai-architect-toolkit/prompts/legal-research/statute-comparison.md 节选）

```text
Compare versions of [STATUTE]. 1 Section-by-section: added / removed / modified.
2 Substantive changes: old vs new side by side / meaning in practice / who affected /
expands or narrows obligations. 3 Technical/housekeeping changes noted separately.
4 Impact: overall direction (more restrictive/permissive/neutral); key provisions
NOT changed (sometimes what doesn't change is as important); compliance implications.
5 Open Questions: ambiguous new language, guidance needed.
```

### 3.4 法律备忘录（素材/github/legal-ai-architect-toolkit/prompts/drafting/memo-writing.md 节选）

```text
Memo Parameters: To / From / Re / Date.
Audience calibration: reader sophistication [Attorney|Executive|Mixed];
purpose [Decision-making|Compliance|Risk awareness|Board presentation].
Structure: 1 Executive Summary (2-3 sentences, bottom line + recommended action)
2 Question Presented  3 Brief Answer (before analysis)  4 Background (facts I provide)
5 Analysis: applicable law → application to facts → risk (probability × impact)
→ counterarguments  6 Recommendations (owners + timelines)  7 Open Items.
Tone: inform a decision, not demonstrate legal knowledge.
```

### 3.5 政策起草风格条款（素材/github/legal-ai-architect-toolkit/prompts/drafting/policy-drafting.md 节选）

```text
Structure: Purpose / Scope / Definitions / Policy Statement / Roles & Responsibilities
/ Procedures / Exceptions (how to request, who approves) / Enforcement /
Related Policies / Review Schedule.
Style: 8th-grade reading level; imperative numbered requirements ("Employees must…"
not "It is recommended that…"); distinguish mandatory vs best practice;
quick-reference summary at top; flag areas needing special legal review.
Params: applicable regulations [GDPR/SOX/HIPAA…]; audience; approval level; replaces.
```

### 3.6 监控运营化（素材/github/legal-ai-architect-toolkit/workflows/regulatory-monitor-setup.md 节选）

```text
Level 2 tracking spreadsheet columns:
Date | Jurisdiction | Regulatory Body | Topic | Summary | Impact | Action Required |
Deadline | Owner | Status
Weekly: run prompts per topic → update sheet → digest to distribution lists →
flag urgent. Monthly: status review → update regulatory calendar → brief leadership.
Upgrade to platform when: 10+ topics × 5+ jurisdictions, >2h/week manual,
missed a change with business impact, or audit trails needed.
Tips: assign owners; set deadlines even for awareness items; review scope quarterly.
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 多法域比较骨架

```text
你是法律研究分析师。议题 {{}}；法域 {{逐一列举，不用"各国/各省"}}。
逐法域 7 维：管辖法（法条/法规/判例原则）/关键要求与禁止/关键术语定义（标注差异）/
罚则与执法实践/例外·安全港·豁免/近 12 个月修法与拟议立法/对日常运营的实务影响。
输出并排矩阵：一致处/冲突处/最严法域/最宽法域。
建议：跨全部法域经营的统一方案（通常向最严看齐的取舍）+ 必须咨询当地律师的事项。
**标记法律未定处、裁判分歧、执法口径变化；每项给来源；注明知识截止局限。**
```

### 4.2 法规监控骨架（可周期调度）

```text
监控范围：行业 {{}}；法域 {{}}；监管机构 {{}}；主题 {{数据隐私/劳动/证券/广告…}}。
检索并汇总 {{周期}} 内动态，每条 6 字段：改了什么/生效日或状态/影响哪些团队产品运营/
法务需做什么及期限/忽视的风险/来源链接。
分诊：立即行动(<30天) / 需规划(30-90天) / 仅知悉(90+)。
输出追加：追踪表行（Date|Jurisdiction|Body|Topic|Summary|Impact|Action|Deadline|Owner|Status）；
与上期相比的新增/状态变化项。无可靠来源的条目显式标注"待核实"，不得混入确定结论。
```

### 4.3 法条/版本对比骨架

```text
对比 {{法规名}} 的 {{现行版}} 与 {{修订/拟议版}}（明确标注哪个是哪个）。
1 逐节：新增/删除/修改清单 2 实质变更：新旧语言并排+实务含义+影响谁+义务扩张或收窄
3 技术性改动（重编号/交叉引用）单列不混入 4 影响分析：整体方向（更严/更宽/中性）+
**关键未变条款**+合规影响 5 开放问题：歧义语言与待出台指引。
长法条按节分块；输出供业务干系人简报的摘要版（白话，≤{{N}} 字）。
```

### 4.4 法律备忘录骨架

```text
To {{收件人及角色}} / From {{}} / Re {{具体议题}} / Date {{}}。
读者 {{律师/高管/混合}}；用途 {{决策/合规/风险知悉/董事会}}。
事实与材料：[FACTS]{{用户提供，AI 不得自行"研究"补充事实}}[/FACTS]
结构：执行摘要（2-3 句结论+建议动作）→ Question Presented → Brief Answer →
Background（组织所给事实）→ Analysis（适用法框架→涵摄→风险=概率×影响→反方论点）→
Recommendations（每条带 owner+时限）→ Open Items（仍需查明事项）。
语气：专业但可读，服务决策而非展示学识。引用须可核实；不确定处显式标注。
```

### 4.5 公司政策起草骨架

```text
政策类型 {{AI 使用规范/数据隐私/行为准则…}}；公司 {{行业/规模/地域}}；
适用法规 {{GDPR/SOX/HIPAA/个保法…}}；约束对象 {{全员/部门/承包商}}；
审批层级 {{董事会/C-suite/部门}}；替代 {{旧政策或新建}}。
10 段：目的(2-3句)/范围/定义/政策声明（祈使句编号，强制与最佳实践分开）/
角色职责/合规程序(分步)/例外申请与审批/违规后果/关联政策/复审周期({{年度}})。
风格：{{8 年级}} 阅读水平；顶部速查摘要；适当举例；标记需法务重点审查处。
附：一页员工培训版；对标 {{行业标准}} 的差距说明。
```

### 4.6 法务运营系统骨架（知识库/playbook/自动化三选一或组合）

```text
[知识库] 团队 {{规模/领域}}；现状 {{知识在哪/最大找不到的缺口}}。
设计：分类法+标签+命名规范；内容类型（先例模板/操作指南/立场 playbook/业务 FAQ/
法规摘要/结案复盘）各给模板与元数据；治理（owner/复审周期/质量标准/退役规则）；
权限与密级；**AI Readiness（结构化与元数据要求，便于检索与准确应用）**；迁移优先级与计划。
[团队 playbook] 事项类型 {{}}；频率 {{}}；现状 {{}}。
含：适用与不适用边界/intake 清单与红旗升级/分步流程（执行人/时限/分支/模板）/
标准立场与偏离审批阈值/3-5 典型场景+边缘案例/质检清单/资源与专家。
验收：不熟悉流程的人可照做；每 {{10-20}} 单后更新。
[流程自动化] 流程 {{}}；量 {{}}；现耗时 {{}}；痛点 {{}}；现有工具 {{}}。
设计：触发→intake 分诊与自动路由规则→处理步骤（每步 human/automated+分支判据+SLA）
→审批门（阈值+审批矩阵+升级路径）→交付与归档→指标（周期/瓶颈/质量/量趋势）。
输出：叙事+文本决策树+自动化机会 vs 必须人工清单；目标态 {{如"X 以下 NDA 24h 免律师"}}。
```

### 4.7 研究质量门骨架（与合同审查篇 4.6 共用）

```text
自检：1 每个结论是否有可核实来源？无来源项标"待核实"。
2 法域/时效是否越界？知识截止后的变化显式声明不确定。
3 是否标记了法律未定/裁判分歧/执法口径变化？
4 建议是否区分了"统一合规方案"与"须当地律师确认"事项？
5 受众适配：交付物语言是否匹配读者（律师/高管/全员）？
```

## 5. 差异与注意点

### 5.1 边界条款归纳（法律类必备，引擎应默认注入）

1. **非法律意见**：研究输出是起点不是终局（"Use this as a starting point for deeper research, not as final legal advice"）；多法域建议中必须保留"哪些事项需法域-specific 执业律师意见"清单。
2. **引用独立核实**：AI 可能给出过时或虚构的法条文本与判例——所有 citation 须对照 primary sources 验证后方可对外使用；监控工作流把"verify critical findings against primary sources"列为每周固定步骤。
3. **知识时效声明**：模型知识滞后于实时法规变化（工作流 L1 局限明示）；监控类任务应接检索工具或要求用户供源，纯参数化记忆不得作为"最新法规"依据。
4. **特权与保密**：输入事实前评估 privilege 与保密义务（memo Tips）；敏感事项脱敏或改用合规部署的工具。
5. **人工责任不可让渡**：政策与备忘录发布前须法务/相应审批层级签核；自动化流程必须保留审批门与升级路径（"what requires human review and at what threshold"）。
6. **范围与不确定性诚实**：法域列举要具体；法律未定处、歧义处、待核实项必须显式输出，不允许以確定语气掩盖不确定性。

### 5.2 其他注意点

- **许可证**：MIT，署名即可商用改编；作者 Ruli AI 背景带来的产品导向（Monitor 平台、知识库工具推荐）应剥离，保留三级成熟度思想。
- **监控提示词的幻觉高危性**：regulatory-monitoring 若在无联网/无用户供源情况下运行，极易编造"最新法规"。引擎组装时必须二选一：接检索工具，或改为"基于用户提供的法规文本做分析"模式，并保留"待核实"输出位。
- **法系与语言**：素材为美国/普通法语境（circuit splits、SOX/HIPAA 示例）；骨架可迁移，实体法内容须替换为中国法（民法典/个保法/数据安全法/广告法/劳动法）并重建来源核实渠道。
- **与合同审查篇的分工**：架构指南、质量门、上下文分层两篇共用，引擎应抽公共组件；审查链的 Step7 归档反哺与本篇三闭环（playbook 更新/知识库复盘/监控追踪表）是同一模式。
- **frameworks 其余篇**（legal-ai-maturity-model、build-vs-buy、ai-adoption-playbook）与 workflows/knowledge-base-architecture、team-onboarding-ai 本次未逐字精读，属战略/落地指南而非提示词正文，引用时以已精读篇为准。

## 6. 缺口

- **中国法规监控源**：无国家网信办/市监总局/人大法工委等来源清单与检索策略素材；中文法规版本对比（征求意见稿→正式稿）无实例。
- **判例检索**：素材全部面向制定法，无判例法检索/类案比对提示词（普通法素材也仅提及 circuit splits 概念）。
- **合规审查清单化**：无面向业务动作的事前合规检查（营销广告极限词/数据出境/劳动用工）checklist 提示词——B 类营销合规缺口需由此补。
- **法律翻译与双语交付**：无中英法律术语对照/翻译保真提示词（可借 O 类翻译骨架）。
- **诉讼与争议**：无证据整理/时间线构建/诉状起草素材，本库 L 类目前只覆盖非诉。
- **量化评估**：监控/研究质量无评测集与准确率基准素材，引擎需自建抽检机制（对应 5.1 核实条款的自动化）。