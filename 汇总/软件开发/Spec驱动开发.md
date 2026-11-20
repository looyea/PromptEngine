# 软件开发 · Spec 驱动开发

> 一级类 S（软件开发）· 二级维度「开发规约」+「工作流/自动化」+「产品设计/PRD」
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md`、`github/playcanvas-prompts/` 为游戏侧 spec 标杆。

## 1. 概述

「Spec 驱动开发（SDD）」解决的是：把**规格（spec）当作唯一真相源**，让代码成为规格的表达/生成物，而非反过来。它用一套结构化提示词管线（specify→clarify→plan→tasks→implement→analyze）把模糊想法逐步精化为可执行规格、技术计划、任务清单，再驱动 AI 实现，并持续做一致性校验。这与本项目"提示词工程引擎按需组装"的目标高度契合。

素材里的三种做法：

- **GitHub 官方 spec-kit**（MIT）：完整方法论（`spec-driven.md`）+ 模板（spec/plan/tasks/constitution/checklist）+ 斜杠命令提示词（specify/clarify/plan/tasks/analyze/implement/constitution/checklist/converge）。核心是"规格→计划→任务"三段式 + 项目宪章（constitution）作治理约束 + 质量门。
- **Kiro Spec 工作流**（system-prompts 仓库，GPL/厂商版权）：requirements（EARS 格式）→ design → tasks 三阶段，每阶段**必须用户显式批准才前进**，用 MUST/SHOULD/MAY 约束模型行为。
- **整游戏 mega-spec**（`../游戏Spec01.md`、playcanvas-prompts）：把整个游戏写成一条超长规格提示词（角色+原则+技术选型+玩法+剧情+关卡+音频+菜单+里程碑+验收清单），一次性无人值守生成。是"单条 mega-spec"路线，与 spec-kit 的"多阶段渐进"路线形成对照。

## 2. 共性结构与模式

归纳出 SDD 提示词管线的通用骨架：

1. **权力反转（哲学）**：规格不是服务于代码的脚手架，而是生成代码的源；维护软件 = 演进规格；调试 = 修正规格与其实现计划；重构 = 为清晰而重结构化（spec-driven.md "The Power Inversion"）。
2. **三段式管线 + 治理层**：specify（想法→规格）→ plan（规格→技术计划）→ tasks（计划→任务清单）→ implement（任务→代码）；constitution（项目宪章）作为贯穿的治理约束，plan 阶段有 Constitution Check 门。
3. **用户故事优先级 + 独立可测**：spec-template 要求每个用户故事标 P1/P2/P3，且**独立可测**——只实现一个也是 viable MVP；验收场景用 Given/When/Then。
4. **澄清有限度**：specify 对 unclear 方面"基于上下文与行业标准做合理猜测"，只有"显著影响范围/体验、多种合理解释、无合理默认"时才标 `[NEEDS CLARIFICATION]`，**总数 ≤3**，按 scope>security>UX>technical 排序；clarify 命令最多问 5 个高针对性问题并把答案编码回规格。
5. **可测的功能需求 + 技术无关的成功标准**：每条需求可测；成功标准可量化（时间/性能/量）+ 定性（满意度/任务完成），且**无需实现细节即可验证**。
6. **EARS 验收语法（Kiro）**：`WHEN [event] THEN [system] SHALL [response]` / `IF [precondition] THEN ... SHALL ...`；用户故事 `As a [role], I want [feature], so that [benefit]`。
7. **逐阶段显式批准（Kiro）**：每阶段文档迭代后必须问"看起来好吗？可以进入下一步吗？"，未获明确批准（yes/approved/looks good）不前进；MUST/SHOULD/MAY 分级约束。
8. **任务按用户故事分组 + 并行标记**：tasks-template 用 `[ID] [P?] [Story] Description`，[P] 表示可并行（不同文件无依赖），含精确文件路径；分 Setup→Foundational（阻塞前置）→各用户故事阶段，每个故事可独立实现/测试/交付为 MVP 增量。
9. **质量门 + 检查清单语义**：implement 前扫 checklists 状态，有未勾选项则停下问用户；`[x]` 表示"评审者确认需求质量 criterion 满足"，**不表示实现完成**；implement 只读检查清单作门、不改标记。
10. **一致性持续校验**：analyze 命令跨 spec/plan/tasks 查歧义/矛盾/缺口，不是一次性门而是持续精化。
11. **mega-spec 的验收清单**：playcanvas/Spec01 把验收写成"必须在运行中 OBSERVED 的逐项清单"，数值是规范性 shipped tuning，末尾诚实报告未能验证项。

## 3. 代表样本

**样本 A — SDD 权力反转哲学**
`github/spec-kit/spec-driven.md`
```
Spec-Driven Development inverts this power structure. Specifications don't serve
code—code serves specifications. The PRD isn't a guide for implementation; it's the
source that generates implementation.
Maintaining software means evolving specifications. Debugging means fixing
specifications and their implementation plans that generate incorrect code.
The specification becomes the primary artifact. Code becomes its expression.
```

**样本 B — 用户故事优先级 + 独立可测 + Given/When/Then**
`github/spec-kit/templates/spec-template.md`
```
Each user story/journey must be INDEPENDENTLY TESTABLE — if you implement just ONE,
you should still have a viable MVP that delivers value. Assign priorities P1, P2, P3.
### User Story 1 - [Brief Title] (Priority: P1)
**Independent Test**: [how this can be tested independently]
**Acceptance Scenarios**:
1. **Given** [initial state], **When** [action], **Then** [expected outcome]
```

**样本 C — 澄清有限度（≤3 标记）**
`github/spec-kit/templates/commands/specify.md`
```
For unclear aspects: Make informed guesses based on context and industry standards.
Only mark [NEEDS CLARIFICATION: specific question] if: the choice significantly
impacts scope or UX; multiple reasonable interpretations exist; no reasonable default.
LIMIT: Maximum 3 [NEEDS CLARIFICATION] markers total.
Prioritize: scope > security/privacy > user experience > technical details.
Define Success Criteria: measurable, technology-agnostic outcomes, verifiable without
implementation details.
```

**样本 D — EARS 验收 + 逐阶段批准（Kiro）**
`github/system-prompts-and-models-of-ai-tools/Kiro/Spec_Prompt.txt`
```
#### Acceptance Criteria (EARS)
1. WHEN [event] THEN [system] SHALL [response]
2. IF [precondition] THEN [system] SHALL [response]
- The model MUST ask "Do the requirements look good? If so, we can move on to the design."
- The model MUST NOT proceed to the design document until receiving clear approval.
- The model MUST continue the feedback-revision cycle until explicit approval.
```

**样本 E — 任务格式与按故事分组**
`github/spec-kit/templates/tasks-template.md`
```
## Format: [ID] [P?] [Story] Description
- [P]: Can run in parallel (different files, no dependencies)
- [Story]: Which user story this task belongs to (US1, US2...)
- Include exact file paths in descriptions
## Phase 2: Foundational (Blocking Prerequisites)
⚠️ CRITICAL: No user story work can begin until this phase is complete.
Tasks MUST be organized by user story so each can be implemented/tested/delivered
as an MVP increment.
```

**样本 F — 检查清单门语义（implement）**
`github/spec-kit/templates/commands/implement.md`
```
Treat checklist markers as a read-only gate: scan checkbox state, report status, ask
before proceeding; do NOT modify checklist files or markers.
[x] means the reviewer determined the requirements-quality criterion is satisfied;
it does NOT mean implementation work is complete.
If any checklist has unchecked items: STOP and ask "Do you want to proceed anyway?"
```

## 4. 可复用模板骨架

引擎组装 SDD 管线时，可按阶段套用以下骨架（每阶段是一个命令提示词）：

```markdown
# 阶段 0：项目宪章（constitution，治理层，贯穿全程）
## Core Principles（每条：名 + 描述，可标 NON-NEGOTIABLE）
- 例：Library-First / CLI Interface / Test-First(TDD) / Integration Testing / Simplicity(YAGNI)。
## 附加约束（安全/性能/合规） + 开发工作流（评审/质量门/部署审批）
## Governance：宪章高于其他实践；修订需文档+批准+迁移计划；所有 PR 验证合规。
Version / Ratified / Last Amended。

