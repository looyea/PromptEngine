# 游戏 · 工作流与验收

> 一级类 G（游戏）· 二级维度「工作流/自动化」+「评测/质检」
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「工作流与验收」解决的是：把游戏开发组织成**可推进、可恢复、可验证**的流程——多 agent 如何编排、阶段如何设门（gate）、进度/会话状态如何持久化、每个改动凭什么证据算"完成"、最终交付凭什么清单算"通关"。它是把前面各二级类（规约/设计/美术/叙事/音频）串成端到端管线的"编排层"。

素材里的三种做法：

- **工作室级多 agent 工作流**（CCGS）：49 agent + 78 skill + hooks，7 阶段管线（Concept→Systems Design→Technical Setup→Pre-Production→Production→Polish→Release），`/prototype` 一句话生成可玩浏览器游戏，director gate 用受控裁决词，`production/session-state/active.md` 作会话检查点，测试证据按故事类型分级。
- **里程碑驱动预制作工作流**（gat）：`brainstorm→story→milestone→design{M}{system}→下游工程`，两个循环（精修全局 / 迭代里程碑），文档所有权按角色划分，里程碑状态 `planned→designing→designed`，工程执行在 GAT 之外。
- **整游戏 spec 的里程碑 + 验收清单**（`../游戏Spec01.md` 第十~十二节、playcanvas 的 Acceptance）：M0–M5 里程碑、每里程碑运行测试并提交 git、PROGRESS.md 持续记录、末尾逐条可观察验收清单。

## 2. 共性结构与模式

归纳出游戏工作流/验收提示词的通用骨架：

1. **阶段化管线 + 阶段门**：把开发切成有序阶段，每阶段有 formal gate（`/gate-check`），通过才前进；rigor 可配（minimal 无门 / standard / full）。
2. **受控裁决词（verdict tokens）**：gate 返回的裁决必须来自该 gate 定义的 Verdicts 行（如 APPROVE/CONCERNS/NOT READY），首行单独给 token，调用方按首行分支；**不同 gate 不共享词表**，用错词即错答。
3. **"未评估"是合法且成功的裁决**：输入缺失时返回 `NOT ASSESSED — NO DATA`，点名缺什么、由哪个 skill 产出；防止"无数据却假通过"。缺失工件是 finding，不是 missing input。
4. **会话状态持久化（文件即记忆）**：`active.md` 是检查点，compaction/crash/clear 后先读它；每完成一节就追加（当前任务/已完成节/关键决策/下一节），使早期讨论可安全压缩。
5. **增量写文件 + 恢复**：先建骨架（所有节标题），逐节起草→批准→写入；中断后读 active.md + 目标文件，从下一个未完成节恢复，不重谈已完成节。
6. **测试证据按类型分级**：Logic→单元测试（BLOCKING）；Integration→集成测试或记录试玩（BLOCKING）；Visual/Feel→保留截图 + lead 签核（BLOCKING）；UI→每屏截图（BLOCKING）；Config/Data→smoke check（ADVISORY）。"保留"是 load-bearing——截图必须仍在磁盘上，拍完即弃只是断言不是证据。
7. **观察而非推断**：playcanvas"claims 必须来自观察运行中的游戏（浏览器输入/截图/console/runtime 查询），而非读源码"；CCGS"parse check 不等于 run"。
8. **里程碑 + 进度日志 + 最终验收清单**：Spec01 每里程碑结束运行测试并提交 git、PROGRESS.md 记录进度/已知问题/解决情况；末尾逐条核对验收清单，没满足的必须补完。
9. **协作协议贯穿**：Question→Options→Decision→Draft→Approval；写文件前问"可否写入 [路径]"；多文件改动需整套批准；无用户指令不 commit；被跳过的问题不算回答，要重问。

## 3. 代表样本

**样本 A — 会话检查点与"脚本只观察不裁决"**
`github/Claude-Code-Game-Studios/CLAUDE.md`
```
- production/session-state/active.md is the session checkpoint. The file is the
  memory, not the conversation. Read it first after any compaction, crash, or /clear.
- Helpers in .claude/scripts/ emit observations, never verdicts. A script that scores
  or judges will eventually contradict a mode or override it cannot see.
Collaboration: Question -> Options -> Decision -> Draft -> Approval.
Agents MUST ask "May I write this to [filepath]?" before Write/Edit.
A skipped or dismissed question is not an answer: act on nothing, re-ask and wait.
```

