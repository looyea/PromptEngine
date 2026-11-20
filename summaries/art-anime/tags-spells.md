# 标签体系与咒语（Danbooru / NovelAI / SD）

> 一级类：A AI绘画/动漫 · 二级：美术
> 素材：`素材/github/danbooru-diffusion-prompt-builder/`（AGPL-3.0）、`素材/github/NovelAIv4-Style-Codex/`（Apache-2.0）、`素材/github/nai5-prompting/skills/nai5-writing/`（GPL-3.0）
> 用途：引擎组装动漫向标签式提示词（NAI/SD）的标签分类索引、质量/排除预设、权重语法与整组"咒语"原料。

## 1. 概述

动漫向生成（NovelAI、SD 动漫模型）用 **Danbooru 标签**而非自然语言作提示词主体。三份素材分工：

- **danbooru-diffusion-prompt-builder（"Danbooru 标签超市"，tags.novelai.dev）**：权威中文标签数据库。`data/tags/` 45 个 YAML 按"人物/人文/自然/构图/物品/艺术破格/限制级"分类，每标签含英文名、中文译名、别名、wiki 链接；`data/presets/` 收整组咒语（著名的《元素法典》、NovelAI 官方质量/排除串、夜空组合）；`data/danbooru_tag_post_count.json` 是 3327 个标签的 Danbooru 帖子数（热度），可用于权重与选词排序；`data/embeddings/`、`data/hypernetworks/` 是画师/角色嵌入模型索引。
- **NovelAIv4-Style-Codex**：1265 个 NAI v4 实测画师 artist tag（js/script.js 的 artistsData 数组），每位配同基准 prompt 示例图（图需另下 assets，本库未含）。
- **nai5-prompting 的 nai5-writing 技能**：NAI **v5** 提示词语法与字段规范（权重、质量词开关、UC、角色栏、多人交互前缀），是 v4 标签玩法到 v5 混合语法的升级说明。

## 2. 共性结构与模式

### 2.1 标签式提示词通用骨架（v4/SD 时代）

```
[质量标签] + [人数/主体: 1girl, solo] + [角色/种族特征] + [发型发色瞳色]
+ [服装配饰] + [动作姿势] + [表情视线] + [构图/镜头/视角]
+ [背景/场景/时段天气] + [光影/色彩主题] + [画风/媒介/画师串]
反向（Negative/UC）: [低质量排除串] + [人体错误排除串] + [水印文字排除] + [本图定向排除]
```

### 2.2 Danbooru 标签数据结构（YAML）

每个标签条目：`英文标签（下划线转空格）: { name: 中文名, alias: [别名], wikiURL, image, restricted }`。分类树：

| 大类 | 文件 | 内容 |
|---|---|---|
| 人物 human/ | quality, humantype, hair, face, ears, neck, breasts, upperbody, lowerbody, clothes, shoesock, head_ornament, body_ornament, actions, directions | 品控、人体、外貌、服装、动作 |
| 构图 image-composition/ | composition, perspective, color, background, effects, format, style, articles, censorship | 构图方式、视角、色彩主题、背景、特效、画幅、画风 |
| 人文 humanities/ | buildings, cities, indoors, outdoors, food/×9 | 建筑、城市、室内外、食物 |
| 自然 natural/ | sky, flowers, outdoors | 天空、花卉、自然 |
| 其他 | characters.yaml（作品角色）, items.yaml（物品）, artistic-license.yaml（艺术破格：拟人化/机械化/僵尸化…）, restricted/（R18，引擎应默认过滤） | |

### 2.3 权重语法（平台各不同）

| 平台 | 增强 | 弱化 | 负向/移除 |
|---|---|---|---|
| SD (A1111) | `(tag:1.2)` | `(tag:0.8)` | 写进 Negative Prompt；支持 `[a:b:0.5]` 切换、`[a|b]` 交替 |
| NAI v4 | `{tag}`（每层 ×1.05） | `[tag]`（每层 ÷1.05） | UC 栏 |
| NAI v5 | `1.2::tag::`（作用到闭合 `::`） | `0.5::tag::` | `-1::tag::` 定向移除（可加深到 -3）；`-1::simple background::` 概念反转 |

