# text-to-SQL 与数据库提示词

> 一级：D 数据 · 二级：text-to-SQL/数据库
> 素材：素材/github/sqlcoder/（prompt.md、inference.py、metadata.sql、README.md）；素材/github/ChatGPT-Data-Science-Prompts/prompts/（write_sql、optimize_code、format_code、translate_code、troubleshoot_problem 各 SQL 相关条目）
> 来源：defog-ai/sqlcoder（代码 Apache-2.0，模型权重 CC BY-SA 4.0）；travistangvh/ChatGPT-Data-Science-Prompts（未声明许可证）

## 1. 概述

解决"自然语言问题 → 可执行 SQL"以及 SQL 的解释、优化、格式化、方言迁移、纠错全生命周期。素材呈现两个明显流派：

- **生产模板派（SQLCoder）**：为专用微调模型设计的极简固定模板——任务声明 + 拒答指令 + DDL 上下文 + 答案前缀引导，四段式，无角色扮演、无废话。模板与模型配套训练，配套确定性解码（beam search、do_sample=False）与输出后处理，是"提示词即产品接口"的范例。
- **对话任务派（Data Science Prompts）**：通用 ChatGPT 时代的"I want you to act as …"角色句式，靠用户手工描述表结构、显式指定 DBMS 方言与版本。短、泛、可迁移，但无 schema 注入规范、无拒答约束。

两派互补：前者给引擎提供**结构化组装骨架**，后者提供**任务动词库**（写/解释/优化/格式化/翻译/纠错）。

## 2. 共性结构与模式

1. **Schema 即上下文，注释即语义**：SQLCoder 的 metadata.sql 不只给 DDL，每个列带行内注释（业务含义），末尾用独立注释行显式声明 join 关系（`-- sales.product_id can be joined with products.product_id`）。这是降低 join 幻觉的关键手段——把 ER 关系写成自然语言提示。
2. **拒答条款（abstention）**：`If you cannot answer the question with the available database schema, return 'I do not know'`。给模型一个合法出口，比放任编造列名更安全。
3. **答案前缀引导（priming）**：`### Answer` 段复述问题并以 `[SQL]` 开头，把模型直接推入"续写 SQL"状态，配合停止符（``` 或 ;）截断，输出即纯 SQL。
4. **问题双写**：user_question 在 Task 段与 Answer 段各出现一次，长 schema 场景下防止问题被上下文淹没。
5. **方言显式化**：对话派每条 SQL 提示都点名 DBMS 甚至版本（"works for PostgreSQL 14"、"does not run in [your DBMS]"）。方言是 SQL 正确性的第一约束变量。
6. **期望-实际调试式**：纠错提示给出"代码本应做什么"（expected function）+ 运行环境 + 代码本体，三要素齐备。
7. **提示词之外的工程配套**：SQLCoder 用 `prompt.format()` 拼装、max_new_tokens=300、num_beams=5、按 `;` 和 ``` 切分补分号。说明生产级 text-to-SQL 的效果一半在模板、一半在解码与后处理约定。

## 3. 代表样本

### 3.1 生产级模板全文（素材/github/sqlcoder/prompt.md，13 行）

```markdown
### Task
Generate a SQL query to answer [QUESTION]{user_question}[/QUESTION]

### Instructions
- If you cannot answer the question with the available database schema, return 'I do not know'

### Database Schema
The query will run on a database with the following schema:
{table_metadata_string}

### Answer
Given the database schema, here is the SQL query that answers [QUESTION]{user_question}[/QUESTION]
[SQL]
```

### 3.2 带注释与 join 提示的 schema（素材/github/sqlcoder/metadata.sql 节选）

```sql
CREATE TABLE sales (
  sale_id INTEGER PRIMARY KEY, -- Unique ID for each sale
  product_id INTEGER, -- ID of product sold
  customer_id INTEGER,  -- ID of customer who made purchase
  salesperson_id INTEGER, -- ID of salesperson who made the sale
  sale_date DATE, -- Date the sale occurred
  quantity INTEGER -- Quantity of product sold
);
-- sales.product_id can be joined with products.product_id
-- sales.customer_id can be joined with customers.customer_id
-- sales.salesperson_id can be joined with salespeople.salesperson_id
```

### 3.3 方言限定的查询生成（素材/github/ChatGPT-Data-Science-Prompts/prompts/9_write_sql/1_create_running_average/prompt.txt）

```text
I want you to act as a data scientist and write SQL code for me. I have a table
with two columns [Insert column names]. I would like to calculate a running
average for [which value]. What is the SQL code that works for PostgreSQL 14?
```

### 3.4 SQL 纠错（素材/github/ChatGPT-Data-Science-Prompts/prompts/8_troubleshoot_problem/3_correct_sql_code/prompt.txt）

```text
I want you to act as a SQL code corrector. This code does not run in
[your DBMS, e.g. PostgreSQL]. Can you correct it for me? [SQL code here]
```

### 3.5 方言迁移与格式化（素材/github/ChatGPT-Data-Science-Prompts/prompts/5_translate_code/1_translate_between_dbms/prompt.txt、4_format_code/3_format_sql/prompt.txt）

