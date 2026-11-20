# 游戏 · 叙事与剧本

> 一级类 G（游戏）· 二级维度「叙事/剧本」（与 W 写作、R 角色扮演共享部分技法）
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「游戏叙事与剧本」解决的是：构建**内部一致的世界观**、有弧光的角色、可分支的剧情结构，以及可落地为对话/任务/lore 的具体文本，并保证**玩法与叙事互相强化**（ludonarrative harmony）。它区别于纯小说写作：叙事要服务于玩家代理（agency）、要可被对话系统/任务系统执行、要与机制对齐。

素材里的三种做法：

- **叙事总监 + 写手 agent**（CCGS `narrative-director`、`writer`）：总监管故事架构、世界构建框架、角色弧光、ludonarrative harmony、对话系统能力、叙事 pacing；写手管对白、lore 条目、物品描述、barks、本地化友好文本，并有严格写作标准（说话人标签、条件注解、变量占位符、行宽限制、不臆造未定义机制）。
- **全局叙事文档模板族**（gat `narrative-world`/`story`/`characters`/`quests`/`dialogue`）：叙事是**全局连贯**的（不按里程碑切分），每份模板用"连贯散文描述 + 结构化字段"双层写法，并互相引用（story→world→characters→quests→dialogue）。
- **整游戏 spec 的剧情硬规格**（`../游戏Spec01.md` 第五节）：直接规定世界观基调、章节结构、关键道德抉择、路线分歧、结局数、篇幅硬指标（≥7000 字/≥15 分钟）、角色构成与每人设定字段、演出要求。

## 2. 共性结构与模式

归纳出游戏叙事提示词的通用骨架：

1. **世界规则内部一致 + 矛盾检查**：narrative-director 的"世界元素文档"必含核心概念、规则（可能/不可能）、历史、与其他元素的联系、玩家相关性、**显式确认与既有 lore 无矛盾**。
2. **角色服务于故事和/或玩法**：每个角色有弧光、动机、关系、voice profile、叙事功能；gat 角色 brief 含 want/need、conflict/stakes、arc、gameplay role、voice、visual identity、defining scene。
3. **ludonarrative harmony（玩法叙事和谐）**：显式标记"故事说一套、玩法奖励另一套"的失调；机制设计意图作为 lore 的输入，而非反过来。
4. **结构 + 分支 + 后果**：故事架构含幕断点、主要 plot beats、分支点、解决路径；玩家选择要写明哪些结局可变、哪些固定，以及选择/失败如何被承认。Spec01 把"关键道德抉择"写成硬约束（决定阵营、同伴离队/入队、后续关卡配置、≥2 结局）。
5. **散文锚定 + 结构化字段双层**：gat 每份模板先"用连贯散文讲清整体（世界/故事/角色群像）"，再分节给可执行字段；避免一上来就堆列表/表格而丢失整体感。
6. **对白可执行 + 本地化友好**：写手标准——每句有说话人标签与上下文注解；玩家选择对白写成树（每个选项→回应→后续分支→每分支条件）；变量用命名占位符 `{player_name}`；行宽 ≤120 字符；避免不可翻译的习语；每句可被配音（自然节奏、清晰情绪方向）。
7. **不臆造未定义机制 / 不违背既定 lore**：描述机制的文本只说设计已定义的；机制未定义就说"未定义"、向 game-designer 要定义、给 flagged 临时占位，绝不发明时长/效果。请求与既定 lore 矛盾时不照写，标记给 narrative-director 裁决 canon，并给 lore 一致的替代。
8. **篇幅/时长可验证**：Spec01 要求统计剧情字数并估算时长、STORY.md 每场景标注地点/出场角色/BGM/镜头走位/音效/估算时长、末尾附总时长表。

## 3. 代表样本