v5 关键规则（nai5-writing/references/conventions.md）：数字权重**不写闭合 `::` 会作用到结尾所有 tag**；旧括号串换算按层数 ×/÷1.05 保留原强度；画师名以数字结尾时 `na_tarapisu153::` 会被解析成权重 153，须写成 `na_tarapisu153, ::`；新写内容默认不加权，成图不足再单层调整。

### 2.4 NAI v5 字段分工（相对 v4 的最大变化）

v5 支持"词组 + 自然语言句子"混合，且界面分栏：
- **Base Prompt（主串）**：人数→物件→姿势表情视线→取景机位→场景时段光线→关系句→技法→排除项（负权重行）。**不写质量词**（官网开关自动追加 `very aesthetic, masterpiece, no text`）。
- **Character N（角色栏）**：每人一栏，只放性别词（girl/boy/other，不带数字）+ 外貌 + 服装；版权角色写规范名触发默认外观，原创角色逐项写锚点。
- **多人交互**：两人以上时主串前两行不写动作词，各人动作写在主串末尾 `Character N:` 行，互动带方向前缀 `source#hug` / `target#hug` / `mutual#holding hands`。
- **UC（Undesired Content）**：按需给；本图定向排除写主串负权重而非 UC；画漫画/多视图时 UC 预设换 Light/None（Heavy 预设含 multiple views、halftone、logo 等会压掉分格与文字）。
- **图内文字**：要画的字必须原样进双引号（`"営業中"`）+ 载体句；此时需关质量词开关（其 `no text` 会压字）。

## 3. 代表样本

**样本 1：NovelAI 官方质量/排除预设**（`素材/github/danbooru-diffusion-prompt-builder/data/presets/novelai.yaml`）
```
正向 Quality Tags: masterpiece, best quality
反向 Low Quality + Bad Anatomy:
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit,
fewer digits, cropped, worst quality, low quality, normal quality,
jpeg artifacts, signature, watermark, username, blurry
```

**样本 2：元素法典·水魔法（正向节选）**（`素材/github/danbooru-diffusion-prompt-builder/data/presets/the-code-of-quintessence.yaml`）
```
masterpiece:1.21, best quality:1.331, ultra-detailed:1.21, illustration:1.21,
disheveled hair:1.21, frills:1.21, 1girl:1.1, solo:1.1, dynamic angle,
big top sleeves, floating, beautiful detailed sky, beautiful detailed water,
beautiful detailed eyes, overexposure, expressionless, hair between eyes,
ribbons, bowties, bare shoulders, detailed wet clothes, blank stare,
pleated skirt, flowers
（参考参数：CFG 5.5, euler_a, step 30；反向用样本 1 的排除串）
```

**样本 3：夜空组合·赛博朋克天台**（`素材/github/danbooru-diffusion-prompt-builder/data/presets/nightsky.yaml`）
```
cyberpunk, city, skyscraper, skyline, night sky, neon lights,
no humans, background
```

**样本 4：NAI v5 基础 UC 串**（`素材/github/nai5-prompting/skills/nai5-writing/references/conventions.md`）
```
2::extra characters, extra fingers, extra toes, logo, watermark, signature,
deformed, ::, artistic error, scan artifacts, worst quality, bad quality,
jpeg artifacts, very displeasing, dithering, too many watermarks
（SFW 需求时最前加 nsfw；多人图/漫画删 extra characters，同段其余保留）
```

**样本 5：画师标签条目**（`素材/github/NovelAIv4-Style-Codex/js/script.js`）
```
{ id: 2, name: "hammer_(sunset_beach)", imageUrl: "./assets/2.png" }
{ id: 16, name: "tony_taka", ... }   { id: 200, name: "mishima_kurone", ... }
（共 1265 条；标签即 Danbooru 画师名，下划线写法，直接拼入 prompt）
```

**样本 6：标签热度 Top**（`素材/github/danbooru-diffusion-prompt-builder/data/danbooru_tag_post_count.json`）
```
1girl 414万 / solo 345万 / highres 304万 / long hair 292万 /
looking at viewer 204万 / blush 200万 / smile 186万 / blue eyes 120万 /
skirt 108万 / blonde hair 108万 / simple background 105万 / absurdres 90万
```

## 4. 可复用模板骨架

### 4.1 通用动漫插画模板（v4/SD）

