# 流派与场景词库（Suno）

> 一级类：M 音乐 · 二级：音乐/音频
> 素材：`素材/github/awesome-suno-prompts/`（CC0-1.0）
> 用途：引擎按"流派 / 场景 / 情绪"检索风格描述词与现成提示词的索引+精选词表。全量 1000+ 条在素材原文件，本篇只给分类、高频词与查全路径。

## 1. 概述

awesome-suno-prompts 把 1000+ 条风格串按 **13 个流派文件**（prompts/）组织，每个文件内再分子流派小节；每条提示词附 Use Case（适用场景）、Energy（1-10 能量分）、BPM、Key、Suno 版本。另有 **4 个场景 JSON 包**（packs/：健身、心碎、TikTok 爆款、跨流派融合）与周更趋势榜（TRENDING.md）。流派覆盖明显偏向 2024-2026 欧美短视频热曲（Phonk、Jersey Club、Corridos Tumbados、Afrobeats 各占独立文件），传统大类（Pop/Rock/Hip-Hop/EDM）条目最多。

## 2. 共性结构与模式

- **条目模式**：`### 曲风名` + 代码块风格串 + Use Case / Suno Version / Energy / Notable Feature 元数据。风格串写法统一遵循"流派+人声+乐器+制作+BPM/Key"骨架（见《AI音乐生成.md》§2.1）。
- **文件模式**：子流派小节 ×4-7 → Production Tips（该流派的乐器配置表、808/鼓型模式表、BPM 惯例）→ Related Genres。
- **JSON 包模式**（引擎可直接解析）：每条含 `id / title / genre / subgenre / prompt / useCase / sunoVersion / energy / bpm / key / tags[] / source`，tags 是场景检索键（gym、viral、30s-loop、heartbreak…）。
- **场景化结构惯例**：短视频用曲写明"30s loop 结构：intro 4 bars → drop 16 bars → breakdown 4 bars → drop 16 bars → outro 4 bars"与"zero dead space"；健身曲强调 half-time、brickwall limiting、aggressive sidechain。

## 3. 代表样本

**样本 1：EDM Festival**（`素材/github/awesome-suno-prompts/prompts/edm.md`）
```
Festival big room house, massive riser build-up, explosive drop,
supersaws layered thick, sidechain compression pumping,
euphoric lead melody, crowd-chant ready, stadium energy,
BPM: 128, Key: C Minor
```

**样本 2：Smooth Jazz**（`素材/github/awesome-suno-prompts/prompts/jazz-blues.md`）
```
Smooth jazz, silky saxophone lead, Rhodes electric piano,
soft brushed drums, walking bass line, evening lounge vibe,
radio-friendly production, sophisticated but accessible,
BPM: 95, Key: Eb Major
```

**样本 3：Drift Phonk 健身 30s 循环**（`素材/github/awesome-suno-prompts/packs/gym-workout.json`）
```
Drift Phonk, gym edit structure (30s loop): intro 4 bars → drop 16 bars →
breakdown 4 bars → drop 16 bars → outro 4 bars, Memphis vocal chops pitched -6st,
distorted 808 cowbell on OFF-beats, half-time trap drums at 140 BPM,
aggressive sidechain compression (-12dB duck), lo-fi cassette saturation,
dark minor atmosphere, zero dead space, BPM: 140, Key: D Minor
```

**样本 4：钢琴力量情歌**（`素材/github/awesome-suno-prompts/prompts/pop.md`）
```
Piano-driven emotional ballad, raw vulnerable vocals,
builds from intimate verse to soaring chorus,
subtle string arrangements, modern radio production,
reverb on vocals, stripped-down bridge,
powerful final chorus, BPM: 72, Key: G Major
```

**样本 5：前后对比（模糊→具体）**（`素材/github/awesome-suno-prompts/examples/before-after.md`）
```
Before: Hard trap beat, heavy 808s, dark vibe, rap instrumental
After:  Modern trap banger, menacing 808 bass with pitch glides, rapid 1/32 hi-hat rolls,
        atmospheric dark pads, half-time snare on 3, confident rap flow pocket,
        ad-libs throughout, hard bass drop at chorus, BPM: 75, Key: F Minor
```

**样本 6：K-Pop 舞曲结构**（`素材/github/awesome-suno-prompts/prompts/pop.md`）
```
High-energy K-pop production, rap verses + melodic chorus,
layered synths, trap-influenced beats, intricate vocal runs,
polished perfection, dynamic structure, addictive hooks,
BPM: 140, Key: F Minor
```

## 4. 可复用模板骨架与精选词库

### 4.1 流派索引表（查全量路径 + 实测 BPM 区间）

