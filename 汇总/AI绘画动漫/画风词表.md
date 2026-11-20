# 画风词表（跨平台：MJ / Danbooru / NAI）

> 一级类：A AI绘画/动漫 · 二级：美术
> 素材：`素材/github/MidJourney-Styles-and-Keywords-Reference/`（未声明许可）、`素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/style.yaml` 等（AGPL-3.0）、`素材/github/NovelAIv4-Style-Codex/`（Apache-2.0）、`素材/github/nai5-prompting/skills/nai5-art-direction/`（GPL-3.0）
> 用途：引擎"画风槽位"的选词原料——流派/媒介/画师三层词表 + 各平台调用写法 + 查全路径。

## 1. 概述

"画风"在素材中呈现为三层可组合的表达：

1. **艺术流派/主义**（impressionism、art nouveau、ukiyo-e、cyberpunk…）——跨平台通用概念词；
2. **媒介/技法**（watercolor、oil painting、ink、colored pencil、impasto…）——描述"画面表面怎么做出来"，比流派名更可控；
3. **画师名**（MJ 的 "Painting By X"、NAI/SD 的 artist tag）——风格压缩包，效果强但有版权/伦理争议与平台差异。

四个来源各管一段：MJ 参考库给**流派分组与艺术家名单**（29 组）及海量媒介词（372 词）；Danbooru 标签超市给**动漫模型认识的画风标签**（style.yaml 77 条，中英对照+别名）；NAI v4 Codex 给 **1265 个实测画师 tag**；nai5-art-direction 给**方法论**：媒介选择要落到"可见做法"（透叠/压印边缘/线条密度），不是报一个媒介名就完事。

## 2. 共性结构与模式

### 2.1 画风槽位的三种写法（按平台）

| 平台 | 流派/媒介写法 | 画师写法 | 备注 |
|---|---|---|---|
| Midjourney | `in the style of {流派}` 或直接并列媒介词：`watercolor painting, impasto` | `Painting By {画家}` / `Art By {摄影师}` | 词序靠前权重高；无负向语法 |
| NAI v4 / SD 动漫模型 | Danbooru 画风标签：`ukiyo-e, watercolor (medium), sketch` | artist tag 直接拼串：`artist_a, artist_b`（可加权混搭） | `(medium)` 后缀是标签规范的一部分 |
| NAI v5 | 技法词进主串靠后"技法风格块"，或 `<style>` 块 | `<artist>` 块，**只允许来自用户原文/启用资料的固定串**，不凭名单自造 | 画师串效果 v5 需重验 |

### 2.2 组合模式

- **流派 × 媒介**产生新风格：`art nouveau + watercolor` ≠ 任一单独效果；MJ 库的 Combinations 页即此思路（该页无文本词表）。
- **画师混搭**（NAI v4 玩法）：2-3 个 artist tag 按权重配比，Codex 提供同基准 prompt 的单人效果作参照。
- **媒介服务情绪**（nai5 方法论）：连续细线→犹疑的动作；大片平涂→压缩空间；柔软渗化→边界消退；粗粝痕迹→摩擦与速度。选媒介先问"需要什么表面和边缘"。
- **风格 parody 标签**（Danbooru）：`style parody`、`fine art parody`、`faux traditional media` 用于"模仿某类作品/伪手绘"的元标签。

## 3. 代表样本

**样本 1：MJ 艺术家页的流派分组**（`素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/MJ_V4/Style_Pages/Just_The_Style/Artists.md`，29 组）
```
Realism | Surrealism | Idealism | Abstract | Modernism | Post-Impressionism |
Art Nouveau | Luminism | Expressionism | Futurism | Gothic | Psychedelic |
Pop Art | Concept Art | Romanticism | Renaissance | Baroque | Dadaism |
Graffiti Artists | Instagram Artist | Artstation Artist | Manga |
Non-Painters | Sculptors | Photographers | Writers | Other Artists
组内词条形如：Painting By Van Gogh / Painted By Alphonso Mucha /
Art by Ilya Kuvshinov / Painted By Akira Toriyama（Manga 组）
```

