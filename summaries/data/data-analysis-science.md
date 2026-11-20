# 数据分析与数据科学提示词

> 一级：D 数据 · 二级：数据分析（清洗、统计、建模、可视化、报告）
> 素材：素材/github/ChatGPT-Data-Science-Prompts/（README.md 索引 + prompts/ 11 类 68 文件）；素材/web/quickref.me/chatgpt-cheatsheet.md（Data Analysis、Spreadsheet、Prompt Structure/Constraint 段）
> 来源：travistangvh/ChatGPT-Data-Science-Prompts（未声明许可证，部分条目 credit 给 @svpino、DataLemur 等）；QuickRef.ME（内容未声明许可证，收集自社区）

## 1. 概述

覆盖数据科学日常工作流的"提示词动词表"：写代码（Python/R/Shell/VBA/Sheets 公式）、解释代码与概念、优化、格式化、跨语言翻译、出主意（特征工程/边缘用例/AB 测试）、排障、写 SQL、杂项（表格化/摘要）。60 条编号提示词按 11 类组织，本质是**任务类型学**而非深度方法论。流派：

- **角色任务派**：统一句式 `I want you to act as a [角色] … [任务+槽位]`，角色随任务切换（data scientist / coder / statistician / code optimizer / instructor / career coach），一条提示只干一件事。
- **受众分层派**：同一概念按受众生成五档解释（五岁小孩/本科生/教授/业务干系人/StackOverflow 风格），是"受众校准"维度最干净的素材。
- **速查指令派（quickref）**：无角色、一句话指令（Extract all numbers from text / Create table from data / Filter list based on [condition]），代表轻量数据操作的最小提示形态；其 Prompt Structure 段给出通用组装公式 `[role] [task] [style] [output format] [constraint]`。

## 2. 共性结构与模式

1. **三段微结构**：角色声明 → 上下文槽位（数据集描述/列名/目标变量/已训模型）→ 任务动词 + 约束（库名、指标、方言、版本）。平均不到 40 词，靠槽位具体化保证质量。
2. **目标变量驱动建模**：分类/时序/调参提示都以 `predicts [target variable]` 为锚，时序额外要求显式划分 train/validation 时间段——防数据泄漏的朴素表达。
3. **指标显式化**：AutoML 提示点名 "highest AUC score on the test set"；对比实验要求 "repeated for 5 times … output the runtime and other summary statistics"。可复现性约束直接写进提示。
4. **期望-实际排障式**：`This code is supposed to [expected function] … cannot be run [code]`；对模型自己刚生成的代码则有极简反馈式 `Your above code is wrong. [Point out what is wrong]. Can you try again?`——多轮纠错的最小原型。
5. **受众即参数**：解释类提示只换受众词不换任务，说明"解释深度"应做成引擎的可枚举槽位。
6. **验证即代码**：Validate Column 要求"写代码测试 DataFrame 满足 [requirements]"——把数据质检转化为可执行断言，而非让模型口头判断。
7. **出处文化**：部分条目附 credit.txt（@svpino、DataLemur、Mathias Halkjær Petersen），提示词社区有署名惯例，汇编时应保留。

## 3. 代表样本

### 3.1 建模与 AutoML（素材/github/ChatGPT-Data-Science-Prompts/prompts/1_write_python/1_train_classification_model/prompt.txt、2_automatic_machine_learning/prompt.txt）

```text
I want you to act as a data scientist and code for me. I have a dataset of
[describe dataset]. Please build a machine learning model that predict [target variable].

I want you to act as an automatic machine learning (AutoML) bot using TPOT for me.
I am working on a model that predicts [...]. Please write python code to find the
best classification model with the highest AUC score on the test set.
```

### 3.2 特征工程与 AB 测试（素材/github/ChatGPT-Data-Science-Prompts/prompts/7_suggest_ideas/6_suggest_feature_engineering/prompt.txt、7_suggest_ab_testing_steps/prompt.txt）

```text
I want you to act as a data scientist and perform feature engineering. I am working
on a model that predicts [insert feature name]. There are columns: [Describe columns].
Can you suggest features that we can engineer for this machine learning problem?

I want you to act as a statistician. [Describe context] Please design an A/B test
for this purpose. Please include the concrete steps on which statistical test I should run.
```

### 3.3 受众分层解释（素材/github/ChatGPT-Data-Science-Prompts/prompts/6_explain_concepts/ 各 prompt.txt）

```text
I want you to act as a data science instructor. Explain [concept] to a five-year-old.
… to an undergraduate. / … to a professor. / … to a business stakeholder.
I want you to act as an answerer on StackOverflow. You can provide code snippets,
sample tables and outputs to support your answer. [Insert technical question]
```

### 3.4 排障与数据验证（素材/github/ChatGPT-Data-Science-Prompts/prompts/8_troubleshoot_problem/2_correct_python_code/prompt.txt、1_write_python/18_validate_column/prompt.txt）

```text
I want you to act as a software developer. This code is supposed to [expected function].
Please help me debug this python code that cannot be run. [Insert function]

I want you to act as a data scientist. Please write code to test if that my pandas
Dataframe [insert requirements here]
```

### 3.5 轻量数据操作与通用约束（素材/web/quickref.me/chatgpt-cheatsheet.md，Data Analysis / Spreadsheet / Constraint 段）

