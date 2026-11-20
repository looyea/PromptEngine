# 游戏 · 关卡与数值设计

> 一级类 G（游戏）· 二级维度「关卡/数值设计」
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「关卡与数值设计」解决两件事：**空间与节奏**（地图布局、遭遇配置、难度曲线、 pacing）和**数字与平衡**（战斗公式、成长曲线、经济 sink/faucet、掉落率、随机奖励的公平性）。它把"好玩"翻译成可测量、可验证、可调的具体参数与布局。

素材里的三种做法：

- **关卡设计 agent + 文档标准**（CCGS `level-designer`）：要求每关产出布局图、关键路径、可选路径、遭遇清单、pacing 图、叙事节拍、音频 cue，并用"引导元素（光照/几何/音频）"保证方向感。
- **可执行数值平衡 skill**（game-dev-skills `game-balance-analysis`、CCGS `balance-check`/`economy-designer`）：核心主张是"别只讨论平衡理论，要产出真实数值/表格/计算/裁决"，并**用可执行脚本或持久模拟器验证模型**，区分模型保真度与参数适配度。
- **整游戏 spec 的关卡与难度规格**（`../游戏Spec01.md` 第六节、playcanvas 的常量表）：直接给出 5 关的地形/目标/机制差异、难度曲线原则，以及把全部 tuning 数值锁进 `constants.ts` 的规范性数据块。

## 2. 共性结构与模式

归纳出关卡/数值提示词的通用骨架：

1. **先分类平衡目标**：game-balance-analysis 要求建模前先判定任务属于哪类——数学关系 / 难度 / 成长 / 初始条件 / 多策略可行性 / 对象比较价值 / 实际与感知公平。目标冲突时**说明权衡、优先用户意图体验，不静默牺牲某一类**。
2. **关系类型决定模型**：transitive（传递，越强越贵）/ intransitive（非传递，克制循环）/ situational（情境依赖）/ mixed。**不要把非传递或情境集合硬塞进一条绝对强度曲线**。
3. **保留有意义的差异**：数值相等不是目标；接受支持身份/发现/风险/可达性的刻意不对称，只有当某差异让一条合法路径变成"必选/无效/不可知/不公平"时才建议调整。
4. **随机系统分三层**：数学概率与分布 / 玩家效用（阈值、损失、重复、边际递减）/ 感知概率（信息、呈现、记忆、信任）。数学公平的系统若效用或感知公平违背意图体验，仍需重设计。
5. **证据匹配置信度**：数学给首轮值与边界检查，试玩给玩家行为与理解，遥测给重复人群模式；三者不可互相替代，并说明什么证据能确认或推翻结论。
6. **可执行验证 + 两种产物生命周期**：一次性验算用临时脚本（跑完留证据再删）；反复调参的系统用持久模拟器（交付并保留工具/参数/场景/结果集）。**先修模型再调参数，绝不用改参数掩盖已知模型缺陷**；随机/迭代模型用固定种子、报告分布而非单次。
7. **关卡文档标准 + pacing**：每关含名称主题、预计时长、布局图、关键路径、可选路径、遭遇清单（类型/难度/位置）、强度随时间的 pacing 图、叙事节拍、音频 cue；难度曲线走"锯齿形"（张力累积→里程碑释放→略高基线再启），避免平坦（无聊）与垂直尖峰（挫败）。
8. **经济 sink/faucet 闭环**：映射每个资源入口（faucet）与出口（sink），在目标会话时长内平衡；sink 问题未解决的设计"还没平衡"，要明说而非批准。
9. **锚定指标 + 失败恢复成本**：以 TTK（time-to-kill）/ TTC（time-to-complete）为首要调参锚点，其余数值由这些目标推导（CCGS game-designer）；失败成本须与失败频率成正比——高频失败（战斗死亡）需快速恢复，低频失败（Boss 战败）可有中等成本。难度曲线走锯齿形而非平坦或垂直尖峰，配合"前 10 分钟通过玩而非教程教学、每个新机制先孤立引入再组合"的脚手架挑战。

## 3. 代表样本

