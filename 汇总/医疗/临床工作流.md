# 医疗临床工作流提示词

> 一级类：H 医疗 · 二级：工作流
> 素材：`素材/github/prompts-for-health/`（MIT，12 个临床工作流 + README 五原则，已全部通读）
> 用途：引擎组装医疗类提示词的工作流骨架与合规原料。**本类全部输出为非诊断建议，须经执业医师/合格专业人员人工审核后方可使用**；引擎第 5 步必须注入 §4.0 合规前缀与 §5 合规条款。

## 1. 概述

prompts-for-health 是面向美国临床场景的提示词集：12 个工作流覆盖医生文书、临床决策支持、患者沟通、行政事务四簇，每个文件都附使用警告与"Requires PHI/PII: YES"标注（全部 12 个都需要患者数据）。素材的价值不在单条 prompt，而在三件可迁移的东西：

1. **缺信息协议**：先列缺失项 → 逐条提问 → 等待回答 → 再生成（问诊框架/试验资格两个 prompt 的核心机制）；
2. **幻觉隔离规则**：模型补写的信息一律**加粗**标出（预授权信）、抽取缺失一律 `<unknown>`（表单抽取），让人审一眼定位机器编造内容；
3. **受众适配矩阵**：同一医学内容按阅读水平×文化背景×年龄改写成多版本（跨文化解释 prompt 的菲律宾移民家长/8 岁儿童双版本）。

README 五原则：人类监督（AI 辅助不替代临床判断）、透明（告知 AI 参与）、循证（输出需对照证据核验）、隐私（HIPAA 合规环境处理 PHI）、守护患者交互（AI 不直接面向患者）。

## 2. 共性结构与模式

### 2.1 十二工作流聚类

| 簇 | 工作流（文件名） | 输入 → 输出 |
|---|---|---|
| 文书 | SOAP-Notes-from-Clinical-Notes / SOAP-Notes-from-Transcript | 病历文本/问诊转录 → 按问题分条 SOAP，括号附 ICD-10/CPT |
| 文书 | Discharge-Summary-from-Notes | 住院病历 → 初中阅读水平出院小结（居家护理步骤+警示症状+何时就医） |
| 文书 | Extract-Information-from-Forms | HL7 等表单 → 有序字段抽取，缺失填 `<unknown>`，忽略账单段 |
| 决策支持 | Brainstorm-Differential-Diagnoses | 病例 → 鉴别诊断清单+下一步检查/影像/转诊建议 |
| 决策支持 | Determine-Patient-Eligibility-Clinical-Trial | 病例+试验标准 → 逐条 meets/does not/potentially meets+缺失信息问询 |
| 决策支持 | Generate-Question-Framework-for-Patient-Followup | 背景清单 → 结构化随访问题框架（ask-and-wait） |
| 患者沟通 | Draft-Replies-to-Patient-Messages | 患者消息 → 护士口吻回复草稿（署名 Melissa Acosta RN，代表 3 位医生） |
| 患者沟通 | Explaining-Health-Concepts-Across-Cultures-Age-Education-Levels | 医学概念+受众画像 → 两版 400-500 词通俗解释 |
| 患者沟通 | Locate-Healthcare-Services | 地点+需求 → ≤40 英里 5 个低价检验选项（滑动收费；**源文件自注高幻觉倾向**） |
| 行政 | Generate-Prior-Authorization-Letter | 病例+拒赔理由 → 说服性预授权信，编造信息加粗 |
| 行政 | Anonymize-Text | 含 PHI 文本 → 18 类标识符替换为 [Patient Name] 式占位+删除清单，保留临床语境 |

### 2.2 通用结构五件套

```
角色声明（expert American physician, specializing in {{专科}} / 护士名+资质）
→ 任务与输入（病历/转录/标准原文，明确"只依据给定材料"）
→ 输出格式硬约束（分条/字段顺序/词数/阅读水平）
→ 幻觉隔离（编造加粗 / 缺失 <unknown> / 占位符 [X]）
→ 边界（缺信息先问再等；不替代临床判断；输出供人审）
```

### 2.3 缺信息协议（ask-and-wait）

试验资格与随访框架共用：①按优先级列出缺失信息清单；②一次问一条或一组；③**明确 WAIT——收到回答前不得继续生成**；④收齐后输出汇总报告。这是把"多轮采集"写进 prompt 的可复用机制，引擎 Loop 模式可直接套用。

### 2.4 幻觉隔离三法

- **加粗法**（预授权信）：允许模型补写合理内容，但所有非输入来源的信息必须 **bold**，人审只需扫粗体。
- **哨兵值法**（表单抽取）：缺失字段一律 `<unknown>`，禁止推断填充；字段顺序固定（Name/Gender/DOB/Medication List/Problems）。
- **占位符法**（去标识化）：18 类 HIPAA 标识符替换为 `[Patient Name]`、`[Date]` 式占位，附"已删除项清单"供核对，同时要求保留临床语境不损伤医学含义。

### 2.5 受众适配与语气控制