# 阶段 1：specify（想法 → 规格）
- 从描述提取 actors/actions/data/constraints；生成 2–4 词短名；建 specs/<NNN-name>/spec.md。
- 用户故事按 P1/P2/P3 优先级，每个独立可测（只实现一个也是 MVP）；验收 Given/When/Then。
- 功能需求每条可测；成功标准可量化 + 定性、技术无关、无需实现细节即可验证。
- unclear 做合理猜测；仅显著影响/多解/无默认时标 [NEEDS CLARIFICATION]，总数 ≤3。
- 写完后做规格质量校验（歧义/矛盾/缺口）。

# 阶段 2：clarify（可选，精化规格）
- 最多 5 个高针对性问题；把答案编码回规格对应节；更新检查清单。

# 阶段 3：plan（规格 → 技术计划）
- Technical Context：语言/依赖/存储/测试/平台/项目类型/性能目标/约束/规模（不明标 NEEDS CLARIFICATION）。
- Constitution Check 门：Phase 0 研究前必过，Phase 1 设计后复查。
- 产出 research.md / data-model.md / contracts/ / quickstart.md；每个技术选择有理由、每个架构决策追溯到需求。

# 阶段 4：tasks（计划 → 任务清单）
- 格式 [ID] [P?] [Story] Description，含精确文件路径；[P]=可并行。
- 分 Setup → Foundational（阻塞前置，未完成任何故事不得开工）→ 各用户故事阶段。
- 按故事分组，每故事可独立实现/测试/交付为 MVP 增量；测试任务仅在规格明确要求时含。

