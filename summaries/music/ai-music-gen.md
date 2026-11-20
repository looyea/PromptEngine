# AI 音乐生成（Suno 为主）

> 一级类：M 音乐 · 二级：音乐/音频
> 素材：`素材/github/awesome-suno-prompts/`（CC0）、`素材/github/suno-reference/`（MIT）
> 用途：提示词引擎组装 Suno 风格串（Style of Music）与整歌元标签结构（Lyrics 字段）的原料。

## 1. 概述

本类解决"如何让 AI 音乐服务生成一首可控、专业质感的歌"。两个素材仓库互补：

- **awesome-suno-prompts**：1000+ 条实测风格提示词，按 13 流派分文件，每条附 Use Case / 能量分 / BPM / Key；另有进阶技巧（箭头演进、分层制作、流派融合配比、人声效果映射）、前后对比案例、场景 JSON 包。已宣称适配 Suno v6 家族（2026-09 起 v6 / v6-wild / v6-mini）。
- **suno-reference**：社区整理的 Suno 元标签系统手册。`tag-reference.md`（5000 行）逐标签讲解语法与参数；`tag-reference-llm.md`（718 行）是专为 LLM 消费蒸馏的紧凑标签表，**引擎应优先引用这份**；`templates.md` 提供 40+ 整歌 track 定义模板；另附商标词规避、AI 音频发行署名（aiaadig / provenance）。

两条控制通道要分清：**Style of Music 字段**放自然语言风格描述串（流派+乐器+人声+制作术语）；**Lyrics 字段**放 `[元标签]` + 歌词，控制歌曲结构与段落细节。二者配合使用。

## 2. 共性结构与模式

### 2.1 风格串通用骨架（Style of Music）

归纳全部流派文件，一条高质量风格串 = 以下要素的有序组合（3-5 个核心要素为宜，勿堆砌）：

```
[流派/子流派] + [情绪/能量形容词] + [人声规格：性别·音色·唱法·和声]
+ [乐器与音色处理：具体到"crunchy distorted guitar"而非"guitar"]
+ [制作/混音术语：radio-ready polish / lo-fi / sidechain / analog warmth]
+ [动态演进：soft verse → explosive chorus]
+ BPM: [数值], Key: [调性]
```

要点（来自 README Best Practices 与 examples/）：
- 乐器要带质感形容词；人声要指定音色（raspy / breathy / powerhouse / melismatic）与处理（doubled / harmonies / dry）。
- 用制作术语（reverb、saturation、compression、sidechain）替代空洞的"good production"。
- 箭头记法 `→` 描述能量演进，可带时间戳（`piano enters at 0:15`）。
- 长度：v4.5 前 ≤200 字符；v4.5+ 上限 1000；awesome-suno 建议 **<950 字符**保持聚焦。
- 忌：模糊词（"nice beat"）、自相矛盾的风格（lo-fi + stadium）、把滑杆值写进文本（"weirdness 20%"无效）。

### 2.2 元标签体系（Lyrics 字段）

suno-reference 将 90+ 标签分为 10 组，引擎可按组取用：

| 组 | 代表标签 | 作用 |
|---|---|---|
| 控制 | `[track]` `[control]` `[sequence]` `[length]` `[compression]` `[language]` | 全局容器、生成行为、段落顺序、时长 |
| 结构 | `[intro]` `[verse]` `[pre-chorus]` `[chorus]` `[bridge]` `[break]` `[build]` `[drop]` `[hook]` `[solo]` `[interlude]` `[outro]` `[coda]` `[end]` | 歌曲段落骨架 |
| 风格 | `[genre]` `[style]` `[mood]` `[era]` `[ambient]` | 流派（支持 X + Y 混合）、质感、情绪、年代 |
| 速度 | `[tempo]` `[bpm]` `[rhythm]` `[accelerando]` `[ritardando]` `[pulse]` | 速度感与律动（BPM 是暗示非精确） |
| 人声 | `[vocals]` `[vocal-style]` `[vocalist]` `[male vocal]` `[female vocal]` `[duet]` `[choir]` `[whisper]` `[spoken word]` `[rap]` `[shout]` `[ad-lib]` `[announcer]` | 音色、唱法、声部编排 |
| 乐器 | `[instruments]` `[instrument]` `[instrumental]` `[bass]` `[arpeggio]` | 音色盘、纯器乐开关 |
| 动态 | `[dynamics]` `[intensity]` `[crescendo]` `[decrescendo]` `[silence]` `[attack]` | 能量弧线（pp→ff、low→explosive→collapse） |
| 和声 | `[key]` `[harmony]` `[melody]` `[chord]` `[modulation]` `[polyphony]` | 调性、和声复杂度、转调 |
| 主题 | `[theme A/B]` `[motif]` `[subject]` `[inversion]` `[lament]` `[chant-loop]` | 古典/赋格/仪式化动机发展 |
| 效果/编曲 | `[sfx]` `[effects]` `[signal-processing]` `[arrangement]` `[articulation]` `[sonority]` `[improvisation]` | 音效、处理链、织体密度、演奏法 |

