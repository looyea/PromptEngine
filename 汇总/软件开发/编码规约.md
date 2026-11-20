# 软件开发 · 编码规约

> 一级类 S（软件开发）· 二级维度「开发规约」+「代码生成/测试/调试」
> 素材路径相对 `素材/`。与 G 类「开发规约」共享通用工程纪律，本篇侧重通用软件（非游戏特定）。

## 1. 概述

「编码规约」解决的是：约束 AI 编码助手**写出什么样的代码、改多少、怎么命名、怎么注释、怎么验证、怎么与用户沟通**。它是加载在 coding agent 之上、跨项目复用的"代码纪律层"，与具体业务逻辑解耦。

素材里的三种来源：

- **社区规约合集**（awesome-cursorrules，257 个 `.mdc`）：以 frontmatter（`description`/`globs`/`alwaysApply`）声明触发范围，正文分通用工程纪律（clean-code、codequality、anti-overengineering、code-style-consistency、DRY/SOLID、git-conventional-commit）与框架/语言特定规约（FastAPI、Spring Boot、React、Go、Rust 等）。
- **商业 coding agent 系统提示词里的代码风格节**（system-prompts-and-models-of-ai-tools、leaked-system-prompts）：Cursor 的 `<code_style>`/`<making_code_changes>`/`<linter_errors>`、Devin 的 Coding Best Practices、Claude Code 的 important-instruction-reminders、Codex 的技术沟通风格。这些是工业级、经过大规模实战的规约写法。
- **极简编辑协议**（leaked-system-prompts 的 aider）：不驱动工具调用，而是解析回复里的 edit block，规约核心是"整文件输出格式 + 绝不省略内容"。

## 2. 共性结构与模式

归纳出编码规约提示词的通用骨架（「角色 → 边界 → 风格 → 验证 → 沟通」）：

1. **角色定位**：一句话（"你是专家软件开发者"/"expert game developer in Ruby"），必要时给语言/框架/版本上下文。
2. **改动边界（最稳定共性）**：只改被要求的；最简方案优先；不改无关代码、不加无需求抽象、不引未用依赖、不为小改动重写整文件、不为不可能场景写错误处理（anti-overengineering）；"Do what has been asked; nothing more, nothing less"（Claude Code）；优先编辑既有文件而非新建（Claude Code）。
3. **命名与可读性**：描述性命名揭示意图（clean-code）；避免 1–2 字符名、函数用动词/变量用名词、全词优于缩写（Cursor code_style）；用命名常量替代魔法数；用变量捕获复杂条件的含义。
4. **控制流与注释**：guard clause/早返回、先处理错误与边界、避免深嵌套（>2–3 层）、不无意义 try/catch、绝不 catch 而不处理（Cursor）；注释解释"why"而非"what"、不为显然代码加注释、避免 TODO（实现它）、不用行内注释（Cursor）；Devin 反向——"除非用户要求或代码复杂，否则不加注释"。
5. **匹配既有风格**：改文件前先理解其约定，模仿代码风格、用既有库与工具、遵循既有模式（Devin）；不重排无关代码格式（Cursor）；新组件先看现有组件怎么写。
6. **依赖与可运行**：绝不假设某库可用，先查 package.json/cargo.toml/邻近文件（Devin）；加全 import/依赖/端点使代码可立即运行；从零建项目要有依赖管理文件 + README（Cursor）。
7. **验证与 lint**：改动不引入 linter 错误，改完读 lint；修 lint 不瞎猜、不牺牲类型安全、**同一文件修 lint 不超过 3 次循环**，第 3 次停下问用户（Cursor）；提交前跑 lint/单测（Devin）；测试失败先怀疑被测代码而非改测试（Devin）。
8. **沟通纪律**：不输出代码给用户除非被要求，用编辑工具实现（Cursor）；简洁、无前后废话、不道歉、不总结改动、不发明额外改动（codequality/Claude Code）；引用代码用 `file_path:line_number`（Claude Code）。
9. **触发范围声明**：`.mdc` frontmatter 的 `globs` 决定规约何时生效（如 `**/*.gml`、`**/*.py`），`alwaysApply` 决定是否常驻。