# 阶段 5：implement（任务 → 代码）
- 先扫 checklists 状态作只读门：有未勾选则停下问用户；[x]=需求质量已评审，非实现完成；不改标记。
- 读 tasks/plan/data-model/contracts/research/constitution/quickstart；验证项目 setup（ignore 文件等）。
- 按任务执行，遵循宪章与编码规约；每故事完成留证据。

# 阶段 6：analyze（持续一致性校验）
- 跨 spec/plan/tasks 查歧义/矛盾/缺口；非一次性门，持续精化。

# 替代路线：单条 mega-spec（无人值守整产品/整游戏）
- 角色 + 工作原则（自主决策/先设计后实现/可验证/完整交付）
- 技术选型（比较≥2 方案，写 DECISIONS.md）+ 各子系统硬规格（数值规范性）
- 里程碑 M0…Mn（每阶段运行测试+提交 git）+ PROGRESS.md
- 末尾验收清单：每项必须运行中 OBSERVED，非读源码推断；诚实报告未能验证项。
```

## 5. 差异与注意点

- **渐进多阶段 vs 单条 mega-spec**：spec-kit/Kiro 是"多阶段、逐段批准、持续校验"，适合团队协作与演进；playcanvas/Spec01 是"一条超长 spec、无人值守、一次生成"，适合复刻标杆或独立交付。引擎应支持两种组装模式，按"是否有人值守/是否需演进"选择。
- **批准模型分歧**：Kiro"每阶段必须显式批准才前进"（MUST NOT proceed until approval）；spec-kit specify"做合理猜测、≤3 澄清标记"偏自主；playcanvas"别停、自主决策"。自主度旋钮须与运行模式一致。
- **检查清单语义易混**：spec-kit 明确 `[x]` = 需求质量已评审，**不是**实现完成；implement 只读不改。引擎若自动勾选会把"评审门"误当"进度门"。
- **EARS vs Given/When/Then**：Kiro 用 EARS（WHEN/IF...SHALL），spec-kit 用 Given/When/Then。两者都是可测验收语法，可互换，但同一管线内应统一。
- **宪章是治理层**：constitution 高于其他实践、plan 阶段有 Constitution Check 门、修订需迁移计划。这是 spec-kit 相对 Kiro 的显著特色，适合有强工程纪律的团队。
- **许可证**：spec-kit **MIT**（GitHub 官方，可自由改用，是引擎最安全的骨架来源）；Kiro 提示词在 system-prompts 仓库（**GPL-3.0 + 厂商版权**，只借鉴 EARS/批准结构，不搬原文）；playcanvas-prompts **未声明**；`../游戏Spec01.md` 用户自有。
- **与 G 类衔接**：游戏侧 spec 标杆（Spec01/playcanvas）的玩法/美术/叙事/音频/验收细节见 `汇总/游戏/` 各篇；本篇提供"spec 管线方法论"骨架，两者组合即可组装游戏 SDD 管线。

## 6. 缺口

- 缺少**analyze/converge 命令的完整正文**提炼（本批 analyze 的 Outline 未含具体校验规则，converge 未精读）——一致性校验的具体检查项需补。
- 缺少**spec 版本管理与分支探索**的提示词（spec-driven.md 提"branching for exploration、从同一 spec 生成多实现"，但无成品命令）。
- 缺少**生产反馈回流规格**的提示词（metrics/incidents→更新 spec→再生成），方法论提到但无模板。
- 缺少**非游戏领域的 mega-spec 成品**（如完整 SaaS/CLI 工具的单条 spec），本批 mega-spec 全是游戏。
- 缺少**spec 与测试的双向追溯**自动化提示词（验收场景→测试用例→实现→回归的链路校验）。
- 缺少**中文 spec 写作规范**（术语统一、需求编号、EARS 中文句式）的本地化范本。