```
正向: masterpiece, best quality, {人数}, {角色特征}, {服装}, {动作},
      {表情}, {镜头: cowboy shot/from below}, {场景}, {光影}, {画风/画师串}
反向: {novelai.yaml 排除串} + {本图排除}
参数: CFG 5-7, steps 28-30, euler_a（元素法典参考值）
```

### 4.2 NAI v5 完整交付模板（nai5-writing 契约）

```
Base Prompt:
{人数; solo 仅单人}, {持握物/支撑物/遮挡实体词组},
{姿势·表情·视线(单人)}, {取景机位}, {场景·时段·光线},
{空间/接触/归属关系句(英文现在时)},
{技法词},
{负权重排除行, 如 -1::hat::},

Character 1: {girl/boy/other}, {动作·表情·视线; 互动带 source#/target#/mutual#},

<style>
{技法/画风串, 有原串才写}
</style>
<artist>
{画师串, 只来自用户原文或启用资料, 无则省略}
</artist>

Character 1:（角色栏，另开代码块）
girl, {外貌锚点},
{服装配饰},

Undesired Content:（用户要才给）
{基础串或用户原串, 按冲突条件删项}
```
规则：人数 tag 只在主串开头一次；角色栏不写 1girl；外貌不两处重复；叙事句不加引号（引号=要画的文字）；版权角色用 Danbooru 英文/罗马音规范名（`archetto (arknights)`），不用中文名。

### 4.3 咒语取用模板

整组咒语 = 正向组 + 反向组 + 参考参数。引擎从 presets YAML 的 `content` 取标签列表，`description` 取作者与参数；**元素法典权重已按 SD `(tag:x)` 风格保留数值**，转 NAI v5 需换算成 `x::tag::`。场景预设（nightsky）是无权重的纯标签组，可直接拼接。

### 4.4 选词与权重策略

- 用 post_count 排序：高频标签（>10 万帖）模型认识度高、效果稳；低频标签（<1000 帖）风险高，需实测或降权。
- 画师串：从 Codex 按风格选 1-3 个 artist tag 混搭（v4 玩法）；v5 中画师串须有明确来源，不凭名单自造配方。
- 嵌入模型索引（data/embeddings/artists 等）提供画师/角色 embedding 的触发词与用法，模型文件本身不在库内。

## 5. 差异与注意点

- **SD vs NAI**：标签词表同源（Danbooru），但权重语法、质量词、排除串不同；SD 的 `(tag:1.2)` 在 NAI 无效，NAI 的 `{}`/`::` 在 SD 无效。元素法典是 SD 语法，跨平台须换算。
- **NAI v4 vs v5**：v4 纯标签+画师串；v5 混合自然语言、角色栏分字段、质量词/UC 走官网开关与预设（手写会叠加两遍）、`no text` 与图内文字冲突、复杂度开关 `high/ultra complexity` 是功能开关不是质量词。Codex 的 1265 画师标签是 **v4 实测**，v5 效果需重验。
- **许可证**：标签超市 **AGPL-3.0**（整库复制/网络服务集成有传染风险，引擎宜"归纳+回源"而非搬运 YAML）；Codex Apache-2.0（宽松，保留声明）；nai5-prompting **GPL-3.0**（方法思想可归纳，正文大段复制进分发产品需注意）。
- **内容分级**：标签超市含 restricted/（R18）目录与大量身体标签，引擎默认过滤 `restricted: true` 条目；post_count Top 榜也含身体词，选词时按题材过滤。
- **画师名伦理/版权**：artist tag 模仿在世画师风格存在争议，部分平台已限制；商用交付建议以媒介/流派词替代画师名（见《画风词表.md》）。
- 元素法典本库只收《元素法典(一)》（水/空间等魔法），后续卷册未收。

## 6. 缺口

- **SD 提示词书/系统教程未收**（如社区《SD 提示词从入门到精通》类资料）；ControlNet、LoRA 训练与触发词素材为零。
- **embedding/hypernetwork 模型文件**未下载（只有索引），离线无法复现画师嵌入效果。
- **Codex 示例图**（assets）未下载，画师风格只能凭名字与外部知识判断。
- **NAI v5 画师串实测库**缺失：nai5 明确"名单不是配方"，v5 下哪些画师串有效需另收实测资料。
- 标签超市数据快照为 2022-12（文件时间），新标签（近年作品角色）不全；post_count 亦为旧快照。