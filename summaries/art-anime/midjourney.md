# Midjourney 提示词（风格关键词体系）

> 一级类：A AI绘画/动漫 · 二级：美术
> 素材：`素材/github/MidJourney-Styles-and-Keywords-Reference/`（**仓库未附许可证**）
> 用途：引擎组装 MJ 提示词时的"风格/光照/相机/材质"修饰词词库与分类索引。全量词表在素材原文件（每词原配示例图，图片未下载），本篇给骨架、精选高频词与查全路径。

## 1. 概述

Will Wulfken 维护的 MJ 风格关键词大全（约 12.3k stars，非官方），是 MJ 提示词领域最知名的"词典式"参考库。它不提供整条 prompt 范例，而是把**可插入 prompt 的修饰词**按 27 个维度分类，每词配同基准 prompt 的示例图（本库仅存文本）。已下载部分：

- `Pages/MJ_V4/Style_Pages/Just_The_Style/`：27 个分类页（Themes、Design_Styles、Digital、Artists、Drawing_and_Art_Mediums、Colors、Materials、Lighting、SFX、Camera、Perspective、Geometry、Nature、Objects、Outer_Space、Intangibles、TV_and_Movies、Song_Lyrics、Emojis、Experimental 等）。
- `Pages/Niji_Journey/Niji_V4/Style_Pages/`：6 个动漫模式专页（Themes、Artists、Design_Styles、Drawing_and_Art_Mediums、Lighting、Colors_and_Palettes）。

核心认知：**MJ 提示词 = 主体描述 + 一串风格修饰词 + 参数**。本库覆盖"修饰词"这一层的几乎全部维度；词与词可叠加，效果以示例图为准（图缺失时需实测）。

## 2. 共性结构与模式

### 2.1 MJ 提示词通用骨架

```
[主体+动作+环境] , [主题/年代词] , [艺术媒介或设计风格] , [艺术家/流派词(可选)] ,
[光照词] , [相机/镜头/胶片词(写实向)] , [色彩/调色板词] , [材质/SFX词(可选)] ,
[视角/构图词] --ar 宽高比 --v 版本 --stylize 风格化 --no 排除项
```

- 修饰词直接以英文短语并列（逗号分隔），无需语法标记；越靠前权重越高（MJ 惯例）。
- 写实路线走 Camera 页（胶片型号、镜头焦段、摄影类型）；插画/设计路线走 Drawing mediums + Design Styles 页；动漫路线换 Niji 页词表 + `--niji`。
- 参数（--ar/--v/--stylize/--chaos/--no 等）**不在本素材内**（Comparison_Pages 未下载），属通用知识，引擎需另行维护参数表。

### 2.2 词库组织模式

每页按 `<details>` 折叠小节分组（如 Lighting 页 7 组：技法/光源类型/灯管/激光/显示/全局光照/阴影），组内词以表格并列，多为"同义词梯度"排列（Dim → Dim Lighting → Dark Lighting；Realistic → Hyperrealistic → Photorealistic），**同族词是强度递进关系**，选一个即可，叠加同族词意义不大。

## 3. 代表样本（词表摘录）

**样本 1：光照技法**（`素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/MJ_V4/Style_Pages/Just_The_Style/Lighting.md`）
```
Frontlight, Halfrear Lighting, Backlight, Moody Lighting, Cinematic Lighting,
Studio Lighting, Soft Lighting, Hard Lighting, Volumetric Lighting, Contre-Jour,
Rembrandt Lighting, Split Lighting, Low-Key Lighting, High-Key Lighting,
Rim Lighting, Crepuscular Rays, Godrays, Tyndall Effect, Candlelight,
Neon Lamp, Blacklight, Ethereal Lighting
```

**样本 2：视角/构图**（同目录 `Perspective.md`，全页 55 词）
```
Top-View, Worms-Eye View, Aerial View, Closeup, Extreme Closeup, Wide Shot,
Epic Wide Shot, First-Person View, Low Angle, High Angle, Isometric,
Orthographic, One-Point Perspective, Two-Point Perspective, Three-Point Perspective,
Fisheye(见 Camera), Forced Perspective, Cross-Section, Cutaway, Exploded-View
```

**样本 3：相机/胶片（写实感来源）**（同目录 `Camera.md`，全页 196 词）
```
Golden Hour, Blue Hour, Film Noir Style, Bokeh, Film Grain, Color Grading,
Kodak Portra, Kodak Gold 200, Fujifilm Superia, Velvia, Polaroid, Lomo,
Shot on 35mm, Shot on IMAX 70mm, 85mm Lens, Macro, DSLR, Drone Photography,
National Geographic Photo, Editorial Photography, Cinematic Haze, Time-Lapse
```

