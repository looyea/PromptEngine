# SD 提示词方法论（原理 · 实战流程 · 术语）

> 一级类：A AI绘画/动漫 · 二级：美术（方法论）
> 素材：`素材/github/StableDiffusionBook/`（GFDL-1.3）——`docs/paint/PromptsEngineering.md`、`docs/paint/Practicalguide.md`、`docs/paint/index.md`、`docs/GettingStarted/term.md`（只取方法论章节，不含安装/训练）
> 用途：引擎组装 SD/WebUI 系提示词的**思考方法、调参流程与修复工艺**。与同目录《标签体系与咒语.md》互补：那篇管"选什么词、权重语法怎么写、整组咒语哪里取"（Danbooru 标签分类/NAI v4 v5 语法/元素法典预设）；本篇管"为什么这样写、画坏了怎么诊断怎么修、参数什么含义"。选词与权重细节一律交叉引用那篇，不重复。

## 1. 概述

StableDiffusionBook 是中文 AI 绘画 Wiki（约 2022-2023 年 SD1.x / NAI 时期沉淀），核心认识论一句话：**提示词是在数据集里做检索限定，不是对模型下自然语言指令**——"限定数据范围内符合期望的样本越多，结果越符合预期"。由此推出全部方法论：

- 用数据集里真实存在的标签（Danbooru）而非自造描述；
- 顺序即优先级（前面的词锚定画面主体与色彩分布）；
- 冲突词把检索拉向不同数据簇，必须排查（分类/风格/次元三类冲突）；
- 画质问题先诊断噪声类型，再动参数，而不是盲目堆质量词；
- 修图靠 img2img/inpaint 工艺 + 降噪强度控制，而非重抽卡。

素材含四章：提示词工程（Token 原理/选词/语法）、实战指南（灵感表→噪声诊断→修图→设计应用）、绘画总览、AIGC 术语表（CFG/采样器/黑话）。

## 2. 共性结构与模式

### 2.1 Token 原理与长度纪律

- CLIP 文本编码器把 prompt 转成 77×768 嵌入；有效 positive token 约 75-80，超出被截断/分组。
- WebUI 允许超 75 token：按 75 一组提交，**组间无上下文**——`blue hair` 恰在边界会被拆成两组导致失效。缓解选项："Increase coherency by padding from the last comma within n tokens"（把边界词组整体挪到下一组）。
- 实践纪律：不堆叠 prompt 是好习惯；内容确实多时适当提高 steps；超 100 词有失败风险。
- 每个词基础权重不同（768 维概念向量），加权=缩放向量强度；权重括号本身占 token 位，token 紧张时少加括号。

### 2.2 选词三档与排序公式

| 档位 | 适用 | 注意 |
|---|---|---|
| Danbooru 标签（首选） | 动漫模型 | 风格要与图像风格同簇（动漫推理别加"乐山大佛"）；标签热度/分类查《标签体系与咒语.md》§2.2、§4.4 |
| 自然语言 | 写实/通用模型 | 准确度受 CLIP 分词限制；避免 with 等连接词与复杂语法 |
| emoji/西方颜文字 | 精确小概念 | 单字符语义准：💐☺️💐 影响构图；`:-)` 微笑 `:-(` 不悦 `;-)` 使眼色 `:-D` 开心 `:-P` 吐舌 `:-C` 悲伤 `:-O` 惊讶（Danbooru 系模型可控表情） |

排序公式（从最重要到最不重要，英文半角逗号分隔）：
```
(quality), (subject)(style), (action/scene), (artist), (filters)
```
quality 利用数据集品控标签（sticker 配 low res 反而"利用"更多数据）；subject 锚定内容；filters 放摄影术语/工作室/特效等补充。

### 2.3 三类 Prompt 冲突（写完必查）

- **分类冲突**：贴纸在数据集里不会打 masterpiece/best quality，而是 normal quality——要 sticker/像素画就移除冲突质量词（正负向都要查）。
- **风格冲突**：`loli` 自带统一画风属性，会压掉你要的 flatcolor——降权或换词。
- **次元冲突**：动漫微调模型避免混入原始 SD 的写实内容词；跨领域术语（摄影/电影词）会引入实景数据，把握量度，必要时加 steps 与风格词拉回。

### 2.4 语法工具箱（细节换算见《标签体系与咒语.md》§2.3）