**两个关键语法**：
- **管道记法**（段落局部覆盖）：`[chorus | style: phonk hook, vocals: autotune-light, instruments: 808 bass]`——只在段落偏离全局默认时使用，避免每段重复全局参数。
- **冒号内嵌指令**：`[intro: Clock-like percussion under warped organ chords.]`。v4.0+ 未识别标签一律按通用段落处理，指令必须写在标签内；**写在标签外的指令文本会被唱出来**。歌词则相反：放在 `[verse]` 下一行，不要写进冒号。

### 2.3 整歌组装模式

`[track]`/全局标签置顶 → `[sequence]` 声明段落顺序 → 各结构段落（管道记法做局部变化）→ `[outro]` + `[end]` 收尾。长曲用 `[length: 360]`；普通歌曲设短些避免引擎重复主歌。防循环：不要逐字重复相同歌词/段落，用 `[verse A]` `[verse B]` 变体。

## 3. 代表样本

**样本 1：标准风格串**（`素材/github/awesome-suno-prompts/prompts/pop.md`）
```
Infectious pop anthem, female powerhouse vocals with layered harmonies,
pulsing 808 bass, synth-wave layers, euphoric build-ups,
radio-ready polish, modern production, vocal chops,
stadium-ready energy, BPM: 128, Key: C Major
```

**样本 2：分层制作"Stem 法"**（`素材/github/awesome-suno-prompts/examples/advanced-techniques.md`）
```
[Foundation] Deep 808 bass, sub-heavy (40-60Hz), sidechained to kick, slight distortion
[Rhythm] Trap hi-hats (1/32 rolls on fills), claps on 2&4 with reverb tail, shaker 1/16
[Harmony] Juno-106 pads, wide stereo, slow attack, filter sweep over 8 bars, 7th/9th voicings
[Melody] Rhodes electric piano, pentatonic runs, chorus effect, counter-melody in chorus
[Vocals] Breathily intimate verse (dry) → doubled + harmonized chorus (plate reverb 2.5s) → falsetto bridge
[FX] Vinyl crackle intro, vocal chops on downbeats, 1/4 note delay throws, riser sweeps
BPM: 78, Key: Db Major
```

**样本 3：带人声整歌元标签结构**（`素材/github/suno-reference/tag-reference-llm.md` Quick Reference）
```
[control: dynamic transitions]
[genre: indie pop]
[mood: bittersweet, nostalgic]
[tempo: mid-tempo]
[vocals: warm female, intimate]
[instruments: acoustic guitar, soft drums, piano]
[length: 210]
[intro | guitar fingerpicking]
[verse 1]
Lyrics here...
[pre-chorus]
Building lyrics...
[chorus | full arrangement, harmonies]
Hook lyrics...
[bridge | stripped down, key change]
[chorus]
[outro | fading reprise]
[end]
```

**样本 4：纯器乐 ambient 模板**（同上文件）
```
[instrumental]
[control: atmospheric, slow-build]
[genre: dark ambient]
[mood: mysterious, vast]
[tempo: very slow, drifting]
[instruments: synth pads, deep drones, distant bells]
[length: 360]
[intro | minimal, emerging from silence]
[verse | layers building slowly]
[chorus | full texture, shimmering]
[outro | fading into void]
[end]
```