**样本 4：Niji 动漫主题词**（`Pages/Niji_Journey/Niji_V4/Style_Pages/Themes.md`，全页仅 26 词）
```
Photorealistic, Surrealism, Dreamlike, Dreamy, Fever-Dream, Wonderland,
Fantasy, Ethereal Fantasy, Dark Fantasy, Retro, Sci-fi, Magical, Unicorncore,
Fairycore, Fairy Folk, Angelcore, Steampunk, Disco, Vaporwave, K-Pop,
Cartoon, Anime, Animecore, Manga, Kawaii, Fractalpunk
```

**样本 5：情绪核（Mood Based Themes）**（`Pages/MJ_V4/Style_Pages/Just_The_Style/Themes.md`）
```
Warmcore, Lovecore, Happycore, Smilecore, Gloomcore, Dullcore, Horror Styled,
Cosmic Horror, Macabre, Dazecore, Sleepycore, Dark Aesthetic, Cleancore,
Safetycore, Academia, Tinycore, Miniaturecore, Miniature World
（另有 20s-2000s 每十年一组的 Horror Style 年代恐怖词）
```

**样本 6：年代风格与拟态（Design_Styles.md）**
```
年代组（每十年一组，20s-2000s）：20s, 20s Pattern, 1920s Decor,
30s, 1930s Decor, 40s, 1940s Decor, 50s, 1950s Decor, …
Morphism 组：Skeuomorphism, Glassmorphism 等界面拟态词
复杂度梯度：Simple → Detailed → Hyperdetailed → Ornate → Intricate Maximalism
图案组：Halftone, Voronoi, Memphis Pattern, Guilloché Patterns, Zellij Patterns
```

## 4. 可复用模板骨架与精选词库

### 4.1 分类索引（查全量路径，均在 `素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/` 下）