**样本 A — 世界构建标准（含矛盾检查）**
`github/Claude-Code-Game-Studios/.claude/agents/narrative-director.md`
```
Every world element document must include:
- Core Concept: one-sentence summary
- Rules: what is possible and impossible
- History: key historical events that shaped the current state
- Connections: how this element relates to other world elements
- Player Relevance: how the player interacts with or is affected by this
- Contradictions Check: explicit confirmation of no contradictions with existing lore
```

**样本 B — 写手的对白与"不臆造机制"标准**
`github/Claude-Code-Game-Studios/.claude/agents/writer.md`
```
- Player-choice dialogue is written as a tree: each player option, the response,
  and any follow-up branches, with the condition for every branch.
- Text that describes a mechanic states only what its design defines. If undefined,
  say so, ask game-designer, offer a flagged temporary placeholder — never invent
  durations or effects.
- When a request contradicts established lore, do not write it as given: flag to
  narrative-director, who rules on canon, and offer a lore-consistent alternative.
- All variable insertions use named placeholders: {player_name}, {item_count}.
- No line should exceed 120 characters for readability in dialogue boxes.
```

**样本 C — 故事模板：叙事意图 + 转折 + 代理**
`github/game-dev-skills/gat/.claude/docs/templates/design/narrative-story.md`
```
## 3. Narrative Intent
- Story promise / Player role / Emotional aim / Boundary (what this story will NOT attempt)
## 6. Story Progression — ### [Turning Point]
- Player-facing event / Player action or choice / Dramatic change / Delivery
## 7. Delivery And Agency
- Delivery channels / Player choices (which outcomes can change, which fixed) /
  Consequences (how choices and failure are acknowledged)
```

**样本 D — 角色 brief 字段**
`github/game-dev-skills/gat/.claude/docs/templates/design/narrative-characters.md`
```
### [Character Name]
- Want and need / Conflict and stakes / Arc
- Gameplay role: what player interaction they support
- Voice: vocabulary, rhythm, or a short sample line
- Visual identity: design cues linked to personality or role
- Defining scene: a moment that reveals the character
```

**样本 E — 用户样本的关键抉择与篇幅硬指标**
`../游戏Spec01.md`
```
关键抉择（必须有）：第 2 或第 3 章一个无法两全的道德抉择……这个选择要决定：
- 主角阵营倾向（秩序线或混沌线）
- 至少 1 名同伴离队或反目，至少 1 名不同角色入队
- 后续至少 1 关的敌我配置或对话不同
- 至少 2 种不同结局
篇幅（硬性）：剧情文本合计不少于 7000 字，每条路线单独计算；纯剧情不少于 15 分钟。
STORY.md 每场景标注地点、出场角色、BGM、镜头和走位、音效、估算时长，末尾附总时长表。
```

**样本 F — 世界规则模板（每条规则挂玩家证据与设计后果）**
`github/game-dev-skills/gat/.claude/docs/templates/design/narrative-world.md`
```
## 4. World Rules — ### [Rule]
- Effect on life or conflict: what this rule changes
- Player-facing evidence: how the player discovers or tests it
- Design consequence: what story, gameplay, or art must respect
## 5. History And Present Conflict
- Formative event: past event and its present consequence
- Visible trace: how the player encounters that consequence
```

## 4. 可复用模板骨架