- 出院小结：ESL 初中阅读水平、白话术语、分步居家护理、警示症状+何时就医、鼓励性收尾。
- 跨文化解释：受众三元组（阅读水平×文化×年龄）各出一版——菲律宾移民家长（8 年级阅读水平、对西医存疑）版需先共情文化顾虑；8 岁儿童版用类比与简单句。
- 患者消息回复：固定护士人设+友善建立关系+性别中性代词，草稿署名真人护士（由真人审核后发出，符合原则 5）。

## 3. 代表样本

**样本 1：鉴别诊断（角色+清单+下一步）**（`素材/github/prompts-for-health/Brainstorm-Differential-Diagnoses.MD`，归纳改写）
```text
You are an expert American physician, specializing in {{专科}}.
Given the case below, list differential diagnoses (most→least likely),
each with supporting/contradicting findings from the case.
Then recommend next steps: labs, imaging, referrals.
Include relevant social history in your reasoning.
Case: {{病例}}
（输出仅供执业医师参考，不构成诊断。）
```

**样本 2：试验资格逐条判定 + ask-and-wait**（`素材/github/prompts-for-health/Determine-Patient-Eligibility-Clinical-Trial.MD`，归纳改写）
```text
Act as a clinical trial administrator speaking peer-to-peer with a physician.
For EACH eligibility criterion, state: meets / does not meet / potentially meets,
with the case evidence. List missing information in priority order,
ask for it, and WAIT for answers before continuing.
After all answers: produce a summary eligibility report.
Criteria: {{试验标准}}  Case: {{病例}}
```

**样本 3：去标识化（18 标识符 + 删除清单）**（`素材/github/prompts-for-health/Anonymize-Text.MD`，归纳改写）
```text
Replace all 18 HIPAA identifiers (names, dates, phones, emails, SSN,
MRN, addresses, URLs, IPs, device IDs, biometrics, photos, ...) with
placeholders like [Patient Name], [Date]. Preserve clinical meaning.
Output: (1) anonymized text (2) list of removed/replaced items.
Text: {{含 PHI 文本}}
（注意：仍须在 HIPAA 合规服务内运行，见 §5。）
```

**样本 4：预授权信 + 编造加粗**（`素材/github/prompts-for-health/Generate-Prior-Authorization-Letter.MD`，归纳改写）
```text
Write a persuasive prior-authorization letter to {{保险方}} for {{治疗}},
citing medical necessity from the case and addressing the denial reason.
Any information you add that is NOT in the provided case must be in **BOLD**
so the clinician can verify or replace it before sending.
Case: {{病例}}  Denial reason: {{拒赔理由}}
```

**样本 5：跨受众双版本解释**（`素材/github/prompts-for-health/Explaining-Health-Concepts-Across-Cultures-Age-Education-Levels.MD`，归纳改写）
```text
Explain {{医学概念}} in two versions, 400-500 words each:
A) Filipino immigrant parent, 8th-grade reading level, skeptical of
   Western medicine — acknowledge cultural concerns first, build trust.
B) 8-year-old child — simple sentences, analogies, reassuring tone.
```

**样本 6：转录 → 分问题 SOAP**（`素材/github/prompts-for-health/SOAP-Notes-from-Transcript.MD`，归纳改写）
```text
From the visit transcript, write bullet-format SOAP notes, one set per
problem addressed. Detailed Subjective from patient's own words;
Objective from exam findings mentioned; Assessment with (ICD-10 code);
Plan per assessment with (CPT code) where applicable.
Transcript: {{问诊转录}}
（编码需人工编码审核，见 §5。）
```

## 4. 可复用模板骨架（引擎原料）

### 4.0 合规前缀块（H 类强制注入，置于任何医疗 prompt 之前/之后）

```text
[边界声明] 本输出为辅助草稿/分析，非诊断、非治疗建议、非处方；
最终医疗决定须由执业医师作出，本输出须经合格专业人员逐项人工审核后方可使用。
[数据边界] 仅依据用户提供的材料；材料未含的信息不得臆造——
缺失用 {{<unknown> / [占位符] / 加粗}} 标出（按任务选 §2.4 三法之一）。
[隐私边界] 含患者可识别信息（PHI/PII）的任务只可在合规服务内运行
（见 §5 服务清单）；优先先用 4.1 去标识化再进入其他工作流。
[交互边界] 面向患者的文本一律为"供医护审核的草稿"，AI 不直接面向患者。
```

### 4.1 去标识化骨架（PHI 任务的第一道工序）

```text
将下列文本中 18 类 HIPAA 标识符（姓名/日期/电话/传真/邮箱/SSN/病历号/
保险号/账号/证件号/车牌/设备ID/URL/IP/生物特征/照片/其他唯一编号/地址细分）
替换为 [Patient Name]、[Date] 式占位符；保留全部临床语义。
输出两部分：①去标识文本 ②替换/删除项清单（供核对）。
文本：{{text}}
```

### 4.2 临床文书骨架（SOAP / 出院小结共用）