- 权重：WebUI `(word:1.4)`/`((word))`（每层 ×1.1）、`[word]` 减弱；NAI `{}`（每层 ×1.05）不可指定数值；换算 NAI `[word]`≈WebUI `(word:0.952)`。字面括号须转义 `\(word\)`；过多圆括号会吃掉逗号。
- 人工分割：`/` 或 `+` 强制拉开 token 距离（推荐 `red eyes,/,/,/,/, blue dress`，约 9 格分隔、干扰 ~7%）；相邻重复符号被过滤（`+++`=`+`），连续长串 `++++` 反而干扰后续词 25%。
- 交替 `[a|b|c]`：每 step 轮换一词，做人物/风格混合体。
- 矩阵 `|`：WebUI 对每个组合各出一张图（批量探索）；NAI 中 `cat:2|dog` 是加权平均混合（更像猫的狗）。
- 日程 `[from:to:when]`：第 when 步（或 <1 时按总步数百分比）替换/添加/删除词。**遏制风格污染**：`[style:10]` 让风格词第 10 步才生效——前 10 步先成形主体，风格像 img2img 一样附着而不污染构图。

### 2.5 实战流程模式（Practicalguide 归纳）

```
灵感表四维（人物:表情/头发/眼睛/衣着/状态/姿势/镜头位；场景:广狭/光影/背景/事件地；
数据限定:绘画类型/评价限定/联想元素/2D3D；事件:缩写词/SFW/出品公司/作品品牌）
→ 查数据集标签（Danbooru tag groups）→ 按 2.2 排序 → 三类冲突自查 → 长度检查
→ 出图 → 噪声诊断（2.6）→ 调参或修图（4.3）→ 超分（Real-ESRGAN/realcugan 动漫向）
```
关键参数语义：**CFG 越高越贴 prompt（手电筒光束越窄），降噪越高越偏离原图**；尺寸不与画质挂钩而与"数据类别"挂钩（竖屏人物/横屏风景/小图表情包），大尺寸扭曲开 Highres Fix 或小图+超分。复刻需同 seed+prompt+全参数（对方用了风格化则几乎无法复刻）。

### 2.6 噪声诊断表（画坏了先对号入座）

| 噪声类型 | 现象 | 根因与处理 |
|---|---|---|
| 粗糙 | 细节不够、人物像米糕 | steps 太低 → 提升 steps |
| 波浪 | 密集波浪/图案 | 权重设置太高 → 降权 |
| 模型 | 输出抽象混乱 | 模型炼炸了 → 换模型 |
| 冲突 | 实景里贴扁平动漫纸片 | 混用不同次元/主题词 → 排查 2.3 |
| 彩虹 | 混乱彩虹色 | 生成分辨率太低 → 提分辨率 |
| 尺寸 | 大尺寸下扭曲 | 开 Highres Fix 或小分辨率+超分 |

### 2.7 术语与黑话（term.md 精选）

- **CFG**（Classifier Free Guidance）：提示与输出一致度；0=纯随机，20=激光笔式窄束。
- **CLIP/Clip skip**：文本→嵌入；NAI 出图用 CLIP layer=2（skip 2）。
- **VAE**：潜空间↔像素的编解码器；推理只用解码端（4,64,64 → 3,512,512，压缩系数 8）。
- **ENSD**：eta 噪声种子增量，NAI=31337。
- 黑话：咒语=prompts；施法/吟唱=t2i；魔杖=参数；炼丹/emb=Textual Inversion 嵌入；冶金=hypernetwork；炸炉=过拟合；废丹=训练失败；db/梦展=DreamBooth；超分=AI 提分辨率。
- 采样器/步数/种子等推理概念见 term.md 推理流程节（UNet+Scheduler 在潜空间迭代去噪）。

## 3. 代表样本

**样本 1：排序公式与质量词相对性**（`素材/github/StableDiffusionBook/docs/paint/PromptsEngineering.md`）
```text
(quality), (subject)(style), (action/scene), (artist), (filters)
例：1girl + high quality 利用高质量数据簇；
    sticker + low res 反而利用贴纸数据簇（贴纸不会被标 masterpiece）。
顺序=优先级：前面的词锚定画面主体与色彩分布。
```

**样本 2：权重与分隔语法**（`素材/github/StableDiffusionBook/docs/paint/PromptsEngineering.md`，节选）
```text
a (word)      → ×1.1        a ((word)) → ×1.21
a [word]      → ÷1.1        a (word:1.5) → ×1.5
a (word:0.25) → ÷4          a \(word\) → 字面括号
red eyes,/,/,/,/, blue dress   （9 格分隔，干扰 ~7%，推荐）
red eyes++++++++++++++++blue dress （不推荐，干扰后续词 25%）
[fantasy:cyberpunk:16]  第 16 步起 fantasy→cyberpunk
[style:10] / [x:0.6]    第 10 步 / 60% 进度才引入风格（防污染）
```