## 3. 代表样本

**样本 A — 反过度工程边界**
`github/awesome-cursorrules/rules/anti-overengineering.mdc`
```
Only change what was asked. Simplest solution first. When unsure, ask.
Do not modify unrequested code, add abstractions without a concrete need, import
unnecessary dependencies, rewrite entire files for small changes, or add error
handling for impossible scenarios.
Before delivery: verify you only changed requested code, check for simpler
approaches, confirm no unrequested files were touched.
```

**样本 B — 工业级代码风格（Cursor）**
`github/system-prompts-and-models-of-ai-tools/Cursor Prompts/Agent Prompt 2025-09-03.txt`
```
<code_style>
Naming: Never use 1-2 character names. Functions verbs, variables nouns.
  genYmdStr → generateDateString; n → numSuccessfulRequests.
Control Flow: guard clauses/early returns; handle error and edge cases first;
  avoid deep nesting beyond 2-3 levels; NEVER catch errors without meaningful handling.
Comments: explain "why" not "how"; never inline comments; avoid TODO — implement instead.
Formatting: match existing style; prefer multi-line over complex ternaries;
  don't reformat unrelated code.
</code_style>
```

**样本 C — lint 循环上限与可运行**
`github/system-prompts-and-models-of-ai-tools/Cursor Prompts/Agent Prompt 2025-09-03.txt`
```
<linter_errors>
Make sure your changes do not introduce linter errors. Run read_lints when done.
Fix them if clear how. Do not make uneducated guesses or compromise type safety.
DO NOT loop more than 3 times on fixing linter errors on the same file. On the third
time, stop and ask the user what to do next.
</linter_errors>
```

**样本 D — 依赖核实与模仿既有约定（Devin）**
`github/system-prompts-and-models-of-ai-tools/Devin AI/Prompt.txt`
```
- NEVER assume that a given library is available, even if well known. First check
  this codebase already uses it (neighboring files, package.json, cargo.toml).
- When making changes to files, first understand the file's code conventions. Mimic
  code style, use existing libraries and utilities, follow existing patterns.
- When struggling to pass tests, never modify the tests themselves unless explicitly
  asked. Always first consider the root cause might be in the code, not the test.
```

**样本 E — 极简改动纪律（Claude Code 提醒）**
`github/system-prompts-and-models-of-ai-tools/Anthropic/Claude Code 2.0.txt`
```
Do what has been asked; nothing more, nothing less.
NEVER create files unless they're absolutely necessary for achieving your goal.
ALWAYS prefer editing an existing file to creating a new one.
NEVER proactively create documentation files (*.md) or README files. Only create
documentation files if explicitly requested by the User.
```

**样本 F — 整文件编辑协议（aider）**
`github/leaked-system-prompts/aider_20260918.md`
```
To suggest changes to a file you MUST return the entire content of the updated file.
Every file listing MUST use this format:
- First line: the filename with any originally provided path; JUST the filename.
- Second line: opening ```
- ... entire content of the file ...
- Final line: closing ```
*NEVER* skip, omit or elide content using "..." or comments like "... rest of code..."!
```

## 4. 可复用模板骨架