```text
角色：{{专科}}临床文书助理（供医师审核）
输入：{{病历/转录}}
格式：{{SOAP 按问题分条，S 详尽引患者原话，A 附 (ICD-10)，P 附 (CPT)}}
     或 {{出院小结：{{初中}}阅读水平白话，分步居家护理，警示症状+何时就医，鼓励收尾}}
规则：只用输入材料；转录外信息不补写；编码标注供编码员复核。
+ 4.0 合规块
```

### 4.3 决策支持骨架（鉴别诊断 / 试验资格 / 随访框架共用）

```text
角色：expert {{美国/本地化}} physician, specializing in {{专科}}
     （试验任务改：trial administrator，与医师同行对同行口吻）
任务：{{鉴别诊断清单（按可能性排序+支持/反对证据）+下一步 labs/imaging/referrals+社会史}}
     / {{逐条标准判定 meets|does not|potentially meets+证据}}
     / {{随访问题框架，背景清单：性别/年龄/主诉/共病/用药/过敏/社会史/病程}}
缺信息协议：按优先级列缺失清单 → 提问 → WAIT（收到回答前不生成结论）→ 汇总报告
+ 4.0 合规块
```

### 4.4 患者沟通骨架（回复 / 解释 / 资源查找共用）

```text
角色：{{护士姓名+资质，代表 {{N}} 位医生}} / 健康科普作者
受众三元组：阅读水平 {{grade}} × 文化背景 {{culture}} × 年龄 {{age}}
输出：{{回复草稿（友善建立关系、性别中性代词、署名待真人审核）}}
     / {{每受众一版 {{400-500}} 词解释，文化顾虑先共情}}
     / {{资源清单：{{半径}}内 {{N}} 个 {{类型}} 选项，注明滑动收费}}
资源查找附加条款：机构名/地址/电话高幻觉风险——逐条标注"需电话核实"，
不得给出未经核实的联系方式为确定事实。
+ 4.0 合规块
```

### 4.5 行政骨架（预授权 / 表单抽取）

```text
预授权：说服性信函致 {{保险方}}，论证 {{治疗}} 医学必要性并回应 {{拒赔理由}}；
  凡非输入材料的信息一律 **加粗**，供临床医生核实替换后再寄出。
抽取：从 {{HL7/表单}} 按固定顺序抽取 {{Name/Gender/DOB/Medication List/Problems}}；
  忽略账单段；缺失填 <unknown>；禁止推断。
+ 4.0 合规块
```

### 4.6 多轮采集 Loop 骨架（引擎 Loop 模式对接）

```text
阶段1 列缺失信息清单（按临床优先级排序）
阶段2 每轮提问一组 → 显式 WAIT → 接收回答 → 更新已知信息表
阶段3 全部收齐（或用户声明放弃某项，标注"信息不全"）→ 生成 {{最终报告}}
每轮输出末尾附：已收集 {{k}}/{{n}} 项，剩余待问清单。
```

## 5. 差异与注意点

- **许可证 MIT**：可自由改写复用，保留版权声明即可；本篇样本均为归纳改写。
- **HIPAA 环境硬约束**：README 明示消费级 ChatGPT **不合规**处理 PHI；合规选项为 BastionGPT、Azure OpenAI（BAA）、O365 Copilot（企业合规边界内）等签 BAA 的服务。12 个工作流全部标注 Requires PHI/PII=YES——引擎路由 H 类时必须先确认运行环境，否则先走 4.1 去标识化或拒绝注入真实患者数据。GDPR/中国《个人信息保护法》场景同理：敏感个人信息需单独同意与合规通道。
- **原则 5（守护患者交互）**：所有面向患者的输出都是草稿，由真人医护审核后发出；不得组装"AI 直接答复患者"的自动化链路。
- **编码与事实需专项复核**：ICD-10/CPT 由模型给出的编码必须经编码审核；Locate-Healthcare-Services 源文件自注高幻觉倾向（机构可能不存在），骨架已加"电话核实"条款。
- **美国语境本地化**：角色声明（American physician）、保险预授权流程、HIPAA 18 标识符、滑动收费诊所均为美国制度；用于中国场景需替换为本地执业角色、医保/商保流程与《个保法》标识符清单，SOAP 结构本身可通用。
- **加粗法与哨兵值法不可混用**：生成型任务（信/小结）用加粗标编造；抽取型任务用 `<unknown>` 禁编造。组装时按任务类型二选一，避免"允许补写"污染抽取任务。
- 与 ENGINE.md 对接：H 类属高合规档，第 5 步注入 4.0 块 + 本节环境条款；QC 门增加"编造内容是否已标记/患者数据是否已脱敏/是否声明人审"三查。

## 6. 缺口

- **中文医疗素材为零**：无中文问诊、中医、中国医保/病历规范（如《病历书写基本规范》）工作流，需另采。
- 无问诊模拟/教学（OSCE、标准化病人）与危机干预（自杀风险、急诊分诊）类 prompt——此类风险更高，采集时需额外伦理审查。
- 无影像/检验报告解读、用药相互作用核查类工作流（幻觉后果更重，素材作者未收录）。
- 12 个工作流均无输出样例与评测数据，效果需自行验证；README 提到的原则未配检查清单，本篇 §4.0 为归纳补全。
- 心理健康、慢病随访管理、护理排班等场景未覆盖。