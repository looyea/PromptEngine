# 游戏 · 美术

> 一级类 G（游戏）· 二级维度「美术」（与 A 类 AI 绘画共享部分技法）
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「游戏美术」解决的是：确立**统一视觉身份**（画风、调色板、形状语言、光影、可读性），把它写成可执行的资产规格，并（在 AI 生成场景下）跑通"规划→生成→检查→评估→迭代→交付"的资产生产管线。它既包含"方向/规约"层，也包含"具体生成技法"层。

素材里的四种做法：

- **美术总监 agent + 艺术圣经**（CCGS `art-director`、`art-bible` skill、`genres/art-direction.md`）：定义风格、调色板、比例、材质语言、光照方向、视觉层级，规定资产命名与规格，强调"每个敌人/单位有独立剪影、不用 emoji 当美术、不抄袭命名游戏的具体美术"。
- **全局/系统美术文档模板**（gat `global-art`、`system-art`）：用"连贯散文描述一个代表场景/交互"来锚定情绪与视觉，再分视觉身份、色彩光影、形状表面、构图可读性、动效、资产方向等节。
- **后端无关的图像资产生产规约**（game-dev-skills `game-image-generation-rules`）：把图像生成当资产管线而非单条 prompt，核心是"先定交付契约与评估标准再生成、不交付未检查的资产、单一语义源编译多后端 prompt"。
- **具体生成技法 skill + HD-2D 规格**（`animation-shader` 二次元 shader、`3d-model-generator-by-threejs` 程序化建模、`../游戏Spec01.md` 第二节八方旅人式 HD-2D）。

## 2. 共性结构与模式

归纳出游戏美术提示词的通用骨架：

1. **先立视觉身份，再产资产**：art bible / global-art 是"视觉唯一真相源"，所有资产对照它审查。共性是**风格统一优先于细节**（Spec01："宁可统一调色板自己生成，也不拼风格不一致的素材"）。
2. **用代表场景锚定情绪**：gat 要求"用连贯散文描述一个玩家会遇到的代表场景/交互，让读者仅凭描述就能想象画面并理解情绪"，而非堆风格标签/ mood 关键词。
3. **可读性与视觉层级**：玩家的眼睛该先/次/最后看什么；关键信息不能只靠颜色区分（无障碍）；每个敌人/单位有独立剪影，play size 下可分辨。
4. **资产规格 + 命名约定**：分辨率、格式、色彩profile、多边形/纹理预算、命名 `[category]_[name]_[variant]_[size].[ext]`。
5. **生成管线的硬门与质量 criteria 分离**：game-image-generation-rules 要求生成前就定义 hard gates（尺寸/格式/alpha/网格几何/无裁切 halo）与 quality criteria（美学），先过硬门再做视觉评估。
6. **单一语义源 → 多后端编译**：写一条后端无关的语义 prompt 作真相源，再派生（而非各自另写）指令式模型 prompt、SD/ComfyUI 的 Danbooru 正向标签、负向 prompt、编辑指令（先说变换再重复保留的不变量）。
7. **确定性检查 + 视觉评估 + 迭代**：先用脚本做客观检查（PNG 元数据/尺寸/alpha/边框/padding/sheet 网格；SVG 的 XML/引用/几何/路径/变换），再按规划时写的标准做视觉评估，不通过则编辑/重生成再评估。
8. **参考的"取/舍"**：gat"Adopt [具体品质]; avoid [具体品质]"；genres"命名游戏指导方向，但其角色/logo/具体美术仍归原作者"。

## 3. 代表样本

**样本 A — 美术总监的资产命名与职责**
`github/Claude-Code-Game-Studios/.claude/agents/art-director.md`
```
All assets must follow: [category]_[name]_[variant]_[size].[ext]
Examples: char_[character]_idle_01.png / ui_btn_primary_hover.png /
          vfx_[effect]_loop_small.png
Key Responsibilities: Art Bible Maintenance / Style Guide Enforcement /
Asset Specifications (resolution, format, naming, color profile, polygon budget) /
Color and Lighting Direction / Visual Hierarchy.
Must NOT: write code or shaders, create actual pixel/3D art (document specs instead).
```

