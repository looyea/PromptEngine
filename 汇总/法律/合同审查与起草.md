# 合同审查与起草提示词

> 一级：L 法律 · 二级：法律合规/合同审查
> 素材：素材/github/legal-ai-architect-toolkit/（prompts/contract-review/ 三篇、prompts/drafting/clause-drafting.md、workflows/contract-review-workflow.md、frameworks/prompt-architecture-guide.md）
> 来源：OneC0de/legal-ai-architect-toolkit（MIT，作者 Donna Scaffidi，Ruli AI 法律 AI 负责人）

## 1. 概述

解决企业法务最高频任务： incoming 合同审查、对方 redline 分析、谈判立场标准化（playbook）、条款起草。素材虽只 4 篇核心提示词+1 条工作流+1 篇架构方法论，但完成度极高——每篇含 When to Use / The Prompt / Tips，且互相引用成链。核心主张："**Prompting is not the skill. Architecture is.**"（README）。三个流派：

- **Playbook 驱动派**：先为每类协议（SaaS MSA/NDA/供应商协议）生成谈判手册——每个关键条款给 7 字段：条款名及变体/我方首选立场/可接受替代/**不可接受底线（walk-away）**/谈判说辞（商业理由）/让步风险等级/偏离审批人。审查时把合同条款与 playbook 比对，输出 ALIGNED/DEVIATION/MISSING 三态。立场先行，审查才有基准。
- **结构化风险报告派**：逐节评估，7 字段（风险类别 Financial/Operational/Legal/Reputational/Regulatory × 等级 Critical/High/Medium/Low × 问题描述 × **引用原文** × 为何有问题 × 建议替换语言 × 不处理的商业后果），外加缺失条款/异常条款/歧义语言/难以履行的义务四类扫描，汇总给整体评级+签署前必须解决的 Top3+可接受风险+下一步。
- **双稿起草派**：条款起草一次给 Primary Draft（我方最强合理立场）+ Fallback Draft（可接受妥协）+ 逐短语注释（为何这样写/防什么/对方会在哪里反击），附常见坑、法域注意点、应配套的相关条款。风格要求现代清晰起草（避 whereas/hereby 等古语）、可执行性优先（不过度reach导致条款无效）。

工作流派系（contract-review-workflow）把上述串成 7 步链：分类提取→关键条款表→风险评估→playbook 比对→审查备忘录→**律师人工审批（non-negotiable）**→归档与偏离登记（反哺 playbook 更新）。

## 2. 共性结构与模式

1. **当事人立场必填**：所有提示都要求显式声明 Our role（vendor/customer/licensor/licensee）——"risk assessment changes dramatically based on which side you're on"。立场是第一槽位，缺省即错误。
2. **风险双维分类**：类别（财务/运营/法律/声誉/监管）× 等级（Critical/High/Medium/Low），等级绑定动作（Critical/High 必须先送资深律师，才能继续流程）。
3. **引用原文再评论**：风险评估要求 Quote the relevant provision；redline 分析要求忽略格式/编号/语法改动除非改变含义——**锚定文本、过滤噪声**是法律提示词的特有纪律。
4. **法律语言→商业语言双输出**：每项风险给 Plain-language business impact（"what this means"），备忘录面向业务团队而非律师炫技（memo 篇："The goal is to inform a decision, not demonstrate legal knowledge"）。
5. **上下文分层（架构指南）**：`[SYSTEM CONTEXT 角色与标准] + [PLAYBOOK CONTEXT 该类合同立场] + [MATTER CONTEXT 本单事实] + [TASK INSTRUCTION 当前动作]`。角色要具体到"中型 SaaS 公司的资深合同律师，中等风险偏好，优先保护 IP 与限制责任"，而非"你是律师"。
6. **质量门（Quality Gates）**：提取后自查"是否所有关键条款都已识别，列出可能遗漏的章节"；分析后"给每项评估打置信度，标记建议人审的区域"；输出前"检查内部一致性，确保建议与 playbook 立场对齐"。另有反馈整合式：把律师修正喂回，"更新分析并说明今后同类审查会怎么做不同"。
7. **三种链式架构模式**：Cascade（广扫→深挖高优先→行动计划）、Parallel（法律风险/商业条款/合规检查并行→综合）、Loop（起草→独立批评→修订，循环至质量阈值）。与 P 类工作流方法论同构，可直接被引擎复用。
8. **分级分块处理长文档**：Tips 反复强调复杂协议按章节拆开跑，而非整份塞入；高价值合同逐 major section 深析。

