# 游戏 · 系统与玩法设计

> 一级类 G（游戏）· 二级维度「系统设计/玩法」
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「系统与玩法设计」解决的是：把模糊的游戏创意落成**可实现、可测试、好玩**的规则系统——核心循环、战斗/成长/经济等机制、架构范式选择，以及每个系统的 GDD（游戏设计文档）。它处在"创意"与"代码"之间，是游戏 spec 的主体。

素材里的三种做法：

- **理论驱动的设计 agent**（CCGS `game-designer`）：把 MDA、自我决定论（SDT）、心流、Bartle 玩家类型、平衡方法论、sink/faucet 经济模型写成 agent 的"理论框架"，要求每个决策都挂到游戏支柱（pillars）上，并产出 8 节式 GDD。
- **架构知识插件**（game-dev-skills `game-architect`）：不含工作流，只提供"范式选择指南（DDD / 数据驱动 / 原型）+ 按系统分类的 references 路由表"，配合 SpecKit/OpenSpec 或 plan 模式使用。
- **里程碑式 GDD 模板 + 具体玩法规格**（gat `system-gdd` 模板、`../游戏Spec01.md` 第四节）：前者给出"摘要→描述→玩家流→规则与边界→公式与调参→依赖与反馈→可观察结果→开放问题"的文档骨架；后者直接写出战棋 RPG 的 WT 行动序、朝向、元素相克、职业、成长、说服、阵亡、敌方 AI 等完整规则。

## 2. 共性结构与模式

归纳出系统设计提示词的通用骨架：

1. **先定体验目标，再定机制**：MDA 的"从目标 Aesthetics 反推 Dynamics 再到 Mechanics"；gat 模板的"Player Fantasy / 玩家应感受到什么"；Spec01 的"基调严肃写实、道德灰色"。共性是**先问"玩家该感觉如何"，再问"造什么系统"**。
2. **核心循环分层**：CCGS 的嵌套循环模型（30 秒微循环 / 5–15 分钟中循环 / 会话级宏循环），每个机制必须挂到至少一个循环。
3. **规则精确无歧义**：gat"程序员只读 Detailed Rules 一节就能实现"；Spec01 把每条规则写成可执行约束（WT 受敏捷/职业/装备重量影响、高打低有加成、弓射程随高度差增加且被障碍挡）。
4. **公式 + 调参旋钮**：所有数值系统给出公式、变量定义、输入范围、算例；CCGS 把旋钮分三类——feel（手感，靠试玩调）/ curve（曲线，靠数学模型调）/ gate（节奏，靠会话时长调），且必须外置到数据文件。
5. **边界与退化策略**：每个机制都要写 edge cases、dominant strategies（支配策略）、exploits，以及如何处理（Sirlin"Playing to Win"区分健康精通与退化玩法）。
6. **依赖与验收**：写明与其他系统的数据流方向、集成契约；验收同时含功能标准（做对了吗）与体验标准（感觉对吗、试玩验证什么）。
7. **架构范式按复杂度选**：game-architect 的核心判据——规则复杂/实体多 → DDD；内容/流程复杂、策划要免改代码迭代 → 数据驱动；快速验证 → 原型；多数项目混用（宏观一致 + 核心实体用 DDD + 内容流程用数据驱动）。

## 3. 代表样本

**样本 A — 设计 agent 的理论框架与平衡方法论**
`github/Claude-Code-Game-Studios/.claude/agents/game-designer.md`
```
3. Balancing Framework: transitive balance (A>B>C in cost and power),
   intransitive (rock-paper-scissors), frustra (apparent imbalance with hidden
   counters), asymmetric (different capabilities, equal viability).
#### Tuning Knob Methodology — every numeric system exposes three categories:
1. Feel knobs: moment-to-moment (attack/move speed, anim timing) — playtest intuition.
2. Curve knobs: progression shape (requirements, scaling, cost multipliers) — math.
3. Gate knobs: pacing (level reqs, thresholds, cooldowns) — session-length targets.
All tuning knobs must live in external data files, never hardcoded.
```

**样本 B — 8 节式 GDD 标准**
`github/Claude-Code-Game-Studios/.claude/docs/coding-standards.md`
```
1. Overview — one-paragraph summary
2. Player Fantasy — intended feeling and experience
3. Detailed Rules — unambiguous mechanics
4. Formulas — all math defined with variables
5. Edge Cases — unusual situations handled
6. Dependencies — other systems listed
7. Tuning Knobs — configurable values identified
8. Acceptance Criteria — testable success conditions
```

**样本 C — 架构范式选择判据**
`github/game-dev-skills/skills/game-architect/SKILL.md`
```
| Signal | Favor DDD | Favor Data-Driven |
| Entity interactions | Complex multi-entity rules | Mostly CRUD + display |
| Behavior source | Varies by entity type | Driven by config tables |
| Change frequency | Rules change with balance | Content/flow changes more often |
| Networking | Stateful objects acceptable | Flat state snapshots preferred |
| Team workflow | Programmers own the logic | Designers iterate without code |
```

**样本 D — 里程碑式系统 GDD 模板（规则 + 公式编号）**
`github/game-dev-skills/gat/.claude/docs/templates/design/system-gdd.md`
```
## 4. Rules And Cases  (number rules from 01)
### Rule 01 : [Rule Name]
[when it applies, how it shapes play, result, design reason]
**Choice or trade-off:** ... **Boundary cases:** ...
## 5. Formulas And Tuning  (number formulas from 01)
### Formula 01 : [Name]
- Design meaning / Formula `result = expression` / Terms /
  Initial range and reason / Worked example / Tuning effect
```