**样本 B — 每游戏一份美术方向（不选菜单、不用 emoji、独立剪影）**
`github/Claude-Code-Game-Studios/.claude/genres/art-direction.md`
```
- Their words and pictures win. "like Manor Lords" takes that game's camera,
  proportions, palette mood and light, never its art itself.
- No default look. Never the flat cream background with bare shapes.
- Never an emoji or a font symbol as art. Draw it.
- Every enemy kind and every unit has its own silhouette, readable at play size:
  a different outline, not one shape recoloured or resized.
- Not a copy. A named game guides the direction; its characters, logos and exact
  art stay its owner's.
```

**样本 C — 图像资产生产管线与单一语义源**
`github/game-dev-skills/skills/game-image-generation-rules/SKILL.md`
```
PLAN → GENERATE → NORMALIZE/INSPECT → EVALUATE
     → ACCEPT OR EDIT/REGENERATE → RE-EVALUATE → OUTPUT
Do not generate before defining the deliverable contract and evaluation criteria.
Do not deliver an uninspected asset.
## Compile prompts from one semantic source
Write one backend-neutral semantic prompt as the source of truth. Derive, do not
independently invent: 1. native-language prompt; 2. positive Danbooru tag list;
3. separate negative prompt only when that backend uses one; 4. edit instructions
that state the transformation first and repeat preserved invariants.
```

**样本 D — 全局美术文档：用代表场景锚定**
`github/game-dev-skills/gat/.claude/docs/templates/design/global-art.md`
```
## 2. Art Direction Description
[Ground the direction in a representative scene the player will encounter: describe
the sense of space and scale, what draws the eye, and how color, light, form,
texture, and motion combine into a coherent impression... The reader should be able
to picture the scene and understand its emotional character from this description
alone. Use continuous prose; make the feeling concrete rather than relying on style
labels or mood keywords.]
## 4. References — [Reference]: Adopt [specific quality]; avoid [specific quality].
```

**样本 E — 用户样本的 HD-2D 硬规格**
`../游戏Spec01.md`
```
- 角色：像素小人，高约 32–48px，广告牌方式立在 3D 场景，最近邻采样，画面不能糊。
- 后处理与光影（HD-2D 核心，必须实现）：移轴景深 + Bloom；动态光源、柔和阴影、
  环境光遮蔽；体积光（丁达尔）；粒子（尘埃/落叶/萤火/飘雪/雨）；水面反光流动；
  每关不同色彩分级（LUT）。
- 风格统一比细节更重要：素材风格对不上，宁可统一调色板（Lospec）自己生成。
```

**样本 F — 二次元 shader 预设（具体生成技法）**
`github/game-dev-skills/skills/animation-shader/SKILL.md`
```
### 1. Basic Anime
Description: standard, clean anime look. Efficient, readable, widely used.
Features: Base Color / Main Texture, 3-Tone Shading (Double Shade), Outline (Inverted Hull).
Use Case: general avatars, NPCs, performance-critical scenes.
### 3. Stylized Sketch
Features: Color Adjustments (Desaturated), Sketchy Outline, Hatching, Halftone Overlay.
Use Case: flashbacks, artistic indie games, manga adaptations.
Key rule: for each identified feature, read ALL reference files listed — not just one.
```

## 4. 可复用模板骨架