| 维度 | 小节数/词量级 | 路径（MJ_V4/Style_Pages/Just_The_Style/） |
|---|---|---|
| 主题 Themes | 11 组（写实/复古/科幻/氛围/建筑/音乐/动漫/情绪核…） | Themes.md（1010 行，最大页） |
| 设计风格 Design_Styles | 14 组 457 词（简繁/图案/年代/拟态/立体派…） | Design_Styles.md |
| 数字风格 Digital | 22 组（渲染引擎/分辨率/像素位深/游戏机/操作系统/AI） | Digital.md |
| 艺术家 Artists | 29 流派分组（见《画风词表.md》） | Artists.md |
| 绘画媒介 Drawing_and_Art_Mediums | 24 组 372 词 | Drawing_and_Art_Mediums.md |
| 色彩 Colors_and_Palettes | 12 组 182 词 | Colors_and_Palettes.md |
| 材质 Materials / 材质属性 | 23 组 / 11 组（固液气/光学/发光/纹理） | Materials.md / Material_Properties.md |
| 光照 Lighting | 7 组 170 词 | Lighting.md |
| 特效 SFX_and_Shaders | 7 组 99 词（反射/模糊/畸变/色差/着色器） | SFX_and_Shaders.md |
| 相机 Camera | 11 组 196 词 | Camera.md |
| 视角 Perspective | 4 组 55 词 | Perspective.md |
| 几何/维度/结构修改 | 7/2/6 组 | Geometry.md / Dimensionality.md / Structural_Modification.md |
| 自然/物件/外太空/地理文化 | 11/21/7/6 组 | Nature_and_Animals.md / Objects.md / Outer_Space.md / Geography_and_Culture.md |
| 抽象概念 Intangibles | 18 组（情绪/概念/符号/时间/感知畸变） | Intangibles.md |
| 影视/歌词/Emoji/实验 | 3/6/13/5 组 | TV_and_Movies.md / Song_Lyrics.md / Emojis.md / Experimental.md |
| **Niji 动漫 6 页** | Themes 26 词 / Artists 5 组 / Design 18 词 / Mediums 21 词 / Lighting 2 组 / Colors 6 组 | Niji_Journey/Niji_V4/Style_Pages/*.md |

### 4.2 精选高频词（引擎默认候选池）

- **光照**：cinematic lighting, volumetric lighting, golden hour, backlight, rim lighting, Rembrandt lighting, moody lighting, soft lighting, godrays, neon
- **相机/写实**：shot on 35mm, 85mm lens, bokeh, film grain, Kodak Portra, macro, wide shot, drone photography, film noir style
- **媒介**：oil painting, watercolor painting, gouache, impasto, line art, crosshatch, charcoal art, colored pencil, ink, ukiyo-e（Design 页）, matte painting, storybook illustration, pixel art（Digital 页）
- **设计/复杂度**：minimalist, maximalist, hyperdetailed, ornate, flat design, isometric, halftone, voronoi, art deco, art nouveau, glassmorphism（Morphism 组）
- **色彩**：warm/cool color palette, monochromatic, vibrant, neon, desaturated, sepia, high contrast, complementary colors, analogous colors, triadic colors
- **SFX**：chromatic aberration, motion blur, tilt blur, ray tracing, cel shading, scan lines, posterization, moire patterns, glitch（Digital 页 Glitchy 组）
- **Niji 专用**：kawaii, animecore, fairycore, angelcore, dark fantasy, dreamy, vaporwave, whimsical illustration, marker art, Japanese painting, flat design, hyperdetailed
- **数字/渲染**（Digital.md，465 词）：Octane, Unreal Engine, Blender Render, C4D, Vray, Zbrush, 4k/8k, 8-bit, 16-bit, pixel art, glitch（Glitchy 组）
- **材质**（Materials.md，543 词）：wooden, oak-wood, cardboard, kraft paper, washi；组级词库：Glass and Crystal / Metal / Ice and Snow / Slime and Putty；材质属性（Material_Properties.md）：translucent, iridescent（Chromism 组）, bioluminescence（Luminescence 组）, subsurface scattering（Optics 组）
- **科幻/奇幻主题**（Themes.md Sci-fi 组）：sci-fi, alien, UFO, magicpunk, aetherpunk, decopunk, dracopunk, fairycore, angelcore, ghostcore, cryptidcore, supernatural

### 4.3 词序、强度与排除

- 同族词是强度梯度（Realistic → Hyperrealistic → Photorealistic），**选一档即可**；要更强效果换更高档词，而不是叠三个同族词。
- 词序即权重：主体与最重要的风格词放最前；光照/色彩/相机等修饰放中后段。
- 排除用 `--no {词}`（如 `--no text, watermark`）；MJ 无负向提示词栏，Danbooru 式排除串不可迁移。
- 生僻维度（Emoji、歌词、自造词、Unicode 符号）效果随版本漂移大，生产环境慎用。

### 4.4 组装模板

```
写实照片：{主体场景}, {摄影类型}, {胶片/镜头}, {光照}, {色彩}, {视角} --ar {比例} --v {版本}
风格插画：{主体场景}, {媒介}, {艺术家/流派}, {设计风格}, {光照}, {调色板} --ar {比例} --stylize {值}
动漫(Niji)：{主体场景}, {Niji主题词}, {Niji媒介词}, {光照}, {色彩} --niji {版本} --ar {比例}
概念/抽象：{概念词(Intangibles)}, {几何/维度词}, {材质+SFX}, {色彩} --chaos {值}
```

## 5. 差异与注意点

- **版本时效**：素材主体是 **MJ V4 alpha 时代**实测词表（V5 页未下载）。V6/V7 对自然语言理解更强、部分生僻词（Emoji、歌词、自造词）效果已变；高频经典词（光照/媒介/胶片）仍通用。引擎应把本库当"候选词池"而非"保证生效清单"。
- **MJ vs NAI/SD**：MJ 无权重语法（靠词序与 :: 多提示词高级用法）、无正向负向之分（排除用 `--no`）；Danbooru 标签（1girl 等）在 MJ 中基本无效，两套词库不可混用（见《标签体系与咒语.md》）。
- **Niji 词表是 MJ 词表的子集+动漫特化**：26 个主题词远少于 MJ 主库，但 kawaii/fairycore 等"核系"词在 Niji 中效果显著；Niji Artists 页只保留 5 组（含 Manga 组）。
- **许可证未声明**：仓库无 LICENSE、README 无许可声明。**归纳学习、少量引用合理，但引擎不宜整库复制词表再分发**；建议引擎只存"分类+精选词+回源路径"。
- **示例图缺失**：原库每词配图是其核心价值，本库未下载图片；词的效果梯度（如 Dim vs Dark Lighting）无法离线核对，需要时回源仓库看图。
- 实验页（自造词、乱码 prompt、歌词）属趣味探索，生产提示词不建议取用。

## 6. 缺口

- **参数体系**（--ar/--v/--stylize/--chaos/--weird/--sref/--cref 等）无素材：Comparison_Pages 以图为主未下载，需另收 MJ 官方文档或参数实测素材。
- **整条 prompt 范例库**缺失：本库只有词，没有"完整提示词→成图"案例；可补 MJ 官方 showcase 或社区 prompt 集。
- **MJ V5/V6/V7 与 Niji V5/V6 页**未下载，动漫模式新词（--niji 6 的 style 参数等）无覆盖。
- **sref/cref（风格/角色参考图）工作流**无素材，这是当前 MJ 保持一致性的主流手段。
- 许可证状态不明，商用集成前需回源确认。