**样本 B — 测试证据分级表**
`github/Claude-Code-Game-Studios/.claude/docs/coding-standards.md`
```
| Story Type | Required Evidence | Location | Default Gate |
| Logic (formulas, AI, FSM) | Automated unit test — must pass | tests/unit/ | BLOCKING |
| Integration | Integration test OR documented playtest | tests/integration/ | BLOCKING |
| Visual/Feel | Retained screenshot + lead sign-off | production/qa/evidence/ | BLOCKING |
| UI | Retained screenshot of each screen touched | production/qa/evidence/ | BLOCKING |
| Config/Data | Smoke check pass | production/qa/smoke-[date].md | ADVISORY |
"Retained" is load-bearing — a screenshot captured and discarded is an assertion,
not evidence.
```

**样本 C — 受控裁决词格式**
`github/Claude-Code-Game-Studios/.claude/agents/art-director.md`
```
Begin your response with the verdict token on its own line:
[GATE-ID]: [a word from that gate's Verdicts line, or NOT ASSESSED]
e.g. AD-ART-BIBLE: APPROVE / AD-CONCEPT-VISUAL: STRONG / AD-PHASE-GATE: NOT READY.
Gates do not share one vocabulary; a word from another gate's list is a wrong answer.
Then provide full rationale below the verdict line. Never bury the verdict in paragraphs.
```

**样本 D — 里程碑工作流与文档所有权**
`github/game-dev-skills/gat/.agents/references/gat-workflow.md`
```
Main Flow: brainstorm -> story -> milestone -> design {milestone} {system} -> engineering
Loop A: refine globals (overview/* + narrative/*). Loop B: iterate milestones.
- designer owns overview/game.md, systems-index.md, per-milestone system GDD + data.
- writer owns narrative/* (global, not per milestone).
- planner owns milestone.md + m{N}-brief.md. artist owns art-direction + system art.
Milestone status: planned -> designing -> designed. Engineering execution is downstream.
```

**样本 E — 观察式验收清单（无人值守）**
`github/playcanvas-prompts/blaster-range/prompt.md`
```
## Acceptance
The game is done when every item below has been OBSERVED in the running game this
session — via browser input, screenshots, console, or runtime-query evidence — not
inferred from source.
- [ ] Boots clean; no console/runtime errors across a full session.
- [ ] WASD accelerates to 7 (10 sprint); slide at 13.5 with dust and 1s cooldown...
- [ ] Numeric grounding: runtime query shows every aabb.min.y within ±0.05 of floor.
Finish with a short report: what works (with evidence), where tuning constants live,
and any item you could not verify — stated honestly, not glossed.
```

**样本 F — 用户样本的里程碑与交付验收**
`../游戏Spec01.md`
```
M0 设计 → M1 可玩原型 → M2 系统完整 → M3 内容 → M4 音频与菜单 → M5 打磨。
每个里程碑结束时运行测试并提交到 git；PROGRESS.md 持续记录进度、已知问题和解决情况。
测试：为战斗公式/寻路/WT 排序/存档读档写自动化测试；做自动通关或关卡跳转调试功能，
逐关验证能胜/能败/能进下一关，确认两条路线和所有结局都能到达。
验收清单：HD-2D / 8 种地形 / WT / 6+2 职业 / 8 角色 / 5 关 / 原创剧情≥15 分钟 /
8 BGM+25 SFX / 菜单设置 / 全简体中文无缺字 / 素材合规 / 从新游戏到结局完整通关。
```

## 4. 可复用模板骨架