**样本 3：表情包/贴纸完整配方**（`素材/github/StableDiffusionBook/docs/paint/Practicalguide.md` "使用 Ai 进行设计"）
```text
正向：1girl, sticker, Chibi, Q, white background
（masterpiece,best quality 可加可不加；lowres 不够复古就加；Chibi 不够Q就加）
负向：bad anatomy, bad hands, text, error, missing fingers, extra digit,
fewer digits, cropped, worst quality, jpeg artifacts, signature,
watermark, username, blurry, bad feet, 3D
（想更平面化就把 3D 加进负向）尺寸：正方形。不要对贴纸强求质量词。
```

**样本 4：NAI 出图参数基准**（`素材/github/StableDiffusionBook/docs/paint/Practicalguide.md` "参数"）
```text
steps 28, scale(CFG) 11.0, strength(eta) 0.69, noise(sigma) 0.667,
CLIP layer=2, ENSD=31337, ema 权重, 全量模型
SFW 负向：lowres, bad anatomy, bad hands, text, error, missing fingers,
extra digit, fewer digits, cropped, worst quality, low quality,
normal quality, jpeg artifacts, signature, watermark, username, blurry
（与《标签体系与咒语.md》样本 1 的 NovelAI 官方排除串同源，可互验）
```

**样本 5：草图迭代工艺**（`素材/github/StableDiffusionBook/docs/paint/Practicalguide.md` "迭代草图"）
```text
首轮：低 steps、低 CFG（要多样性）、denoise 0.3-0.4（保草图）、随机种子
末轮：提 steps 与 denoise（≤0.8，>512px 时更高会毁图）、按需提 CFG 与尺寸
中途可随时增删 prompt 细节、换采样器
线稿修复：AI 只认色彩不认线条 → denoise 降到 0.2
固定种子用于微调；长期固定种子会过饱和/过锐/像素化
```

**样本 6：CFG 手电筒比喻**（`素材/github/StableDiffusionBook/docs/GettingStarted/term.md`）
```text
提示=可变宽度手电筒，照进模型潜空间：
CFG→0：极宽光束，输出几乎来自任何地方（随机）
CFG→20：窄成激光指示器，照亮潜空间一个点（死板贴词）
推论：贴词度与创造性是同一旋钮的两端，按任务取中。
```

## 4. 可复用模板骨架（引擎原料）

### 4.1 SD 文生图五段骨架

```text
正向：{{quality 段——按目标数据簇选，贴纸/像素不加 masterpiece}},
{{subject 主体+人数}}, {{style 画风（可选）}},
{{action/scene 动作+场景}}, {{artist 画师/工作室（可选，伦理见《标签体系与咒语.md》§5）}},
{{filters 摄影术语/特效/平台名等补充}}
负向：{{低质量排除串 + 人体错误串 + 水印文字串 + 本图定向排除（如 comic 2koma 3koma 4koma collage 防分割线）}}
参数：steps {{20-30}}, CFG {{5-11}}, 尺寸 {{按类别：竖屏人物/横屏风景/方形表情包}}, seed {{-1 探索 / 固定微调}}
自查：三类冲突？token ≤75（或开 comma padding）？重要词在前？
```

### 4.2 需求 → 提示词工作流骨架

```text
1 灵感表四维展开：人物（表情/发/眼/衣着/状态/姿势/镜头位）×
  场景（广狭/光影/背景/事件地）× 数据限定（绘画类型/评价/2D3D）× 事件（品牌/公司/缩写）
2 反问："这张图在互联网上会被打什么标签？"→ 用 Danbooru 真实标签替换自造描述
3 按 4.1 排序组装；跨领域术语（摄影/电影/平台名 pixiv）用于缩小数据范围，注意次元副作用
4 冲突自查（分类/风格/次元）→ 长度检查 → 出图
5 不满意：先诊断（4.4），再决定改词/调参/修图，不盲目重抽
```

### 4.3 修图工艺骨架（img2img / inpaint）

```text
手部修复：inpaint 遮手部（或全图），手部描述词前置，
  denoise {{只想补全≤0.25 / 允许重画 0.3-0.5}}，steps/CFG 与原图一致；
  或全分辨率局部修复+低填充，只提示手部细节
去除/替换元素：边缘描细线+色块（阴影取亮色），denoise ~0.3，
  Inpaint+相关词迭代至满意 → realesrgan 超分去纹理
添加元素：PS 贴图/嫁接（移植手、接下半身）→ img2img 润色
草图迭代：首轮 denoise 0.3-0.4 低 CFG → 末轮 ≤0.8 提 steps；线稿 denoise 0.2
多人物：草图/有色 3D 排列定人数与位置 → img2img（>3 人难控，>6 人数据几乎没有）
风格污染：主体词正常写，风格词改 [{{style}}:{{10 或 0.6}}] 延迟生效
不变风格改内容：用 Inpaint 不用 img2img；尺寸靠近原图选 Crop and resize
```