**样本 E — 用户样本的具体玩法规格（WT 制 + 朝向 + 高低差）**
`../游戏Spec01.md`
```
### 4.2 行动顺序（WT 制，不是双方轮流的回合制）
- 每个单位按自己的等待时间（WT）独立行动。WT 受敏捷、职业和装备重量影响。
- 一次行动里只移动或只行动的单位，下次的等待时间会缩短。
### 4.3 战斗规则
- 朝向：行动结束时选择面朝方向。从侧面或背后攻击，命中率和伤害更高。
- 元素：风火地水光暗，相克关系影响伤害。
- 攻击预测：出手前显示命中率、预计伤害、能不能击杀。
```

**样本 F — 用户样本的职业系统规格（成长 + 转职 + 技能）**
`../游戏Spec01.md`
```
### 4.4 职业（至少 6 种基础职业 + 2 种进阶职业）
- 每个职业都要有：基础属性成长、可用武器和防具类型、移动力和跳跃力、
  3–5 个主动技能加 1–2 个被动技能，以及独特的战斗小人外观。
- 转职在营地进行。进阶职业需满足等级、属性或阵营条件。
### 4.5 成长与加点
- 升级：按职业基础成长自动提升属性；另获若干自由属性点由玩家手动分配；
  技能点用来学习或强化职业技能。提供"推荐加点"按钮；营地可花金币重置。
```

## 4. 可复用模板骨架

```markdown
# 系统设计：<系统名>（里程碑 M{N}）
## 0. 体验目标（先于机制）
- 玩家应感受到什么（目标 MDA aesthetics）：<...>
- 挂到的游戏支柱 / 核心循环层级（微/中/宏）：<...>

## 1. 摘要与边界
- 一句话概述 + 玩家-facing 结果。
- 本系统拥有什么、什么属于别的系统。

## 2. 玩家流
1. 玩家动作/事件 → 系统响应 → 可见后果（含成功/失败分支）。

## 3. 详细规则（编号 Rule 01…，精确无歧义，程序员可据此实现）
### Rule 01 <名>
- 何时适用 / 如何塑造玩法 / 结果 / 设计理由。
- 选择与权衡：玩家能权衡或改变什么。
- 边界与退化：极值/中断/失败时怎样；支配策略与 exploit 如何缓解。

## 4. 公式与调参（编号 Formula 01…）
### Formula 01 <名>
- 设计含义 / 公式 `result = expr` / 各项定义与取值范围 /
  初始值与理由 / 具体算例 / 调高或调低的影响。
- 旋钮分类：feel（试玩调）/ curve（数学调）/ gate（节奏调），全部外置到 <数据文件>。

## 5. 架构范式（按需）
- 规则复杂/实体多 → DDD；内容流程复杂/策划免改码 → 数据驱动；快速验证 → 原型。
- 混合策略：宏观模块框架一致 + 核心实体 DDD + 内容/流程数据驱动。

## 6. 依赖与反馈
- 依赖系统 + 需要的行为；对别的系统的玩家-facing 后果；
   meaningful 事件 → 玩家必须感知到什么（视觉/音频处理归美术文档）。

## 7. 可观察结果 / 验收
- 功能标准：做对了吗（可自动化测试）。
- 体验标准：感觉对吗（试玩验证什么）。

## 8. 开放问题
- 未决设计选择及其后果。
```

## 5. 差异与注意点

- **理论密度差异**：CCGS game-designer 把大量设计理论（MDA/SDT/Flow/Bartle/Quantic Foundry/Sirlin）写进提示词，适合"有理论依据的严肃设计"；gat 模板更轻、面向里程碑交付；Spec01 几乎不谈理论、直接给规则。引擎应按"是否需要理论论证"选择密度，避免给简单原型套上沉重框架。
- **GDD 节数可配置**：CCGS 明确 8 节是 `full` 档，`standard` 只要 5 节（+ 数值系统加 Formulas），`minimal` 干脆不写 GDD、用一页 game-brief。模板骨架应支持降档，而非永远 8 节。
- **架构知识 vs 工作流**：game-architect 自述"只含领域知识、不含工作流"，必须配 SpecKit/OpenSpec 或 plan 模式；单独喂给 agent 不会自动产出设计文档。
- **许可证**：CCGS **MIT**；game-dev-skills **未声明许可证**（个人 skill 仓库，引用/再分发有风险，建议只借鉴结构与判据表，不整段复制）；`../游戏Spec01.md` 用户自有。
- **玩法借鉴 vs 抄袭**：Spec01 明确"系统参考《皇家骑士团》但主线原创、禁用原始素材/角色名/台词/旋律"。系统设计可借鉴机制结构，但具体表达须原创——这是法律与创意的双重边界。

## 6. 缺口

- 缺少**多人/网络玩法系统**的设计提示词范本（权威划分、同步策略、回滚）——game-architect 有 references 知识但未成"设计 agent 提示词"。
- 缺少**程序化内容生成（PCG）/ Roguelike** 的完整设计规格样本（game-architect 列了 system-pcg 路由，但无成品 spec）。
- 缺少**新手引导/教学关**作为独立系统的设计模板（Spec01 有教学关、CCGS 提"scaffolded challenge"，但无专门 GDD 骨架）。
- 缺少**UI/UX 系统**（HUD、菜单、输入映射）的设计提示词——CCGS 有 ux-designer agent，但本批未沉淀成模板。
- 数值平衡的"可执行模拟验证"在 `关卡与数值设计.md` 详述，本篇只到公式与旋钮层。