```markdown
# 叙事设计：<游戏名>（全局连贯，不按里程碑切分）
## 1. 故事（story.md）
- 摘要： central conflict、玩家角色与 stakes、情绪旅程（2–3 句）。
- 故事散文：玩家如何被卷入冲突、重大事件为何彼此因果、选择与关系如何导向结局。
- 叙事意图：story promise / player role / emotional aim / boundary（不尝试什么）。
- 前提：setting & conflict / inciting event / goal & stakes。
- 主题与基调：主题如何由玩家行动/后果表达 / 基调与转变 / 参考取舍。
- 故事推进（每个转折）：player-facing event / player action or choice / dramatic change / delivery。
- 交付与代理：交付渠道（对白/环境/目标）/ 哪些结局可变哪些固定 / 选择与失败如何被承认。

## 2. 世界（world.md）
- 世界散文（代表地点/情境锚定）+ 快照（place & era / distinctive premise / player understanding）。
- 世界规则（每条）：对生活/冲突的影响 / 玩家-facing 证据 / 设计后果（故事玩法美术须尊重）。
- 历史与当下冲突 / 势力与权力。
- 矛盾检查：显式确认与既有 lore 无矛盾。

## 3. 角色（characters.md，每人）
- want & need / conflict & stakes / arc / gameplay role / voice（词汇节奏 + 样句）/
  visual identity / defining scene。
- 关系变化：起始关系 → 张力来源 → 有意义的改变。

## 4. 任务（quests.md，每条）
- purpose & stakes / entry（虚构中为何可用）/ objective & player actions / people & places /
  分支与后果 / 失败或放弃如何回应。

## 5. 对白（dialogue.md）
- voice guide（每角色：词汇/句式/节奏/避免什么 + 样句）。
- 会话设计：pattern / player agency / repetition 如何保持有意义。
- 玩家选择对白写成树：选项 → 回应 → 后续分支 → 每分支条件。
- barks 与目标文本：情境 → 台词 → 传达什么。
- 工程约束：说话人标签 + 上下文注解；变量命名占位符 {player_name}；行宽 ≤120；
  本地化友好（避免不可翻译习语）；可配音（自然节奏 + 情绪方向）。

## 6. 对齐与验证
- ludonarrative harmony：标记故事与玩法奖励的失调；机制意图作为 lore 输入。
- 不臆造未定义机制；不违背既定 lore（矛盾交叙事总监裁决 canon）。
- 篇幅/时长：统计字数、估算时长；每场景标地点/角色/BGM/镜头走位/音效/时长，附总时长表。
```

## 5. 差异与注意点

- **全局 vs 里程碑**：gat 明确"叙事是全局连贯的，不按里程碑切分"（与系统 GDD 按里程碑切分相反）；引擎编排时叙事文档应一次性建好、跨里程碑引用，而非每个里程碑重写。
- **总监 vs 写手分工**：narrative-director 管架构/世界/角色弧光/和谐/系统能力，**不写最终叙事内容**；writer 写具体文本但**不裁决 canon、不定义机制**。越界会导致设定漂移。
- **散文锚定是 gat 的显著特色**：每份模板第 2 节强制"连贯散文、不用小标题/列表/表格"，先建立整体感再给字段。这与"一上来就堆结构化字段"的写法分歧明显，适合需要情绪连贯的叙事，不适合纯数据表。
- **篇幅硬指标可验证**：Spec01 把"≥7000 字/≥15 分钟/每路线单独计"写成可统计验收项，比"剧情丰富"这类模糊要求强得多。
- **许可证**：CCGS **MIT**；game-dev-skills **未声明**（引用有风险，借鉴字段骨架为宜）；`../游戏Spec01.md` 用户自有，且明确"主线原创、架构参考《皇家骑士团》、禁用其角色名/地名/台词/旋律"。
- **与 R/W 类的衔接**：本篇侧重"游戏叙事工程"（分支树、条件注解、机制对齐、本地化）；纯角色人格模拟见 R 类，纯小说/文案技法见 W 类。

## 6. 缺口

- 缺少**对话系统/任务系统的技术实现规格**（状态跟踪、条件检查、变量插入的引擎侧 schema）——narrative-director 提"与 lead-programmer 协作定义对话系统能力"，但本批无成品技术模板。
- 缺少**环境叙事（environmental storytelling）**的专门提示词（level-designer 提了"用环境讲故事"，但无独立骨架）。
- 缺少**多结局/分支图的可视化与一致性校验**提示词（如何保证所有分支可达、无死锁、标记正确）。
- 缺少**中文叙事专门规范**（文言/白话比例、称谓体系、本地化占位符与汉字字体缺字）的成体系范本，Spec01 有零散要求。
- 缺少**叙事 pacing 与玩法节奏的量化对齐**模板（多少分钟一个 beat、 exposition/action/mystery/revelation 的配比）。