**样本 5：箭头演进（电影预告片式动态）**（`素材/github/awesome-suno-prompts/examples/advanced-techniques.md`）
```
Solo cello, intimate, dry → piano enters at 0:15, subtle →
strings swell at 0:30, harmonic tension →
full orchestra at 1:00, brass fanfare, percussion drive, choir pads →
sudden drop to piano + cello at 2:15, sparse, emotional →
crescendo to massive finale at 2:45, all forces, sustained chord
BPM: 72, Key: D Minor
```

## 4. 可复用模板骨架（引擎组装原料）

### 4.1 风格串填空模板

```
{子流派} {情绪形容词}, {人声性别+音色+唱法} with {和声/处理},
{主奏乐器+质感}, {节奏组描述}, {制作术语×2},
{能量演进短语}, BPM: {90-150}, Key: {调性}
```

### 4.2 流派融合配比模板（Suno 理解比例优于模糊"blend"）

```
{XX}% {主流派} ({标志乐器/律动/人声})
+ {YY}% {副流派} ({标志元素})
+ {ZZ}% {点缀流派} ({标志元素})
{人声方案}, {结构要点}, {关键时刻如 breakdown at 2:15}
BPM: {X}, Key: {Y}
```
实测配比见 `examples/advanced-techniques.md` 融合表（Country+Trap 60/30/10、R&B+Jersey 50/40/10、K-Pop+Hyperpop 50/40/10 等 7 组）。

### 4.3 整歌 Lyrics 模板（suno-reference templates.md 推荐式）

```
[control: {生成行为}]
[genre: {流派，可 X + Y}]
[style: {质感}]  [mood: {情绪}]
[tempo: {速度感描述}]
[instruments: {音色盘}]
[vocals: {人声规格}]
[compression: light]
[length: {180-360}]
[sequence: intro, verse, chorus, verse, chorus, bridge, chorus, outro]
[intro: {开场描述}]
[verse]
{歌词}
[chorus | {局部覆盖参数}]
{副歌歌词}
...
[outro: {收尾描述}]
[end]
```

### 4.4 人声效果分段映射模板

```
[Verse] dry, close-mic, compressed, intimate
[Pre-Chorus] doubled, building tension
[Chorus] layered harmonies, plate reverb, wide stereo
[Bridge] stripped, whispered / falsetto
[Final Chorus] maximal stacks, ad-lib runs
```
效果词表：dry / doubled / harmonized / autotuned / bitcrushed / whispered / shouted（全表见 advanced-techniques.md §4）。

### 4.5 引擎硬规则清单

- 商标词规避（`tag-reference.md` 开篇）：`kraftwerk→krautrock / old school EDM`；`skank（吉他技法）→ska stroke`；艺人名直接写会触发版权拦截，改用描述性短语捕捉其"声音 DNA"（awesome-suno 的核心做法）。
- 废弃/无效标签勿用：`[sing-style]`（→`[vocal-style]`）、`[song-type]`、`[loop]`、`[autotune]`、`[mix]` `[master]` `[filter]` `[panning]` `[volume]`、无编号 `[theme]`、`[style: none]`。
- v6 滑杆在 Advanced Options 设置，不写进文本：**Variety=0**（保持提示词原样）、Style Influence 75-85、Weirdness ~20（求稳）、Exclude Styles 排除不想要的乐器/人声。
- 模型分工：v6-mini 打草稿 → v6 定编曲 → v6-wild 探索融合；长曲/翻唱用 Max Mode。
- `[end]` 单独不可靠，须配合 outro 描述；纯器乐把 `[instrumental]` 放 Lyrics 开头。

## 5. 差异与注意点

