# 游戏 · 开发规约

> 一级类 G（游戏）· 二级维度「开发规约」
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。
> 本篇是归纳提炼，非原文搬运；摘录每段 ≤20 行，模板骨架供引擎组装。

## 1. 概述

「游戏开发规约」解决的是：在让 AI 动手写游戏代码之前，先约定**它能做什么、不能做什么、代码写到哪些文件、按什么风格、怎样才算验收通过**。它是游戏项目里最先加载、贯穿全程的"宪法层"提示词，与具体玩法/美术/叙事内容解耦。

素材里可归纳出三种流派：

- **引擎/语言绑定规约**（awesome-cursorrules）：以 `.mdc` frontmatter（`description`/`globs`/`alwaysApply`）声明触发范围，正文是该引擎的编码纪律。代表 GameMaker GML、DragonRuby、Unity C#、Blender Python、ASCII 模拟游戏。颗粒度从"通用最佳实践"到"某个具体项目的上下文"不等。
- **工作室级总规约**（Claude-Code-Game-Studios，简称 CCGS）：`CLAUDE.md` + `.claude/docs/coding-standards.md` 把编码标准、设计文档标准、测试证据标准、协作协议、上下文管理写成一套可配置（`project.yaml`）的体系，强调"数据驱动、可验证、留证据"。
- **整游戏 spec 内嵌工作规则**（playcanvas-prompts、`../游戏Spec01.md`）：把规约作为超长规格提示词的开头几节（Working Rules / 工作原则），与具体数值、文件结构、验收清单绑在一起，面向"无人值守一次性生成"。

## 2. 共性结构与模式

跨素材归纳，游戏开发规约普遍遵循「**角色 → 边界 → 文件/命名约定 → 编码纪律 → 验收**」骨架：

1. **角色与项目上下文**：一句话定位（"你是 DragonRuby 专家"/"全栈独立游戏开发者"），必要时给出引擎版本、语言、当前项目状态。
2. **能做/不能做（边界）**：最稳定的共性。CCGS 每个 agent 都有 `What This Agent Must NOT Do`；playcanvas 明确"只建 spec 描述的，不加额外功能/抽象/防御性代码"；Spec01 要求"不留 TODO、占位图块、点进去没反应的菜单"。
3. **文件与命名约定**：把代码/资产写到哪、怎么命名。playcanvas 给出 `src/` 一文件一系统的清单；CCGS 规定资产命名 `[category]_[name]_[variant]_[size].[ext]`；GML 规约要求脚本/对象/精灵/房间统一前缀。
4. **编码纪律**：数据驱动（数值外置不硬编码）、状态机、输入与执行分离、性能热路径、及时释放资源、匹配既有代码风格。
5. **验收/可验证**：CCGS"parse check 不等于 run"——每个玩家可观察的改动都要实际运行并留截图证据；playcanvas"claims 必须来自观察运行中的游戏，而非读源码"；Spec01"每阶段实际运行测试修复"。
6. **协作/自主模式开关**：CCGS 区分 collaborative/guided/autonomous；playcanvas/Spec01 是"无人值守、自主决策、把假设写进报告"。这是游戏规约相对通用编码规约的显著分歧点（见 §5）。

## 3. 代表样本

**样本 A — 引擎规约的 frontmatter + 职责分离（GameMaker GML）**
`github/awesome-cursorrules/rules/gamemaker-gml.mdc`
```
globs: ["**/*.gml", "**/*.yy", "**/*.yyp"]
## Gameplay Architecture
- Use finite state machines for player, enemy, UI, and game-flow states.
- Keep collision logic explicit and deterministic.
- Separate input collection from action execution.
- Store save data through structured maps/structs and version the save format.
## Common Mistakes
- Do not put game logic in Draw events.
- Do not create data structures without destroying them.
- Do not hardcode magic numeric state IDs.
```

**样本 B — 极简反过度工程边界**
`github/awesome-cursorrules/rules/anti-overengineering.mdc`
```
Only change what was asked. Simplest solution first. When unsure, ask.
Do not modify unrequested code, add abstractions without a concrete need,
import unnecessary dependencies, rewrite entire files for small changes,
or add error handling for impossible scenarios.
Before delivery: verify you only changed requested code, check for simpler
approaches, confirm no unrequested files were touched.
```

**样本 C — 工作室级"可验证 + 留证据"标准**
`github/Claude-Code-Game-Studios/.claude/docs/coding-standards.md`
```
- Gameplay values must be data-driven (external config), never hardcoded
- All public methods must be unit-testable (dependency injection over singletons)
- Commits must reference the relevant design document or task ID
- Verification-driven development: Write tests first when adding gameplay systems.
  For UI changes, verify with screenshots.
- A parse check is not a run. Every story that changes something player-observable
  is launched and observed before it closes, and the observation is a retained
  screenshot in production/qa/evidence/.
```

**样本 D — 无人值守整游戏 spec 的工作规则**
`github/playcanvas-prompts/blaster-range/prompt.md`
```
## Working Rules
- You are running unattended; no one can answer questions. Decide, proceed, and
  note assumptions in your final report. Never stop at uncertainty, never end
  with only a plan.
- Every requirement below is hard. Numeric values are NOT suggestions — they are
  the shipped tuning; transcribe every row and value exactly into src/constants.ts.
- Build exactly what this spec describes: no extra features, abstractions, or
  defensive code.
- Do not create git commits; leave version control untouched.
```