## 3. 代表样本

### 3.1 Redline 分析（素材/github/legal-ai-architect-toolkit/prompts/contract-review/redline-analysis.md，提示词全文要点）

```text
You are a senior contract attorney reviewing a redlined agreement. Provide:
1 Change Summary: what was changed, added, or deleted
2 Risk Assessment: rate each change Low / Medium / High
3 Business Impact: plain-language explanation
4 Recommended Response: Accept, reject, or counter-propose with suggested language
Focus on substantive changes. Ignore formatting, numbering, and minor grammatical
edits unless they change meaning.
For each high-risk change explain: why it's high risk / what the other party is
likely trying to achieve / your recommended negotiation position.
[Paste redlined text or describe changes here]
```

### 3.2 风险评估七字段（素材/github/legal-ai-architect-toolkit/prompts/contract-review/risk-assessment.md 节选）

```text
For each section evaluate:
1 Risk Category: Financial / Operational / Legal / Reputational / Regulatory
2 Risk Level: Critical / High / Medium / Low
3 Issue Description  4 Current Language: quote the relevant provision
5 Concern: why problematic  6 Recommended Fix: alternative language
7 Business Impact if Unaddressed
Also identify: missing clauses / unusual provisions / ambiguous language /
obligations difficult to comply with operationally.
Summary: overall rating; top 3 must-resolve before signing; acceptable risks; next steps.
Our role: [PARTY POSITION]  Our industry: [INDUSTRY]  Contract value: [TIER]
```

### 3.3 Playbook 七字段（素材/github/legal-ai-architect-toolkit/prompts/contract-review/playbook-generation.md 节选）

```text
Create a negotiation playbook for [AGREEMENT TYPE]. For each key clause:
1 Clause Name (standard + variations)  2 Our Preferred Position
3 Acceptable Alternatives  4 Walk-Away Position  5 Negotiation Talking Points
(business justification)  6 Risk Level if Conceded  7 Approval Required for deviation.
Cover at minimum: limitation of liability / indemnification / IP ownership /
confidentiality / term & termination / data protection / warranties /
governing law & dispute resolution.
Format as a matrix a junior attorney could use independently.
Context: [COMPANY, INDUSTRY, RISK TOLERANCE]
```

### 3.4 条款双稿起草（素材/github/legal-ai-architect-toolkit/prompts/drafting/clause-drafting.md 节选）

```text
You are a senior contracts attorney drafting a [CLAUSE TYPE] for a [AGREEMENT TYPE].
Context: our role / industry / key concern / negotiation leverage [Strong|Moderate|Weak].
1 Primary Draft: strongest reasonable language in our favor
2 Fallback Draft: moderate compromise position
3 Annotations per key phrase: why chosen / what it protects against /
where the other side will push back.
Style: clear modern drafting; avoid archaic legalese unless industry-standard;
bracket defined terms; keep provisions enforceable — avoid overreach.
Also: common pitfalls / jurisdiction considerations / related clauses to accompany.
```

### 3.5 工作流第 4 步：playbook 比对（素材/github/legal-ai-architect-toolkit/workflows/contract-review-workflow.md）

```text
Compare contract terms against our standard playbook positions.
OUR PLAYBOOK POSITIONS: [paste]   CONTRACT TERMS: [from Step 2 extraction]
For each term indicate:
- ALIGNED: matches preferred or acceptable position
- DEVIATION: differs — describe how and recommend response
- MISSING: expected clause absent — recommend whether to add
Prioritize deviations by business impact.
```