```text
Extract all numbers from text
Filter list based on [condition]
Create table from data
Generate spreadsheet dummy data as a placeholder
Explain formula =SUM(A1:A10) in simple terms
约束词表：Be [extremely detailed|concise] / In [quantity] [words|sentences] /
No jargon or technical terms / Show [quantity] [examples|options] / Only show [changed|unchanged] content
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 通用数据分析任务骨架（quickref 公式 + 角色任务派融合）

```text
你是 {{角色：数据科学家/统计学家/数据工程师}}。
背景：{{数据集描述：来源、行数、列名与类型、业务含义}}。
任务：{{任务动词：探索/清洗/建模/可视化/检验}} {{具体目标}}。
约束：使用 {{库与版本}}；{{指标/口径}}；{{输出形式：可运行代码+结果解读}}。
{{可选：先列出你的假设与数据质量疑点，再给代码}}
```

### 4.2 建模骨架（分类/回归/时序通用）

```text
我有数据集 {{描述与列清单}}，目标变量 {{target}}，任务类型 {{分类/回归/时序}}。
请给出完整可运行 {{language}} 代码：
1) 数据检查与预处理（缺失/异常/类型）
2) {{时序必填：按时间切分 train={{range}} / valid={{range}}，禁止随机切分}}
3) 基线模型 + {{可选：AutoML 搜索，优化指标 {{AUC/RMSE/…}}}}
4) 评估（{{指标}} + 混淆矩阵/残差图）
5) {{可选：不平衡处理——过采样/欠采样方案与代价}}
每步附一句话说明为什么。
```

### 4.3 特征工程骨架

```text
预测目标：{{target}}。现有列：{{列名+类型+业务含义}}。
请建议 {{N}} 个可构造特征：每个给出业务动机、构造公式/代码、
预期信息增益方向、泄漏风险检查（是否用到未来信息/目标代理变量）。
```

### 4.4 AB 测试设计骨架

```text
背景：{{业务场景与假设}}。核心指标 {{metric}}，护栏指标 {{guardrails}}。
请以统计学家身份设计 AB 测试：分流单元与比例、样本量计算（给出 α/power/MDE 假设）、
实验时长、应跑的统计检验 {{及前提条件检查}}、常见陷阱（辛普森悖论/新奇效应/多重比较）。
输出可执行步骤清单。
```

### 4.5 受众分层解释骨架

```text
把 {{概念/模型结果/代码}} 解释给 {{受众枚举：儿童/本科生/同行专家/业务干系人}}。
{{业务干系人档：只讲影响与决策含义，禁术语，用类比，≤{{N}} 句}}
{{专家档：含推导/复杂度/文献定位}}
{{StackOverflow 档：结论先行 + 可运行示例 + 示例输出}}
```

### 4.6 排障骨架（含自纠错反馈式）

```text
代码本应 {{expected}}，实际 {{报错/错误输出}}，环境 {{库版本/DBMS}}。
[CODE]{{code}}[/CODE]
请：定位根因 → 最小修复 → 说明为何原写法错 → 给一个防回归的测试用例。
（多轮极简式：你上面的代码错了，{{具体错在哪}}，请重试。）
```

### 4.7 数据质检断言骨架

```text
为 {{DataFrame/表}} 写可执行质检代码，断言：{{规则清单：非空/唯一/值域/参照完整性/分布漂移}}。
每条规则输出违规行数与样例；全部通过时打印 PASS 汇总。
```

### 4.8 轻量提取/转换骨架（quickref 式，无角色）

```text
{{Extract all numbers/emails/URLs from text | Filter list based on {{condition}} |
Create table from data | Format the following into a table for {{Docs/Sheets}}}}
输入：{{raw_text}}。输出：{{格式}}，不要解释。
```

## 5. 差异与注意点

- **许可证**：主素材仓库**未声明许可证**（68 文件版权归原作者，部分另有 credit 署名）；quickref 内容同样未声明。两者都只能作为**研究/改写原料**：引擎应吸收其任务类型学与槽位设计，重写文本，不逐字复制；保留 credit 署名惯例。
- **时代局限**：素材成型于早期 ChatGPT，提示极短、无输出 schema、无质量门。现代模型下可直接用，但生产环境应叠加 4.1 的约束段与输出格式段（参见 P 类结构化提示词汇总）。
- **角色≠能力**：`act as a statistician` 不保证统计严谨；AB 测试、样本量等关键计算应要求模型展示公式与假设，或交由代码执行验证。
- **数据隐私**：全部素材都没有"上传真实数据前脱敏"条款——引擎组装涉及真实数据集时应默认追加脱敏/最小化提示。
- **misc 类越界**：情绪支持、书籍摘要等条目属 O/E 类，不应混入数据类引擎目录；书籍/论文摘要可归"翻译/总结/提取"二级。
- **与营销分析的边界**：AiCMO 的 MMM/LTV:CAC 等营销科学提示归 B2B商业/产品与运营.md，本篇只覆盖通用数据科学；统计方法骨架（4.4）可跨类复用。

## 6. 缺口

- 无**数据治理**素材（元数据管理、血缘、质量规则体系）——分类体系中 D 类明确列出但素材未覆盖。
- 无**分析报告叙事**模板（从图表到给管理层的结论故事线）；可视化仅有"写画图代码"，无图表选型/配色/误读防范。
- 无 **agent 式数据分析**（code interpreter 循环：假设→执行→观察→修正）素材，需与 S/P 类工作流骨架拼接。
- 无中文数据集/中文报告场景；无大数据工具链（Spark/dbt/BI 语义层）提示词，PowerBI 排障仅 1 条。
- 缺统计严谨性护栏（置信区间、效应量、因果措辞约束）的系统素材。