**样本 A — 平衡目标分类与关系类型**
`github/game-dev-skills/skills/game-balance-analysis/SKILL.md`
```
### Classify option relationships
- transitive: broadly more benefit should carry more cost or disadvantage;
- intransitive: value depends on the opposing choice or counter cycle;
- situational: value depends on content, state, timing, or frequency;
- mixed: more than one model is required.
Do not force an intransitive or situational set onto one absolute power curve.
### Preserve meaningful differences
Numerical equality is not the goal. Recommend adjustment when a difference makes
one legitimate path compulsory, futile, unknowable, or unfair.
```

**样本 B — 可执行验证与"先修模型再调参"**
`github/game-dev-skills/skills/game-balance-analysis/SKILL.md`
```
Validate both model fidelity and parameter fitness. When results conflict with
confirmed rules, determine whether the model/tool is wrong or the parameters miss
their targets. Fix and version the model and simulator first, add the failed case
as a regression check, rerun validation, and only then tune parameters. Never use
parameter changes to compensate for a known model defect.
```

**样本 C — 关卡文档标准**
`github/Claude-Code-Game-Studios/.claude/agents/level-designer.md`
```
Each level document must contain:
- Level Name and Theme / Estimated Play Time
- Layout Diagram (ASCII or described)
- Critical Path (mandatory route) / Optional Paths (exploration and secrets)
- Encounter List (type, difficulty, position)
- Pacing Chart (intensity over time)
- Narrative Beats / Music-Audio Cues
```

**样本 D — 经济 sink/faucet 与"未解决即未平衡"**
`github/Claude-Code-Game-Studios/.claude/agents/economy-designer.md`
```
Map all resource sources (faucets) and sinks. For each currency, state the faucet
rate, the sink capacity — including sinks that run out, such as a shop catalog a
player can exhaust — and the projected surplus or deficit over time. A design whose
sink question is open is not balanced yet: say so instead of approving it.
```

**样本 E — 无数据时的诚实裁决（防"假通过"）**
`github/Claude-Code-Game-Studios/.claude/skills/balance-check/SKILL.md`
```
If every required input is ABSENT, stop and report NOT ASSESSED — NO DATA as the
whole verdict, naming what was missing and which skill produces it.
A verdict of NOT ASSESSED is a success. The failure mode this prevents: a report
whose verdict enum has no "could not run" state produces false clean passes.
Absence of evidence is never evidence of absence.
```

**样本 F — 用户样本的关卡差异化与难度曲线**
`../游戏Spec01.md`
```
每关在地形、目标和机制上都要有明显区别：
1. 教学关：海港小镇夜袭，全灭敌军，逐步教会移动/攻击/朝向/高低差。
2. 峡谷山道：高低差大，弓手对决，击败首领。
3. 河畔村庄：浅水桥梁、下雨，保护 NPC 撤离，有可说服敌人。
4. 雪山要塞：攻城战、城墙阶梯、敌方增援、飘雪。
5. 王都神殿：最终战，首领多阶段，演出与剧情交织。
难度曲线：普通难度下第一次玩的玩家能通关，但需要动脑。
```

## 4. 可复用模板骨架

```markdown
# 关卡与数值设计：<对象>
## A. 平衡目标分类（先于建模）
- 任务类型：数学关系 / 难度 / 成长 / 初始条件 / 多策略可行性 / 比较价值 / 公平（可多选）。
- 目标冲突时的权衡与优先体验：<...>。
- 关系类型：transitive / intransitive / situational / mixed（决定用哪种模型）。
- 上下文锚定：受众、模式、内容集、技能段、build、时间跨度、版本。

## B. 数值模型与公式
- 公式 + 变量定义 + 输入范围 + 算例（普通/优化/边界三类都算）。
- 成长曲线形状：线性 / 二次 / 对数 / S 曲线；锚定 TTK / TTC 目标，其余值由此推导。
- 随机系统三层：数学分布 / 玩家效用 / 感知公平；必要时设 pity（N 次保底）。
- 经济：faucet 速率 vs sink 容量（含会耗尽的 sink），目标会话时长内的盈余/赤字。

## C. 可执行验证
- 临时验算脚本（跑完留证据再删）或持久模拟器（保留工具/参数/场景/结果集）。
- 固定种子、报告分布；先验模型保真度，再验参数适配度；失败案例进回归。
- 证据来源：数学（首轮值）/ 试玩（行为理解）/ 遥测（人群模式），说明何种证据可推翻结论。

## D. 关卡文档（每关）
- 名称主题 / 预计时长 / 布局图（ASCII）/ 关键路径 / 可选路径与秘密。
- 遭遇清单：敌人职业·等级·AI 模式·位置·生成时机·难度目标。
- pacing 图：强度随时间（锯齿形：累积→释放→略高基线再启），休息点。
- 胜负条件 / 战中事件 / 宝箱隐藏要素 / 叙事节拍 / 音频 cue。
- 引导元素：用光照/几何/音频标出方向感。

## E. 裁决与调参建议
- 推荐数值 / 调参选项 / 明确裁决 + 验证计划。
- 无数据时输出 NOT ASSESSED — NO DATA，点名缺什么、由哪个环节产出（绝不假通过）。
- 把参数变化翻译成玩家-facing 后果，并追踪对相邻系统的连锁影响。
```