### 4.4 噪声诊断 → 调参决策骨架

```text
输入：{{坏图现象}}
对号：粗糙→+steps；波浪→降权重；抽象→换模型；纸片混实景→查次元冲突；
彩虹→+分辨率；大图扭曲→Highres Fix/小图+超分
语义旋钮：CFG↑=更贴词更死板；denoise↑=更偏离原图；steps↑=更细但边际递减
尺寸=类别选择器不是画质旋钮；画质交给超分（动漫用 Real-ESRGAN/realcugan）
复刻：同 seed+prompt+参数+尺寸；对方有风格化则不可复刻
```

### 4.5 跨平台换算骨架（SD ↔ NAI）

```text
权重：WebUI (w:x) ↔ NAI 层数 n=x 以 1.05 为底取对数（{w}×1.05/层，(w)×1.1/层）
NAI [w] ≈ WebUI (w:0.952)；NAI 不支持数值权重，混合用 cat:2|dog
参数基准（NAI 官方）：steps 28 / CFG 11 / eta 0.69 / sigma 0.667 / CLIP skip 2 / ENSD 31337
语法不通用：{} 是 NAI（WebUI 2022-09 前旧实现），() 是新 WebUI；
NAI v5 的 x::tag:: 新语法与换算见《标签体系与咒语.md》§2.3/§5
```

### 4.6 术语速查块（注入知识密集任务）

```text
CFG=贴词度旋钮（0 随机~20 激光）；CLIP=文本编码器（77×768）；Clip skip=取第几层
VAE=潜空间编解码（系数 8）；ENSD=eta 噪声种子增量（NAI 31337）
采样器=去噪调度算法；steps=去噪迭代数；seed=初始噪声
emb/炼丹=Textual Inversion；hypernetwork/冶金=超网络；DreamBooth/梦展=少样本微调
炸炉=过拟合；废丹=训练失败；超分=AI 提分辨率；咒语=prompt；魔杖=参数
```

## 5. 差异与注意点

- **许可证 GFDL-1.3**：文档型 copyleft——大段复制进分发文档需同许可并保留声明；引擎按"归纳改写+回源"使用（本篇即改写），不整段搬运原文。
- **时代局限**：素材沉淀于 SD1.x/NAI leak 时期（2022-2023）。SDXL/SD3/Flux 等新一代模型自然语言能力大幅提升，"必须 Danbooru 标签、75 token 截断"等结论对新模型不完全成立；WebUI 分组机制、comma padding 选项属 A1111 特性，ComfyUI/NAI 官网不适用。组装时先确认目标模型代际。
- **平台语法三套**：WebUI `()`/`[a:b:when]`/`|` 矩阵 ≠ NAI v4 `{}` ≠ NAI v5 `x::tag::`（v5 详见《标签体系与咒语.md》）。本篇语法默认 WebUI。
- **与《标签体系与咒语.md》分工**：本篇不重复标签分类树、热度选词、v5 字段规范与咒语预设；遇到"选哪个词/权重怎么写/整组咒语"路由到那篇，遇到"为什么/怎么调/怎么修"用本篇。
- 素材内大量外链（B站教程/rentry/Google Docs）与图片未随库保存，示例图缺失（原文即标"缺少示例图片"），工艺描述可复现但视觉效果需自行验证。
- 部分术语表内容偏训练侧（DreamBooth/DeepSpeed），本库未收训练章节，仅作名词释义用。
- 画师名/版权角色词的使用伦理与平台限制见《标签体系与咒语.md》§5，本篇 artist 段同样适用。

## 6. 缺口

- **ControlNet/LoRA/Embedding 训练与触发词素材为零**（原书训练章节未采集），立绘/姿势控制只在实战指南里被提及。
- **新一代模型方法论缺失**：SDXL/SD3/Flux/即梦等自然语言系模型的提示词方法需另采（visual-skills 的 image/ 分支可补，见《视频生成提示词.md》§6）。
- ComfyUI 工作流素材缺失；WebUI 版本演进（如 ADetailer 修手插件）未覆盖。
- 示例图全部未下载，噪声诊断表/风格词表只能凭文字理解。
- 无量化评测：权重倍数、分隔格数（7%/25% 干扰）来自社区经验帖（NGA 等），非系统实验。
- 英文对照版（.en.md）已采集但本篇未比对，术语英译以中文版为准。