**样本 2：Danbooru 画风标签（中英对照节选）**（`素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/style.yaml`）
```
art nouveau: 新艺术（alias: alphonse mucha (style), mucha style）
art deco: 装饰艺术        impressionism: 印象派
ukiyo-e: 浮世绘            nihonga: 日本画        sumi-e: 水墨画
ligne claire: 清晰线条（丁丁历险记式）   flat color: 平涂
retro artstyle: 复古画风   sketch: 素描         faux traditional media: 伪手绘
watercolor (medium): 水彩  oil painting (medium): 油画
gouache (medium): 水粉     brushpen (medium): 毛笔   g-pen (medium): G笔
```

**样本 3：MJ 媒介词梯度**（同库 `Drawing_and_Art_Mediums.md`，372 词节选）
```
Sketch, Line Art, Crosshatch, Pointillism, Stipple, Charcoal Art,
Colored Pencil, Ink, Calligraphy, Marker Art, Pastel Art,
Oil Painting, Watercolor Painting, Gouache Painting, Tempera Painting,
Fresco Painting, Impasto, Matte Painting, Chinese Painting, Japanese Painting,
Mural, Street Art, Illuminated Manuscript, Graphic Novel, Storybook Illustration
```

**样本 4：NAI v4 画师 tag 抽样**（`素材/github/NovelAIv4-Style-Codex/js/script.js`，1265 条）
```
ebifurya / hammer_(sunset_beach) / ruu_(tksymkw) / tony_taka /
mishima_kurone / yomu_(sgt_epper) / kakage / kanden_sky /
homare_(fool's_art) / lm_(legoman) / onono_imoko / hood_(james_x)
（括号内是画师社团/别名消歧，属 Danbooru 规范名的一部分，须原样保留）
```

**样本 5：媒介=可见做法（方法论摘录）**（`素材/github/nai5-prompting/skills/nai5-art-direction/references/art.md`）
```
选择水彩感、版画感、拼贴、素描或数字绘画等方向时，具体说明需要的是
透叠、压印边缘、纸片搭接、线条密度还是其他可见做法。这样即使媒介名称
相同，方案也有区别；即使不用媒介名称，表达意图仍然清楚。
```

**样本 6：Niji 媒介词摘录（全量 21 词）**（`素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/Niji_Journey/Niji_V4/Style_Pages/Drawing_and_Art_Mediums.md`）
```
Sketch, Drawing, Doodle, Stipple, Illustration, Storybook Illustration,
Whimsical Illustration, Colored Pencil, Ballpoint Pen, Marker Art, Painting,
Japanese Painting, Paper-Marbling, Splatter Paint, Comic Book, Pokemon Card,
Wall Decal, Bejeweled, Carved Lacquer, Bone China
```

## 4. 可复用模板骨架与精选词库

### 4.1 画风槽位模板

```
{流派词(可选)} + {媒介词×1-2} + {技法细节短语(可选)} + {画师串(有来源才写)}
MJ 示例槽：..., {art nouveau}, {watercolor painting}, {soft bleeding edges}, ...
NAI 示例槽：..., {ukiyo-e}, {watercolor (medium)}, {traditional media}, ...
```

### 4.2 精选流派词（跨平台通用，约 30）

impressionism / post-impressionism / expressionism / surrealism / art nouveau / art deco / cubism / pop art / minimalism / romanticism / baroque / renaissance / gothic / futurism / luminism / psychedelic / ukiyo-e（浮世绘）/ nihonga（日本画）/ sumi-e（水墨）/ ligne claire / cyberpunk / steampunk / vaporwave / dark fantasy / fairycore / kawaii / retro artstyle / flat color / photorealistic / concept art

### 4.3 精选媒介词（中英对照，Danbooru 规范）

| 英文标签 | 中文 | 英文标签 | 中文 |
|---|---|---|---|
| watercolor (medium) | 水彩 | oil painting (medium) | 油画 |
| gouache (medium) | 水粉 | acrylic paint (medium) | 丙烯 |
| ink (medium) | 墨水 | brushpen (medium) | 毛笔 |
| calligraphy brush (medium) | 书法毛笔 | g-pen (medium) | G笔（漫画蘸水笔） |
| colored pencil (medium) | 彩铅 | graphite (medium) | 石墨/铅笔 |
| charcoal (medium) | 炭笔 | pastel (medium) | 粉彩 |
| marker (medium) | 马克笔 | airbrush (medium) | 喷枪 |
| sketch | 素描 | flat color | 平涂 |
| traditional media | 手绘质感 | faux traditional media | 伪手绘 |
| clay (medium) | 黏土 | porcelain (medium) | 瓷器 |
| lego (medium) | 乐高 | amigurumi (medium) | 钩织玩偶 |