## 5. 差异与注意点

- **"产出数值"vs"讨论理论"**：game-balance-analysis 明确"要真实数值/表格/裁决，不要泛泛平衡理论"，且"非平凡数值设计必须可执行验证"；CCGS balance-check 更偏"扫描公式与数据找离群值"。前者是设计/诊断/裁决三合一，后者是体检。
- **三任务路由 + 四领域路由**：game-balance-analysis 按任务读且只读一个主 playbook（创建首轮数值 / 诊断调参 / 评判提案），再按领域读一个（战斗成长 / 经济 / PvP metagame / 随机奖励），按需读计算配方与模拟工具参考。这种"按需路由、不全量加载"是控制上下文的关键写法，引擎组装时可借鉴。
- **诚实裁决是硬约束**：balance-check 把 `NOT ASSESSED` 定义为成功而非失败，专门防"无资产却报 COMPLIANT、无 profiler 数据却报 99% 余量"的假通过。引擎组装验收类提示词时应保留这个"could not run"状态。
- **数值规范性强度**：playcanvas 把常量视为"逐行精确转录的 shipped tuning"，Spec01 把关卡方案视为"可参考可自行调整"。复刻标杆用前者，原创开发用后者。
- **许可证**：CCGS **MIT**；game-dev-skills **未声明许可证**（引用有风险，建议借鉴方法论骨架而非整段复制）；playcanvas-prompts **未声明**；`../游戏Spec01.md` 用户自有。
- **不越界实现**：game-balance-analysis 末尾"除非用户任务或单独工程流程授权，否则不实现所选改动"——数值设计 skill 与编码实现解耦，引擎编排时注意阶段边界。

## 6. 缺口

- 缺少**具体品类的数值成品表**（如战棋职业成长表、武器 DPS 表、掉落率表）作为 few-shot 范例；Spec01 给了职业/属性框架但未填具体数值。
- 缺少**PvP/匹配/天梯/metagame** 的平衡提示词成品（game-balance-analysis 列了 pvp-and-metagame-playbook 路由，但本批未含该 reference 正文）。
- 缺少**程序化关卡生成**的约束提示词（保证可通关、难度可控、种子可复现）。
- 缺少**难度自适应（DDA）**与"简单/普通/困难"如何量化映射到敌人属性/AI 积极度/永久阵亡/回溯次数的具体公式范本（Spec01 提了开关但未给映射）。
- 缺少**遥测/试玩数据回收**的提示词（如何从 playtest 问卷与埋点反推调参），CCGS 有 analytics-engineer/playtest-report 但本批未精读其正文。
- 缺少**跨系统全局平衡**的提示词（单系统改动如何连锁影响相邻系统与整体经济/战力曲线，如何做全局而非局部平衡）。
- 缺少**感知公平的呈现设计**（概率公示、保底进度条、掉落动画如何影响玩家信任）的 UX 侧范本——game-balance-analysis 提了"感知概率"层但无呈现设计模板。
- 缺少**数值设计评审检查清单**成品（评审一份数值提案时逐项核对什么），可结合 spec-kit checklist-template 与 balance-check 组装。