```markdown
# 美术方向与资产规格：<游戏/系统名>
## 1. 视觉身份（先于资产）
- 一句话概述视觉身份与氛围 + 让它可识别的标志性选择。
- 代表场景散文：空间/尺度、什么吸引眼睛、色彩光影形状材质运动如何合成 coherent 印象、
  情绪如何在关键玩法时刻变化（让读者仅凭描述就能想象画面）。
- 参考：每个参考"取 [具体品质] / 舍 [具体品质]"；命名游戏只借方向，不借其具体美术。

## 2. 色彩与光影
- 主调色板（关键色 + hex + 用途）/ 强调与警告色（信号什么、如何保持区分）。
- 情绪转换（关键情境间色彩/光如何变）/ 光照（对比、方向、软硬、意图情绪）。

## 3. 形状与表面 / 构图与可读性
- 形状语言与比例（可识别剪影规则）/ 材质纹理 / 细节集中与克制处 / 渲染风格。
- 视觉层级（先/次/最后看什么）/ 场景与 UI 关系 / 无障碍（关键信息不只靠颜色）。
- 每个敌人/单位独立剪影，play size 下可分辨；不用 emoji/字体符号当美术。

## 4. 资产规格与命名
- 各类资产：分辨率、格式、色彩 profile、alpha/混合模式、多边形/纹理预算、引擎约束。
- 命名：`[category]_[name]_[variant]_[size].[ext]`。
- 动画：帧数、循环、pivot、cell 尺寸、sheet 布局、时序。

## 5. 生成管线（AI 生成时）
- 交付契约 + hard gates（尺寸/格式/alpha/网格几何/无裁切 halo/安全边距）+ quality criteria，生成前定义。
- 单一语义源 prompt → 派生：指令式 prompt / Danbooru 正向标签 / 负向（仅该后端用）/ 编辑指令（先变换后不变量）。
- 路由：SVG 走矢量校验； raster PNG 生成≥交付尺寸再归一化检查；透明/合成 VFX 走真 alpha 或黑底 additive；
  sprite sheet 选 video-generation / reference-sheet / direct-sheet 之一。
- 确定性检查脚本（PNG/SVG）→ 按规划标准视觉评估（先硬门后美学，按游戏内尺寸 + 检查缩放看）→ 不通过则编辑/重生成再评估。
- 不交付未检查的资产；批量分"直接资产批"（每张是不同可用资产）与"变体批"（竞争同一槽位，只交付被接受的）。

## 6. 开放问题
- 未决视觉选择及其影响。
```

## 5. 差异与注意点

- **方向层 vs 生成层**：art-director/gat 模板产出"文字规格"（art-director 明确"不画实际像素/3D，只写规格"）；game-image-generation-rules/animation-shader/3d-model 才真正生成资产。引擎编排时先方向后生成，别让"美术总监"直接画图。
- **后端无关 vs 后端绑定**：game-image-generation-rules 刻意不绑定具体生成器（"不点名/要求某个 skill 除非它确实可用且被选中"）；animation-shader 绑定 lilToon/Poiyomi/UTS2 等具体 shader 库；3d-model 绑定 Three.js 管线。按实际可用工具选择。
- **风格统一优先**：Spec01 与 genres 都强调"统一 > 细节""不拼风格不一致素材""不用默认平淡 look"。这是游戏美术相对单张 AI 绘画（A 类）的关键差异——资产要成体系、可复用、剪影可分辨。
- **许可证**：CCGS **MIT**；game-dev-skills **未声明**（引用有风险）；playcanvas 资产用 Quaternius **CC0** 素材包（可自由用，但 prompts 仓库本身未声明）；`../游戏Spec01.md` 要求第三方素材逐条记 CREDITS.md（CC0/CC-BY/OFL），禁用商业游戏原始素材。
- **HD-2D 是具体风格规格**：Spec01 第二节是"八方旅人式 HD-2D"的硬实现清单（景深/Bloom/体积光/粒子/LUT），可作风格化渲染的 few-shot，但属特定风格，不宜当通用美术模板。

## 6. 缺口

- 缺少**像素美术专门管线**（调色板管理、逐帧动画、tile 集、9-slice）的成体系提示词；Spec01 提了 Pillow 程序生成与 Lospec 调色板，但无完整 skill。
- 缺少**UI/UX 视觉设计**（HUD 布局、图标系统、字体排版、动效规范）的独立模板（CCGS 有 ux-designer，本批未沉淀）。
- 缺少**3D 角色绑定/动画重定向**的提示词（playcanvas 提到 UAL 骨架按骨名重定向，但属具体资产校准，非通用技法）。
- 缺少**美术一致性自动审查**（把生成资产对照 art bible 打分）的成品提示词；game-image-generation-rules 有 evaluation reference 但本批未含正文。
- 与 A 类（Midjourney/SD/NAI 画风词表）的衔接：本篇侧重"游戏资产管线与方向"，具体画风词/标签体系见 A 类汇总。