| 流派 | 子流派小节 | BPM 区间 | 标志元素 | 全量路径（素材/github/awesome-suno-prompts/ 下） |
|---|---|---|---|---|
| Pop（200+） | Dance Pop / Ballads / Indie Pop / Synth-Pop / Teen Pop / Adult Contemporary / Experimental | 60-170 | layered harmonies, vocal chops, radio-ready polish | prompts/pop.md |
| Rock（150+） | Stadium / Grunge / Classic / Metal / Punk / Indie Rock | 65-200 | crunchy distorted guitar, driving riffs | prompts/rock.md |
| Hip-Hop（120+） | Trap / Boom Bap / Melodic Rap / Drill / Conscious / Old School | 85-145 | 808 pitch glides, 1/32 hat rolls, half-time snare on 3 | prompts/hip-hop.md |
| EDM（130+） | Big Room / Progressive House / Dubstep / Trance / Techno / Future Bass | 110-174 | supersaws, riser→drop, sidechain pumping | prompts/edm.md |
| Country（100+） | Pop-Country / Traditional / Outlaw / Bluegrass / Country Rock | 75-140 | telecaster twang, pedal steel, storytelling vocals | prompts/country.md |
| Indie（90+） | Bedroom Pop / Indie Rock / Shoegaze / Dream Pop / Folk / Lo-Fi | 70-160 | jangly guitars, wall of sound, lo-fi aesthetic | prompts/indie.md |
| R&B/Soul（80+） | Trap-Soul / Classic Soul / Neo-Soul / Quiet Storm / Alt-R&B | 60-128 | Rhodes, 7th/9th voicings, breathy intimate vocals | prompts/rnb-soul.md |
| Jazz/Blues（60+） | Smooth / Bebop / Chicago Blues / Fusion / Swing / Contemporary | 60-240 | walking bass, brushed drums, sax lead | prompts/jazz-blues.md |
| Afrobeats（50+） | Amapiano / Mainstream / Afro-House / Afro-Fusion / Afro-Swing | 95-155 | log drum, 3-2 clave, 3-step kick (1, &a, 3) | prompts/afrobeats.md |
| Phonk（50+） | Memphis / Drift / Aggressive / Melodic / Hybrids | 60-155 | distorted 808 cowbell off-beats, Memphis vocal chops 降调 | prompts/phonk.md |
| K-Pop（50+） | 4/5 代女团 / 男团 / Hyperpop / Hybrids / Dance Structure | 100-165 | rap verses + melodic chorus, idol vocals, performance structure | prompts/k-pop.md |
| Jersey Club（40+） | Classic Jersey / Baltimore Club / TikTok 舞曲 / Hybrids / DJ Tools | 130-140 为主 | staccato kick pattern, bed squeak, 30s dance loop | prompts/jersey-club.md |
| Regional Mexican（40+） | Corridos Tumbados / Sad Sierreño / Pop Crossover / Norteño-Banda | 70-145 | requinto tremolo, tuba bass, grito vocals, Spanglish | prompts/regional-mexican.md |

注：文件内标称条数（README 口径）大于实际 `###` 条目数（每文件 18-30 条精选），README 的"200+"含其付费包宣传口径，引擎以文件实际内容为准。

### 4.2 场景包索引（packs/）

| 包 | 条数 | 场景 tags | 路径 |
|---|---|---|---|
| viral-tiktok-reels.json（另有 .md 版） | 15 | gym / viral / 30s-loop / tiktok / dance | packs/viral-tiktok-reels.json |
| gym-workout.json | 12 | gym / workout / PR-attempt / aggressive | packs/gym-workout.json |
| heartbreak-emotional.json | 15 | heartbreak / sad-sierreño / melodic-phonk / ballad | packs/heartbreak-emotional.json |
| cross-genre-fusion.json | 10 | afro-phonk / corrido-phonk / kpop-jersey | packs/cross-genre-fusion.json |
| complete-starter.json | 索引 | 汇总入口（宣称 200 条，实际指向各包） | packs/complete-starter.json |

### 4.3 跨流派高频描述词精选（按维度）

- **能量/情绪**：euphoric, anthemic, menacing, bittersweet, nostalgic, gritty, dreamy, aggressive, intimate, vulnerable, hypnotic, dark, uplifting
- **人声音色**：powerhouse / raspy / breathy / silky / smooth / raw / falsetto / melismatic / whispered；处理：layered harmonies, vocal chops, doubled, ad-libs, autotune-light
- **制作质感**：radio-ready polish, lo-fi cassette saturation, analog warmth, brickwall limited, sidechain pumping, gated reverb, vinyl crackle, wide stereo, dry close-mic
- **律动/结构**：half-time feel, four-on-the-floor, triplet hi-hats, syncopated, build-up → drop, breakdown, stripped-down bridge, final chorus maximal, zero dead space
- **融合配比**（examples/advanced-techniques.md 实测表）：Country+Trap 60/30/10；R&B+Jersey 50/40/10；Indie+Phonk 55/35/10；Afrobeats+Pop 60/30/10；K-Pop+Hyperpop 50/40/10；Regional Mexican+Hip-Hop 50/30/20；Phonk+House 50/40/10