全量 77 条画风标签 + 别名：`素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/style.yaml`；MJ 侧 372 媒介词：`.../Just_The_Style/Drawing_and_Art_Mediums.md`。

### 4.4 画师串取用规则（引擎策略）

1. NAI v4/SD：从 Codex（1265 条）按目标风格选 tag，1-3 个混搭，先单测再配比；名字含括号消歧须原样。
2. NAI v5：画师串必须有用户原文或启用资料给定，**名单/频次表不是配方**（nai5 硬规则）；无来源就省略 `<artist>` 块。
3. MJ：用 `Painting By {name}` 短语；在世画师/版权敏感场景改用"流派+媒介+技法细节"描述替代。
4. 查全路径：Codex 数据 `素材/github/NovelAIv4-Style-Codex/js/script.js`（artistsData 数组）；MJ 名单 `.../Artists.md`（29 组）与 Niji `.../Niji_V4/Style_Pages/Artists.md`（5 组）；SD 画师 embedding 索引 `素材/github/danbooru-diffusion-prompt-builder/data/embeddings/artists/`。

### 4.5 动漫特化风格词（Niji / NAI 通用候选）

kawaii / animecore / manga / cartoon / chibi（Danbooru）/ cel shading（MJ SFX 页）/ visual novel cg（NAI v5 开关）/ retro artstyle / 90s anime 风格描述句（v5 自然语言）/ fairycore / angelcore / dark fantasy / dreamy

### 4.6 风格调试策略（引擎迭代用）

1. 先单测：每个候选风格词/画师 tag 用同一基准 prompt 出图（Codex 与 MJ 库都是此法建库），确认效果再入"已验证池"。
2. 再组合：流派×媒介×画师逐层叠加，每加一层对比一次，避免一次堆满无法归因。
3. 强度不足时：MJ 换更高档同族词或前置词序；NAI 按 `1.2::tag::` 单层加权；SD 用 `(tag:1.2)`。
4. 风格冲突时（如画师串压过媒介词）：降画师权重或减画师数量，而不是加更多风格词。
5. 记录"词→平台→版本→效果"四元组，版本升级后重验（V4 词表在 V6 失效是常态）。

## 5. 差异与注意点

- **同名不同效**：`watercolor` 在 MJ 是自由短语，在 Danbooru 规范标签是 `watercolor (medium)`；`art nouveau` 在 Danbooru 有别名 `mucha style`。引擎按目标平台选词形。
- **画师 tag 的平台时效**：Codex 是 NAI **v4** 实测；v5 对画师串响应不同且 nai5 要求来源约束；SD 各底模（NAI 系 vs 原创系）对 artist tag 认识度差异大。MJ 的艺术家名单是 V4 时代效果，V6+ 部分在世画师名已被官方限制。
- **伦理与合规**：模仿在世画师风格有争议；商用建议优先"流派+媒介+技法"路线。Danbooru style.yaml 中 `cosplay`、`style parody` 等标签涉及既有作品，注意 IP。
- **许可证**：MJ 库未声明（勿整库搬运）；标签超市 AGPL（归纳+回源）；Codex Apache-2.0（可宽松引用数据，保留声明）；nai5 GPL（方法可归纳）。
- **示例图缺失**：MJ 与 Codex 的画风效果都依赖示例图，本库未下载；选词后建议小样实测再入库为"已验证风格"。

## 6. 缺口

- **中国画风水墨/工笔的系统词表**不足：仅 sumi-e、nihonga、brushpen、Chinese Painting 数词；国风细分（工笔、写意、青绿山水、敦煌）无素材。
- **3D/渲染风格**（OC render、Blender、claymation、isometric 3D）散见 MJ Digital/Materials 页，无专门归纳；SD 3D 模型触发词无素材。
- **摄影风格**（胶片模拟、布光方案）MJ Camera 页有词但无成片案例；可另收摄影提示词素材。
- **NAI v5 已验证画师串库**、**SD LoRA 风格模型清单**均为零。
- 像素画/复古游戏风：MJ Digital 页有 1-bit~16-bit 与游戏机词，Danbooru 侧 pixel art 标签未在本篇展开，需要时直读 `data/tags/image-composition/format.yaml` 与 style.yaml。