```markdown
---
description: "<语言/框架> 编码规约"
globs: ["<触发模式，如 **/*.ts / **/*.py>"]
alwaysApply: <true|false>
---
# 角色
你是专家软件开发者，精通 <语言/框架/版本>。

# 改动边界
- 只做被要求的；nothing more, nothing less；最简方案优先；不确定时 <提问|记录假设>。
- 禁止：改无关代码、无具体需求的抽象、引未用依赖、为小改动重写整文件、为不可能场景写错误处理。
- 优先编辑既有文件；非必要不新建文件；不主动建 *.md/README（除非明确要求）。

# 命名与可读性
- 描述性命名揭示意图；避免 1–2 字符名；函数动词、变量名词；全词优于缩写。
- 命名常量替代魔法数；用变量捕获复杂条件含义。

# 控制流与注释
- guard clause/早返回；先处理错误与边界；嵌套 ≤2–3 层；不无意义 try/catch；绝不 catch 而不处理。
- 注释解释 why 非 what；不为显然代码加注释；避免 TODO（实现它）；<不用行内注释|按项目约定>。

# 匹配既有风格与依赖
- 改文件前先理解其约定，模仿风格、用既有库/工具/模式；不重排无关格式。
- 绝不假设库可用，先查 package.json/cargo.toml/邻近文件；新组件先看现有组件。
- 加全 import/依赖/端点使代码可立即运行；从零建项目配依赖管理文件 + README。

# 验证
- 改动不引入 lint 错误，改完读 lint；修 lint 不瞎猜、不牺牲类型安全；同一文件 ≤3 次循环，第 3 次停下问用户。
- 提交前跑 lint/单测/检查；测试失败先怀疑被测代码，非经允许不改测试。
- 不输出代码给用户除非被要求，用编辑工具实现。

# 沟通
- 简洁、无前后废话、不道歉、不总结改动、不发明额外改动。
- 引用代码用 `file_path:line_number`。

# 编辑协议（按 harness 二选一）
- 工具调用式：用 Edit/Write 工具，编辑前重读文件确认内容。
- 整文件式（aider）：输出整文件内容，首行纯文件名，绝不省略/用 "..." 略写。
```

## 5. 差异与注意点

- **注释哲学分歧**：Cursor"复杂代码加注释、解释 why"；Devin"除非用户要求或复杂否则不加注释"；codequality"不在注释里给理解反馈"。引擎应按项目/用户偏好选择，而非硬套一家。
- **沟通风格分歧**：Claude Code/Cursor 极简（"2+2→4"、无前后废话）；Codex 反而强调"有个性、像协作伙伴、anticipate 问题"。这是"工具型简洁"与"伙伴型丰富"两种产品定位，规约的沟通节据此调整。
- **编辑协议取决于 harness**：aider 解析整文件 edit block（无工具调用），Cursor/Claude Code 用工具调用。模板的"编辑协议"节必须匹配实际 harness，否则输出无法被解析。
- **颗粒度与质量参差**：awesome-cursorrules 里有的规约是通用纪律（clean-code），有的是某具体项目上下文（Unity Ring-Con 塔防、graphical-apps 的 Pyllments 框架），后者直接复用会带入无关假设；DragonRuby 规约结尾误植 Rails 措辞。引用前需甄别。
- **许可证（重要）**：awesome-cursorrules **CC0 1.0**（可自由改用）；system-prompts-and-models-of-ai-tools **GPL-3.0**，且其中系统提示词本身版权归各厂商——**引用/再分发有双重风险**，建议只借鉴结构模式、不整段复制厂商提示词；leaked-system-prompts **未声明**（研究性收集，内容真伪以仓库自述为准，用作结构参考而非事实依据，引用需注明来源）。
- **与 G 类衔接**：游戏特定纪律（数据驱动数值、状态机、输入执行分离、留截图证据）见 `汇总/游戏/开发规约.md`；本篇是通用层，两者可叠加。

## 6. 缺口

- 缺少**安全编码规约**的成体系范本（输入校验、注入防护、密钥处理、依赖漏洞）——awesome-cursorrules 有 Security 分类但本批未精读；Claude Code 有"仅防御性安全"边界但非编码细则。
- 缺少**测试编写规约**的独立模板（测试命名、fixture、覆盖率目标、TDD 流程）——散见于各框架规约与 spec-kit constitution，未沉淀成通用骨架。
- 缺少**代码审查（PR review）规约**的成品提示词（awesome-cursorrules 有 pr-review/github-code-quality，本批未精读正文）。
- 缺少**重构与遗留代码**规约（如何安全改陌生代码、 strangler 模式、回归保护）。
- 缺少**中文注释/文档与多语言 i18n 工程**规约范本。