```text
I want you to act as a coder and write SQL code for MySQL. What is the
equivalent of PostgreSQL's DATE_TRUNC for MySQL?

I want you to act as a SQL formatter. Please format the following SQL code.
Please convert all reserved keywords to uppercase [Insert requirements]. [Insert Code]
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 生产级 text-to-SQL 主骨架（SQLCoder 式，可泛化到通用模型）

```markdown
### Task
Generate a {{dialect}} SQL query to answer [QUESTION]{{user_question}}[/QUESTION]

### Instructions
- Use only tables/columns present in the schema below; never invent identifiers
- If the question cannot be answered with the available schema, return 'I do not know'
- {{可选：只读约束——仅生成 SELECT，禁止 DDL/DML}}
- {{可选：聚合/日期/NULL 处理约定，如"日期比较用 DATE 类型直比"}}
- {{可选：输出仅一条 SQL，以分号结束，不加解释}}

### Database Schema
The query will run on a database with the following schema:
{{ddl_with_column_comments}}
{{join_hints：-- A.col can be joined with B.col}}
{{可选：枚举值/业务口径说明，如 status 的取值含义}}

### Answer
Given the database schema, here is the SQL query that answers [QUESTION]{{user_question}}[/QUESTION]
[SQL]
```

组装要点：`{{ddl_with_column_comments}}` 由元数据管道生成（列注释=业务语义）；`{{join_hints}}` 从外键/ER 图推导；问题双写；配套解码参数（低温度/beam）与后处理（截断 ``` 与多余语句、补分号）。

### 4.2 SQL 解释骨架

```text
你是 {{受众：数据科学讲师/业务方/初级工程师}}。解释下面这段 {{dialect}} SQL 在做什么：
先一句话概括业务目的，再按 CTE/子查询逐层说明数据流，最后指出输出列含义。
[SQL]{{sql_code}}[/SQL]
```

### 4.3 SQL 优化骨架

```text
你是 SQL 性能优化专家。下面的查询在 {{dialect}} {{version}} 上运行缓慢。
表规模与索引情况：{{stats_and_indexes}}。
请：1) 指出慢的根因（全表扫描/隐式转换/不当 join 顺序等）
2) 给出改写后的 SQL 3) 说明预期收益与验证方法（EXPLAIN 看点）。
[SQL]{{sql_code}}[/SQL]
```

### 4.4 SQL 纠错骨架（期望-实际-环境三要素）

```text
这段 SQL 本应实现 {{expected_behavior}}，但在 {{dialect}} {{version}} 上
{{报错信息/错误结果}}。请定位原因并给出修正版，说明改了什么、为什么。
[SQL]{{sql_code}}[/SQL]
```

### 4.5 方言迁移骨架

```text
把下面的 {{source_dialect}} SQL 等价迁移到 {{target_dialect}} {{version}}。
逐一列出方言差异点（函数/类型/语法）及对应替换；语义必须完全等价，
无法等价处显式标注并给替代方案。
[SQL]{{sql_code}}[/SQL]
```

### 4.6 格式化骨架

```text
按 {{风格规范：关键字大写/缩进 2 空格/每子句换行/CTE 命名}} 格式化以下 SQL，
只改格式不改语义，输出格式化后的完整 SQL。
[SQL]{{sql_code}}[/SQL]
```

## 5. 差异与注意点

- **许可证**：sqlcoder 代码 Apache-2.0（可商用、需保留声明），但**模型权重 CC BY-SA 4.0**——微调后的权重必须同许可开源；只引用其提示词模板属代码/文本范畴，风险低。Data-Science-Prompts 仓库**未声明许可证**，版权归原作者：引擎应**改写重组**而非逐字搬运，引用需标注出处。
- **模板与模型耦合**：SQLCoder 极简模板是为微调模型训练的分布设计的；直接套到通用大模型上建议扩充 Instructions（只读约束、输出格式、口径约定），否则拒答与纯 SQL 输出行为不稳定。
- **注入面**：schema 与 join hints 来自元数据管道而非终端用户；用户问题应始终包裹在 [QUESTION] 标签内，防止问题文本篡改指令段。
- **方言缺省即错误源**：对话派素材证明"不写方言"是常见失败模式，引擎组装时 dialect 应为必填槽位。
- **评测口径**：sqlcoder README 按 date/group_by/order_by/ratio/join/where 六类评测，其中 ratio（比率）与 join 是弱项——引擎可据此对复杂问题追加 few-shot 或拆解步骤。

## 6. 缺口

- 无**执行反馈闭环**素材（生成→执行→报错→自修正的 loop 提示词），需引擎阶段自建或从 S/P 类 agent 素材借骨架。
- 无**多轮澄清**模板（问题歧义时先反问口径再出 SQL）。
- 无中文业务口径/中文列名场景素材；无大 schema 裁剪（只注入相关表）策略素材。
- 窗口函数、复杂分析 SQL 仅 running average 一例；无 SQL→自然语言反向解释的生产级模板。
- 数据安全（脱敏、行级权限、查询审计）约束条款缺失，引擎需补默认安全段。