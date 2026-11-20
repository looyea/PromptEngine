# 结构化提示词（LangGPT 方法论）

> 一级：P 提示词方法论 · 二级：结构化/模板化
> 素材：素材/github/langgpt/（Docs/HowToWritestructuredPrompts.md、LangGPT/templates/、examples/）
> 来源：github.com/langgptai/LangGPT（Apache-2.0，云中江树）

## 1. 概述

结构化提示词 = **像写文章一样写 Prompt**：用标识符（`#`、`<>`、`-`、`[]`）控制层级，用属性词（Role、Profile、Rules、Workflow…）标识语义模块。2023 年由 LangGPT 开源提出，已被百度、字节、华为、智谱等平台采用，是国内最具影响力的提示词范式。与 CRISPE 等"思维框架"的区别：框架只给内容要点（一层结构），结构化给**可多层嵌套的形式模板**，无记忆负担、可工程化维护。

## 2. 核心概念

- **标识符**：`#` 标题层级、`<>` 变量引用。层级即语义聚拢，降低模型理解难度。
- **属性词**：模块的语义标签，定向唤醒模型能力——`Role` 唤醒角色扮演，`Rules`/`Constraints` 约束幻觉与不良输出。属性词可自定义、可替换（Expert/Master 替代 Role 等）。
- **载体**：Markdown（非程序员友好）；YAML/JSON（工程化开发友好，模板见 templates/miniRole.yaml、miniRole.json）。
- **全局思维链**：好模板本身就是一条 CoT：
  `Role → Profile → Skill → Rules → Workflow → Initialization → 实际使用`
- **语义一致性**：格式上标识符功能前后一致（`#` 不混用作变量）；内容上属性词与模块内容匹配（Rules 里不堆技能描述）。
- **与其他技巧正交**：结构化是组织方式，CoT/ToT/few-shot 可嵌入其中协同。

## 3. 代表样本

### 3.1 基础模板（templates/baseRole.md 精简）

```markdown
# Role: 角色名
## Profile
- Author / Version / Language / Description
### Skill
1. 具体技能与预期输出
## Rules
1. 任何情况下不打破角色设定
2. 不编造事实
## Workflow
1. 先… 2. 然后… 3. 最后…
## Initialization
作为 <Role>，遵守 <Rules>，用 <Language> 对话，问好并介绍 <Workflow>。
```

### 3.2 诗人示例（README_zh.md，社区引用最广的样例）

```markdown
# Role: 诗人
## Profile
- Description: 诗人是创作诗歌的艺术家…
### 擅长写现代诗 / 七言律诗 / 五言诗（分点描述）
## Rules
1. 内容健康向上 2. 律诗要押韵
## Workflow
1. 让用户以"形式：[], 主题：[]"指定
2. 创作诗歌（题目+诗句）
## Initialization
作为角色 <Role>，严格遵守 <Rules>…
```

### 3.3 扩展模块（按需增删）

- `Commands`：`/help /continue /config /language` 等前缀命令（交互型角色）
- `Reminder`：每轮回复前自我提醒角色设定（长对话防漂移）
- `Output`/`OutputFormat`：机器可解析输出（JSON/XML），见 templates/autoGPT.md
- `Input`：定义上游 agent 传入格式（多 agent chain 协同时固定接口）
- `Tools`：browser/python/dalle 等工具说明（templates/Role.md）
- `Examples`：疑难用例 + 内心独白示范（Role.md 模板）
- `Capabilities / Limitations / Style`：能力、边界、语气三分（Role.md）

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 通用角色骨架

```markdown
# Role: {{领域专家角色}}
## Profile
- Language: {{语言}}
- Description: {{一句话角色定位}}
### Skill
1. {{技能1：能力+预期输出}}
## Rules
1. {{边界：不做什么}}
2. {{质量：不编造、可验证}}
3. {{文件/格式约定：产出写到哪、什么格式}}
## Workflow
1. {{理解输入}} 2. {{执行步骤}} 3. {{自检/交付}}
## Output
{{格式要求，可选 JSON schema}}
## Initialization
作为 <Role>，遵守 <Rules>，按 <Workflow> 开始工作。
```

组装要点：Role/Rules/Workflow/Initialization 为必选骨架；Skill/Output/Examples/Tools/Commands 按场景插拔；变量用 `{{}}` 或 `<>` 统一一种。

### 4.2 元提示词骨架（提示词生成提示词）

依据 §5 所述 LangGPT"Prompt 生成助手"路线归纳；完整版即 引擎/ENGINE.md（七步 harness），本骨架是其单文件轻量版，供引擎生成"某领域的提示词生成器"时直接套用：

```markdown
# Role: {{领域}}提示词工程师
## Profile
- Description: 根据用户的场景描述，产出可直接使用的{{领域}}生产级提示词
### Skill
1. 场景解析：提取槽位（任务/平台/交付物/自主度/硬约束），缺失标 [假设] 或反问
2. 骨架与技术选配：按任务特征选 zero/few-shot/CoT/参考文本块
## Rules
1. 你的输出是**提示词本身**，不是执行任务的结果
2. 生成的提示词必含 Role/Rules/Workflow/Initialization，边界与验收标准不可缺
3. 交付物为代码/多文件文档时，注入文件写入约定（产物清单/决策日志/写入权限）
4. 不编造出处与数据；缺信息声明"未提及"
## Workflow
1. 解析场景列槽位 → 2. 选骨架与技术 → 3. 组装 → 4. 自检（语义一致/可验证/合规）→ 5. 输出
## Output
【场景解析】【提示词正文】【组装说明】
## Initialization
作为 <Role>，遵守 <Rules>，请用户给出场景描述，开始生成。
```

## 5. 差异与注意点

- LangGPT 模板 vs Mr.-Ranedeer AI-Tutor：后者功能近似（Features≈Profile）但过于复杂不通用；LangGPT 是其简化通用化。
- 生产级用法：把 Prompt 当代码——Git 版本管理、模块化复用（Rules 可跨角色复用）、Input/Output 固定后多人并行开发各 agent。
- 元提示词：LangGPT 提供"Prompt 生成助手"，用提示词生成提示词（examples/ 与 README 有入口），是引擎"自动生成"路线的直接参照。
- 许可证 Apache-2.0：可自由使用修改，保留署名。

## 6. 缺口

- 结构化提示词的量化评测数据（素材多为经验论述）
- YAML/JSON 载体的大型生产案例（模板有，完整应用少）