### 4.4 场景组装模板

```
{场景关键词检索 packs/*.json 的 tags} → 取基底 prompt
→ 替换 BPM/Key/人声性别 → 追加结构短语（30s loop / full song）
→ 短视频场景补：zero dead space, hook in first 3 seconds, loop-friendly
```

### 4.5 流派制作要点速查（Production Tips 归纳）

各流派文件末尾的 Production Tips 表是"子流派→乐器/律动配置"的速查原料，引擎组装时可据此校验风格串是否缺标志元素：

**Hip-Hop 808 配置**（prompts/hip-hop.md）：
| 子流派 | 808 形态 | Slide | 特征 |
|---|---|---|---|
| Trap | 长延音 | 滑音 glide | sub-bass 主导 |
| Boom Bap | 短促有力 | 无 slide | kick 主导 |
| Drill | 快速滑音 | 旋律化移动 | 黑暗威胁感 |
| G-Funk | 平滑正弦 | 细微 glide | 深层 sub |
| Emo Rap | 旋律化 | 基于和弦 | 情绪化 |

人声惯例：Trap = auto-tuned + 频繁 ad-libs + 重混响；Boom Bap = 清晰咬字、少效果、歌词主导；Melodic Rap = 唱说混合 + 音高修正。

**Jazz/Blues 乐器配置**（prompts/jazz-blues.md）：
| 风格 | 主奏 | 节奏组 | 特征 |
|---|---|---|---|
| Smooth Jazz | 萨克斯/吉他 | Rhodes + 软鼓 | 电台级抛光 |
| Bebop | 小号/萨克斯 | 钢琴 + walking bass | 极速 |
| Chicago Blues | 电吉他/口琴 | shuffle 鼓 | 粗粝过载 |
| Fusion | 电吉他/键盘 | slap bass + 复杂鼓 | 技术炫技 |
| Big Band | 全管乐组 | swing 鼓 | 大编制 |

EDM/Country/R&B 等文件同构（乐器表 + 律动表 + 混音惯例），查全路径见 §4.1。

### 4.6 趋势榜的用法（TRENDING.md）

周更趋势榜把 viral 声音映射到库内提示词锚点（如 Drift Phonk Gym Edits → prompts/phonk.md#drift-phonk），并给"上升期预测"（Brazilian Funk+Phonk、Corridos+Jersey、Hyperpop Ballads 等）。引擎做"热点配乐"任务时：先查 TRENDING.md 取流派锚点 → 进对应流派文件取基底串 → 按场景包补结构短语。榜单的 viral 数据（50M+ creations 等）是运营口径，只作选题参考不作事实引用。

## 5. 差异与注意点

- **趋势流派 vs 常青流派**：Phonk/Jersey/Corridos/Amapiano 文件含大量 TikTok 趋势语境（TRENDING.md 周更），时效性强；Pop/Rock/Jazz 的描述词更稳定。引擎组装" timeless 配乐"时优先常青词。
- **艺人名规避**：所有条目用描述性短语替代艺人名（如 Dr. Dre → "G-funk, slick production, 90s West Coast, deep bass, chronic synths"）。这是 Suno 版权拦截的绕行写法，但 README 宣称的"100% legally"是营销话术，商用仍需自行评估。
- **版本标注混杂**：条目内 Suno Version 有 V4.5/V5 残留（README 称已全量复验 v6），BPM/Key 写法不受版本影响，可放心复用。
- **数据口径**：README 的条数、播放量、榜单数据带推广目的，不可作为事实引用；JSON 包内 prompt 字段才是可靠原料。
- **许可证 CC0**：词表与提示词可无限制复制、改编、商用，无需署名。

## 6. 缺口

- **华语/东亚流派**：除 K-Pop 外无 C-Pop、J-Pop（City Pop 仅在融合语境）、古风、演歌等素材。
- **古典/影视配乐**：无独立流派文件；管弦/预告片段落写法散见 examples/advanced-techniques.md 与 suno-reference/templates.md，引擎可从那里取。
- **功能音乐**（冥想、睡眠、专注 lo-fi、游戏 BGM 循环）无专门场景包；gym 包的结构化写法可迁移。
- **Udio 场景词库**为零；Suno 素材的流派描述词可部分迁移，但元标签与结构惯例不通用。