### 3.6 上下文分层与质量门（素材/github/legal-ai-architect-toolkit/frameworks/prompt-architecture-guide.md 节选）

```text
[SYSTEM CONTEXT: Your role and standards] + [PLAYBOOK CONTEXT: positions for
this contract type] + [MATTER CONTEXT: facts of this deal] + [TASK INSTRUCTION]
Quality gates: after extraction — "List any sections that may contain terms not
captured"; after analysis — "Rate your confidence in each assessment. Flag areas
for human review"; before output — "Ensure recommendations align with the playbook."
Patterns: Cascade (scan→deep dive→action) / Parallel (risk+commercial+compliance
→synthesis) / Loop (draft→critique→revise until threshold).
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 合同审查链骨架（7 步，可整体编排或单步调用）

```text
[Step1 分类] 判定协议类型（MSA/NDA/SOW…）、当事人与角色、生效日与期限、管辖法、
估值、异常结构特征。→ 按类型路由到对应 playbook 与审查人。
[Step2 提取] 关键条款表：| 条款 | 所在章节 | 摘要 | 标准/非标准 |，覆盖付款、责任上限、
赔偿、IP、终止、竞业/挖角、数据处理、保险、保证及异常条款。
[Step3 风险评估] 用 4.3 骨架；Critical/High → 停链送资深律师。
[Step4 比对] 用 3.5 式 ALIGNED/DEVIATION/MISSING，按商业影响排序。
[Step5 备忘录] 执行摘要≤3句；建议动作（批准/附条件批准/拒绝）；必须修改项；
建议修改项；已接受风险及理由；下一步与时间线。
[Step6 人审] 律师核对：评级准确/偏离识别正确/立场适合本单/无遗漏/备忘录忠实。
[Step7 归档] 分析与合同同存；登记获批偏离 → 反哺 playbook 季度更新。
每步之间传递结构化产物（分类卡/条款表/风险表/比对表），不传自由文本。
```

### 4.2 Playbook 生成骨架

```text
为 {{协议类型}} 生成谈判手册。公司背景：{{行业/规模/风险偏好/商业底线}}。
对每个关键条款（至少：责任限制/赔偿/IP/保密/期限与终止/数据保护/保证/管辖与争议解决）：
条款名及常见变体 / 首选立场（理想语言）/ 可接受替代 / 不可接受底线 /
谈判说辞（商业理由）/ 让步风险（低中高）/ 偏离审批人。
输出初级律师可独立使用的矩阵；标注需资深律师定稿的条款；设 {{季度}} 复审。
```

### 4.3 风险评估骨架（单份或分节）

```text
你是 {{资深公司/合同律师}}，我方立场 {{customer/vendor/licensee…}}，行业 {{}}，合同价值 {{}}。
逐节输出七字段：风险类别（财务/运营/法律/声誉/监管）/等级（Critical-高-中-低）/
问题描述/**原文引用**/为何有问题/建议替换语言/不处理的商业后果。
另扫：缺失条款/异常条款/对我方不利的歧义/运营上难以履行的义务。
汇总：整体评级 + 签署前必须解决 Top3 + 可接受风险 + 下一步。
每项给置信度；低置信与 Critical/High 标记"需人审"。长文档按节分块运行。
```

### 4.4 Redline 分析骨架

```text
我方立场 {{}}；适用 playbook {{相关条款摘录}}。
分析以下修订：[REDLINE]{{修订文本或变更描述}}[/REDLINE]
只关注实质变更（格式/编号/语法除非改变含义一律忽略）。
每项变更：改了什么 / 风险等级 / 商业含义（白话）/ 建议回应（接受-拒绝-反提案+建议语言）。
高风险项加：为何高风险 / 对方意图推测 / 我方谈判立场。
```

### 4.5 条款起草骨架（双稿+注释）

```text
起草 {{条款类型}} 用于 {{协议类型}}。我方 {{角色}}；行业 {{}}；核心关切 {{防什么}}；
谈判筹码 {{强/中/弱}}；被替换的现有语言 {{如有}}。
输出：1 主稿（我方最强合理立场）2 退让稿（可接受妥协）
3 逐关键短语注释（选词理由/防护对象/对方反击点）。
风格：现代清晰起草，避古语；定义术语用括号占位；确保可执行、不过度reach。
附：该类条款常见坑 / {{法域}} 特殊考虑 / 应配套条款。
```

### 4.6 审查质量门骨架（可挂在任何法律分析输出后）

```text
自检：1 关键条款是否全部识别？列出可能遗漏的章节。
2 每项评估的置信度（高中低），低置信项标记人审。
3 建议是否与提供的 playbook 立场一致？矛盾处显式列出。
4 风险等级与建议动作是否匹配（Critical 必须升级）？
（反馈式：律师修正如下 {{corrections}}，更新分析并说明今后同类任务的改进。）
```

## 5. 差异与注意点

### 5.1 边界条款归纳（法律类必备，引擎应默认注入）

素材中反复出现、可归纳为六条标准边界条款：

1. **非法律意见**：AI 输出是审查辅助与研究起点，不构成法律意见（"Use this as a starting point for deeper research, not as final legal advice"——multi-jurisdiction 篇 Tips；contract-review 工作流 Step6："AI assists the review — it doesn't replace attorney judgment. This step is non-negotiable."）。
2. **执业律师终审**：所有 AI 起草语言用于真实协议前必须律师审查（clause-drafting Tips："Have counsel review all AI-drafted language before use in live agreements"）；playbook 分发前须资深律师定稿。
3. **引用独立核实**：AI 能识别正确框架但可能没有最新法条文本，引用必须独立验证（"Always verify citations independently"）。
4. **特权与保密**：向任何 AI 工具输入事实前考虑 attorney-client privilege 与保密义务（memo-writing Tips："be mindful of privilege considerations before inputting facts into any AI tool"）；引擎应默认提示脱敏/最小化输入。
5. **法域限定与升级路径**：法域特定问题须咨询当地执业律师；高风险项强制升级资深律师/外部 counsel，流程中写明 escalation path。
6. **立场与事实依赖**：输出质量取决于显式提供的当事人立场、行业、合同价值与事实；缺省这些槽位时引擎应拒绝生成结论性建议。

### 5.2 其他注意点

- **许可证**：MIT，"use freely, attribute kindly"——商用改编低风险，保留作者署名。
- **厂商植入**：作者供职 Ruli AI，架构指南 Layer3、workflows 多处导向其产品（Monitor/Assistant/CLM 集成）；引用时剥离产品推荐，保留架构思想。
- **法系局限**：全部素材为英文、普通法/美国企业法语境（circuit splits、UCC 式条款清单）；中国法下条款效力判断（如违约金调整、格式条款规则）不能直接套用，骨架可用、实体立场必须本地化。
- **架构指南跨篇复用**：三层模型（单提示→提示系统→带记忆/反馈/集成的架构）与 Cascade/Parallel/Loop 同样适用于 法律研究与合规.md 及 P 类方法论，引擎应抽为公共组件。
- **指标意识**：工作流附运营指标（标准合同 intake→memo <24h、律师时间降 40-60%、playbook 依从 90%+）——可作为引擎验收目标槽，但属作者目标值非实证。

## 6. 缺口

- **中文合同素材**：无中文合同范本审查、中国法条款库（民法典合同编/劳动法/个保法数据处理协议）。
- **专项协议深度**：NDA/SaaS MSA 仅在 playbook 条款清单中提及，无逐类协议的专用审查提示词。
- **谈判执行**：审查产出谈判立场后，无实际谈判对话/让步交换提示词（可与 B 类销售谈判素材拼接）。
- **批量与迁移**：工作流提到 bulk review/migration 场景但无专用提示词；无合同比对（版本 diff）工具化 schema。
- **签署与履约**：无电子签流程、履约义务跟踪、续约提醒类提示词。
- 法律检索（判例/法条原文获取）与合规监控见同目录 法律研究与合规.md。