**样本 E — 用户样本的工作原则（自主决策 + 完整交付）**
`../游戏Spec01.md`
```
1. 自主决策：引擎、工具、美术管线和素材来源都由你决定……遇到不确定的地方，
   自己做出合理选择并写进 DECISIONS.md，不要停下来等人确认。
2. 先设计后实现：写代码之前，先完成设计文档（GDD）、完整剧本和素材清单。
3. 可验证：每个阶段结束都要实际运行、测试并修复问题。
4. 完整交付：成品里不能留下 TODO、占位图块、占位文字，也不能有点进去没反应的菜单项。
```

**样本 F — 一文件一系统的文件结构约定**
`github/playcanvas-prompts/blaster-range/prompt.md`
```
Modular TypeScript in src/, one file per system:
src/app.ts        wiring only: app boot, asset load, arena build, game start
src/constants.ts  ALL gameplay tuning (MOVE, CAM, GUN, BOT, PLAYER, ARENA, ASSET_TUNING)
src/collision.ts  AABB math: groundY heightfield, moveBox collide-and-slide, ray tests
src/player.ts     first-person controller (yaw body / pitch head), slide, crouch, shake
src/game.ts       state machine + per-frame orchestration
Name visible gameplay entities by role (Enemy_3, WeaponRifle, Wall_N_4) so runtime
queries stay readable. Expose the app as (window as any).app for devtools verification.
```

## 4. 可复用模板骨架

引擎组装游戏开发规约时，可套用以下骨架（按需裁剪 §协作模式）：

```markdown
---
description: "<引擎/语言> 游戏开发规约"
globs: ["<触发文件模式，如 **/*.gd / **/*.cs / **/*.gml>"]
alwaysApply: <true|false>
---
# 角色与项目上下文
你是 <引擎> 游戏开发专家。引擎版本 <X>，语言 <Y>，项目当前状态 <一句话>。

# 边界（能做 / 不能做）
- 只实现被要求的内容；最简方案优先；不确定时 <提问 | 自主决策并记录到 DECISIONS.md>。
- 禁止：改动无关代码、无具体需求的抽象、引入未用依赖、为不可能场景写错误处理、
  留下 TODO/占位资源/无响应菜单。
- 禁止使用任何商业游戏的原始素材/角色名/台词/旋律，只借鉴系统与风格。

# 文件与命名约定
- 代码组织：<一文件一系统 / 模块划分>，关键目录 <src/...>。
- 资产命名：`[category]_[name]_[variant]_[size].[ext]`。
- 数值外置：所有可调数值写入 <assets/data/ | src/constants.ts>，不硬编码。

# 编码纪律
- 用有限状态机管理玩家/敌人/UI/流程；输入采集与动作执行分离。
- 逻辑放 Update/Step，渲染放 Draw；碰撞确定、可复现。
- 热路径保持简单；频繁对象用池化；数据结构用完即释放。
- 匹配既有代码风格与库；新组件先看现有组件怎么写。

# 验收（可验证 + 留证据）
- 每个玩家可观察的改动：实际运行 + 截图/日志证据，存 <production/qa/evidence/>。
- 逻辑/公式/寻路/存档：写自动化测试，确定性、可隔离。
- "能解析"不等于"能运行"；交付前逐条核对验收清单。

# 协作模式（二选一）
- 协作式：写文件前先问"可否写入 [路径]？"，先给草稿再批准。
- 无人值守：自主决策、把假设写进最终报告，不停下来等确认。
```

## 5. 差异与注意点

- **自主 vs 协作的根本分歧**：CCGS 默认"用户驱动、写文件前必须批准"（collaborative），而 playcanvas/Spec01 是"无人值守、自主决策、别停"。引擎组装时**必须先确定运行模式**，两套边界互斥，混用会导致 agent 要么频繁打断要么擅自行动。
- **颗粒度差异**：Unity C# 规约（`unity-cursor-ai-c-cursorrules-prompt-file.mdc`）其实是"某个 Ring-Con 塔防项目"的上下文，并非通用 Unity 最佳实践，直接复用会带入无关项目假设；DragonRuby 规约结尾误植了 Rails 的"routing/controllers/models/views"措辞（疑似模板复制残留），引用时需剔除。GML/Blender 规约则是较干净的通用引擎纪律。
- **许可证**：awesome-cursorrules 为 **CC0 1.0**（可自由改用）；CCGS 为 **MIT**（需保留版权声明）；playcanvas-prompts **未声明许可证**（PlayCanvas 官方组织出品，引擎本体 MIT，但 prompts 仓库无 LICENSE，引用/再分发有风险，建议只借鉴结构）；`../游戏Spec01.md` 为用户自有样本。
- **数值"规范性"强度**：playcanvas 把数值视为"shipped tuning，逐行精确转录"，Spec01 把数值视为"建议范围、由你决定"。前者适合复刻标杆，后者适合原创开发——模板里"数值外置"条款的措辞强度应据此调整。

## 6. 缺口

- 素材偏 **Web/Three.js/PlayCanvas/Godot/GameMaker/DragonRuby**，缺少 Unreal C++/Blueprint、Unity 通用（非项目特定）的成体系编码规约。
- 缺少**主机/移动端性能预算**（帧率目标、内存上限、draw call 预算）写进规约的范本；CCGS 有 performance-analyst 但规约层未给硬指标模板。
- 缺少**网络/多人同步**的开发规约（确定性、回滚、权威划分）——game-architect 的 references 有多人设计知识，但未沉淀成"规约提示词"。
- 缺少**中文/本地化工程规约**（字体缺字扫描、文本外置、占位符规范）的独立范本，Spec01 有零散要求但未成体系。