```markdown
# 游戏开发工作流与验收：<项目名>
## 1. 阶段管线与门
- 阶段：Concept → Systems Design → Technical Setup → Pre-Production → Production → Polish → Release。
- 每阶段 formal gate（rigor: minimal 无门 / standard / full）；通过才前进。
- 里程碑（示例）：M0 设计 → M1 可玩原型 → M2 系统完整 → M3 内容 → M4 音频菜单 → M5 打磨发布。
- 每里程碑结束：实际运行 + 测试 + 修复 + 提交 git；PROGRESS.md 记录进度/已知问题/解决。

## 2. 编排与协作协议
- 流程：Question → Options(2–4，带理由与推荐) → Decision → Draft → Approval。
- 写文件前问"可否写入 [路径]？"；多文件改动整套批准；无指令不 commit。
- 被跳过/驳回的问题不算回答：重问并等待。
- 自动化模式：collaborative（总问）/ guided（只问重大）/ autonomous（记录并推进）。

## 3. 会话状态与恢复
- production/session-state/active.md 是检查点（文件即记忆）；compaction/crash/clear 后先读它。
- 增量写：先建骨架→逐节起草→批准→写入→每节追加 active.md（任务/已完成/决策/下一节）。
- 恢复：读 active.md + 目标文件，从下一个未完成节继续，不重谈已完成节。
- 脚本只 emit observations，never verdicts。

## 4. 裁决门（gate）
- 首行单独给 verdict token：`[GATE-ID]: <该 gate Verdicts 行内的词 | NOT ASSESSED>`。
- 不同 gate 不共享词表；用错词即错答；rationale 写在 token 下方，不埋进段落。
- 输入缺失 → NOT ASSESSED — NO DATA，点名缺什么、由哪个环节产出（这是成功，不是失败）。
- 缺失工件是 finding（NOT READY/CONCERNS），不是 missing input。

## 5. 测试证据分级（按故事类型）
| 类型 | 证据 | 位置 | 门 |
| Logic | 自动化单元测试通过 | tests/unit/ | BLOCKING |
| Integration | 集成测试或记录试玩 | tests/integration/ | BLOCKING |
| Visual/Feel | 保留截图 + lead 签核 | production/qa/evidence/ | BLOCKING |
| UI | 每屏保留截图 | production/qa/evidence/ | BLOCKING |
| Config/Data | smoke check 通过 | production/qa/smoke-*.md | ADVISORY |
- "保留"= 关闭故事时截图仍在磁盘；拍完即弃只是断言。
- 确定性测试（无随机种子/时间依赖）、隔离（自带 setup/teardown）、fixture 不硬编码。

## 6. 最终验收清单（可观察，逐条核对）
- 每项必须 OBSERVED（运行/截图/console/runtime 查询），非读源码推断。
- 覆盖：核心玩法 / 内容量 / 美术风格 / 音频 / 菜单设置 / 本地化无缺字 / 素材合规 / 完整通关无卡关。
- 自动通关/关卡跳转调试：逐关验证能胜/能败/能进下一关；所有路线与结局可达。
- 收尾报告：什么 works（带证据）、tuning 常量在哪、哪些未能验证（诚实陈述，不粉饰）。
```

## 5. 差异与注意点

- **协作 vs 无人值守**：CCGS 默认 collaborative（写前批准），playcanvas/Spec01 是无人值守（自主决策、别停、把假设写报告）。引擎组装前必须确定模式——这决定"问还是不问""commit 还是不 commit"。
- **rigor 可配，别一刀切**：CCGS 的 gate/测试/GDD 节数都随 `modes.workflow`（minimal/standard/full）降档；minimal 甚至不写 GDD、 waive 测试但"look 不 waive"。模板应支持降档。
- **裁决词表是 per-gate 的**：art-director 强调"不同 gate 不共享词表，用别 gate 的词是错答"。引擎若做自动分支，必须按各 gate 自己的 Verdicts 行解析，不能统一映射。
- **GAT 只管预制作**：gat 明确"技术设计/实现任务分解/验证计划/代码改动在 GAT 之外"，里程碑状态止于 designed；下游工程接 OpenSpec/Superpowers/SpecKit。别把 GAT 当端到端实现工作流。
- **许可证**：CCGS **MIT**；game-dev-skills **未声明**（引用有风险）；playcanvas-prompts **未声明**；`../游戏Spec01.md` 用户自有。
- **证据留存是游戏特有硬约束**：Visual/UI 之所以 BLOCKING，是因为"对游戏而言看起来怎样就是产品本身"，advisory 视觉门会被推迟，结果"测试良好但看起来不对"。这是游戏验收区别于通用软件测试的关键。

## 6. 缺口

- 缺少**引擎路线（Godot/Unity/Unreal）的端到端工作流验证**——CCGS README 自述 engine route 与"用 GDD 深化单系统"尚未像 one-shot 那样被实测。
- 缺少**CI/CD 与自动构建发布**的游戏管线提示词（打包、平台提交、day-one patch 自动化）——CCGS 有 release-manager/devops agent 但本批未精读其正文。
- 缺少**playtest 协议与问卷设计**的成品模板（qa-lead 提"设计试玩协议、创建问卷、分析反馈"，但无具体骨架）。
- 缺少**多 agent 冲突仲裁**的提示词（当两个 specialist 裁决矛盾时如何 escalate 到 creative-director/producer 并记录 trade-off）。
- 缺少**回归/性能 soak 测试**的自动化提示词成品（CCGS 有 regression-suite/soak-test/perf-profile skill，本批未含正文）。