- **版本口径冲突**：awesome-suno 已按 v6 重写（2026-09），suno-reference 停在 v5.0（2025-11）。元标签体系两版基本兼容，但 v6 的滑杆/多模态/8 分钟能力以 awesome-suno README 为准；引擎输出时应注明目标版本。
- **v4.0 vs v4.5+ 行为差**：v4.0 要"关键词电报体"，v4.5+ 支持标签嵌入自然语言整句、混合流派、1000 字符风格串、8 分钟结构。老教程的 200 字符限制已过时。
- **Suno vs Udio**：本库素材全部面向 Suno；Udio 的提示词语法（更偏自然语言描述、无同款元标签体系）不可直接套用本表。
- **营销噪声**：awesome-suno README 含大量 Song AI Farm / Gumroad 推广与"100% copyright-safe"等营销话术，取用时只信 prompts/、examples/、packs/ 的技术内容；"播放量 2.3M"等案例数据无法核实。
- **许可证**：awesome-suno CC0-1.0（可自由复制改编）；suno-reference MIT（保留版权声明即可）。两者对引擎最友好。
- **元标签是社区逆向经验**，非官方 API 文档；`[bpm]` `[key]` 等标签在 tag-reference.md 中被列为"歧义标签慎用"，与 llm 版收录口径略有出入——引擎以"描述性短语优先、数值标签兜底"策略处理。

## 6. 缺口

- **Udio 无任何素材**；其他音乐服务（Producer/Riffusion、Stable Audio）仅在 README 外链提及。
- **歌词写作方法论缺失**：素材只给结构模板与风格串，歌词创作（押韵、叙事、hook 写作）需 W 类或另收素材。
- **中文/华语流派**（C-Pop、古风、戏腔）无覆盖；`[language: Japanese]` 等标签存在但无中文实测样例。
- **AI 音频发行合规**（aiaadig.md / provenance.md 的 DDEX AI 标注、DistroKid 署名模板）已收但未在本篇展开，属发行环节而非生成环节，引擎需要时直读原文。
- Suno v6 多模态参考（音频/图片参考提示）只有功能描述，无提示词实例。
- **工具落地信息缺失**：收集素材全部默认"用户已经在用 Suno"，不含注册、额度、价格、开源替代——由下面 §7 引擎自补覆盖。

## 7. 工具与落地（⚠️引擎自补：收集素材只讲提示词语法，不讲工具怎么拿到；以下为 2026-11 网络检索所得，**推荐前必须对照官方仓库 LICENSE 原文核实**，免费额度/价格随平台政策变动）

**主路线（本库全部素材面向）**：Suno——闭源商业 SaaS，suno.com 网页使用，**必须注册账号**，有免费额度（额度数以现行页面为准）。Custom 模式下 Style of Music 与 Lyrics 两个输入框对应本库两段交付；Advanced Options 滑杆（Variety/Style Influence）只在网页界面里设，文本无法替代。用户没有账号时：给注册引导，或换下面路线。

**本地开源路线**（均需 NVIDIA 独显/足够显存，CPU 机器不可行）：

| 模型 | 许可证 | 能否商用 | 备注 |
|---|---|---|---|
| ACE-Step 1.5 | MIT | 可 | 检索口径一致 [待核实原文] |
| DiffRhythm | Apache 2.0 | 可 | 带人声歌曲生成 |
| YuE v1 | Apache 2.0 | 可 | 官方建议标注 "YuE by HKUST / M-A-P" |
| YuE2（2026-09-09 起） | **CC BY-NC 4.0** | **不可** | 第一方代码/权重/配套模型均转非商用；v1 归档保留 Apache——推荐时务必分清版本 |
| Stable Audio 3.0 | 社区许可 | 年收入 <100 万美元可 [待核实] | 开源权重、可本地跑 |
| MusicGen（Meta） | 代码 Apache / 权重 CC-BY-NC 4.0 | 不可商用 | 检索来源口径有出入 [待核实] |

落地纪律：① 开源≠免版权——代码、权重、生成物归属是三个独立维度，商用发布前逐层核对；② 公开分享/发行建议标注 AI-generated（对应 §5 的 aiaadig/provenance 署名素材）；③ 翻唱/参考音频另涉原曲著作权，与模型许可无关；④ 选了开源路线 → 该模型逐条记入 CREDITS（名称/作者/来源/许可证）。