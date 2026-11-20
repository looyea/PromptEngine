# 构图与分镜（NAI v5 方法论 + 构图标签）

> 一级类：A AI绘画/动漫 · 二级：美术
> 素材：`素材/github/nai5-prompting/skills/`（GPL-3.0，11 技能包）、`素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/`（AGPL-3.0）、`素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/MJ_V4/Style_Pages/Just_The_Style/Perspective.md`、`Camera.md`（未声明许可）
> 用途：引擎处理"画面怎么取景、动作选哪一刻、漫画怎么分页分格、照图反推"的方法与模板原料。这是 A 类中唯一的**系统方法论**素材（其余多为词表）。

## 1. 概述

nai5-prompting 把插画/漫画创作拆成 11 个可独立调用的技能，形成两条流水线：

- **单幅插画**：nai5-ideas（主题故事种子）→ nai5-scene-design（环境空间）→ nai5-art-direction（情绪光色媒介）→ oc-character-design / oc-costume-design（角色服装）→ **nai5-illustration-composition（想法转画面）** → nai5-writing（转 NAI v5 提示词）。
- **漫画**：nai5-comic-storyboard（故事架构+页格布局+连续性）→ nai5-comic-writing（分镜编译为逐格提示词）；另有 nai5-reverse-prompt（照图反推）与 nai5-costume-writing（服装片段）。

构图方法论的核心立场：**先定"第一眼应看见什么"，再选镜头；先选"动作的哪一刻"，再摆身体；格形必须容纳镜头**。配套词表来自 Danbooru 构图标签（composition/perspective/format 等 YAML，中文对照）与 MJ 视角/相机词页。

## 2. 共性结构与模式

### 2.1 单幅构图决策链（nai5-illustration-composition）

```
意图（这张画要让人看懂什么）
→ 第一视觉焦点 + 其余元素如何衬托
→ 镜头组合：景别 × 机位高低 × 观看方向 × 裁切 × 遮挡 × 透视 × 主体占幅 × 前中后景
→ 瞬间选择：将起（意图）/ 进行中（变化）/ 接触（受力交接）/ 收势（结果）
→ 身体与物件：从动作目标安排躯干倾转、肢体到达、接触与支撑
→ 可见后果：柔软物滞后弯折、连接限制运动、光影随受光面变化
→ 取景核对：必看部位是否入镜；装不下就指出冲突，不静默裁掉
→ 光色气氛：主光方向性质 + 第二光源/补光 + 冷暖明暗 + 材质边缘
```

要点：不强制戏剧机位（等大正面全身展示也成立）；不强制人人运动（安静站立/空镜成立）；多人分"谁发起/谁接受/谁旁观/谁不参与"；变化方案靠改观看距离/遮挡/瞬间/主次，**不靠凑元素**。

### 2.2 漫画分镜三层（nai5-comic-storyboard）

1. **故事层**（comic-story.md）：先固定故事任务；分开"事件顺序"与"揭示顺序"；按阅读节拍拆格；镜头连接控制节奏（格间省略由动作或可理解逻辑支撑）。
2. **布局层**（comic-layout.md）：每页有"入口→重点→出口"；格形跟随镜头任务（建立环境用大横格、横向递物用横细节格、站姿落下用竖格、反应线索用小格、情绪揭示用大面积留白）；读序默认左→右、上→下，四格默认 2×2；区分裁切/破框/跨栏/前景覆盖四种手段——**破框四决定**：阅读目的、几何来源（越界部位属于本格人物）、越界区域（不遮邻格入口/气泡）、时间归属（仍是本格同一瞬间）。
3. **连续性层**（comic-continuity.md）：五层记录——身份/本场服装/道具结构/状态/空间；同一可见部件复用相同描述；画外事实保留在记录里不塞进每格；黑白漫画逐对象做色彩转换（原色另存）；契诃夫之枪先建立功能再生效。

### 2.3 分镜→提示词编译（nai5-comic-writing）

每个"生成单元"（整页或拆组）导出：主串写去重人数+布局模式，**每个画格用一个 Character 框容器**（写格位置/大小/视图镜头+本格可见人物动作状态+必要环境+文字）；人数按不同身份去重（同一角色四格仍 `1girl`，多格不加 solo）；角色位置用 NAI v5 画布 Custom 摆放（不再用 v4 的 5×5 网格格名）；文字只在需要的格出现（引号原文+载体）；UC 预设换 Light/None。

### 2.4 照图反推（nai5-reverse-prompt）

三种来源分开标：**原文**（图内元数据）/ **用户给的** / **推测**（不能说成原文）。范围可只推一部分（构图/光影/服装/姿势…）。看图按字段落点表分配：人数物件→主串前部词组；姿势视线→主串（多人按 Character N 行）；镜头场景光色→主串环境段；接触遮挡相对位置→关系句；稳定外貌服装→各角色栏。机位高低与人物低头抬头**分别判断**（看顶面/底面/地平线线索，近大远小不能单独定仰俯）；看不清的保留缺口写进附注，不在提示词里假装确定。

## 3. 代表样本

**样本 1：镜头服务意图**（`素材/github/nai5-prompting/skills/nai5-illustration-composition/references/composition.md`）
```
要强调高度，可把机位放低，让近处结构从画边向上延伸；要看桌面分工，
可从斜上方同时看清物件与各人的手；要从隐蔽处观察，可让门边或近处
物件遮住部分视野，人物留在后方开口里。近景可只露局部，中景承担
主动作，远处人物可以缩小。
```

**样本 2：瞬间选择**（同上文件）
```
递物停在手刚伸到的瞬间、想接又收回的手、另一个人故意看向别处，
都比泛泛的"互动"具体。……动作将起时，视线、准备姿态和与目标的
距离可以提示意图；进行中可借身体与物件的方向、相对位置表现变化；
接触时可突出受力或交接关系；收势时可通过留下的姿态、物件状态和
人物反应表现结果。
```

**样本 3：格形任务表**（`素材/github/nai5-prompting/skills/nai5-comic-storyboard/references/comic-layout.md`）
```
建立环境与相对站位 → 较大横格或主格（先查背景是否必要、人物是否清楚）
横向伸手、递物     → 横向细节格（交互同处一格，手臂来源与距离成立）
站姿、落下、上下距离 → 竖格（身体与动作方向不被无故截断）
单人反应、物件线索 → 小格或插格（视觉重点单纯，文字不挤占表情）
情绪停留或重要揭示 → 大面积、留白或全宽格（强调值得，不凑版式）
```

**样本 4：NAI/Danbooru 镜头标签**（`素材/github/nai5-prompting/skills/nai5-writing/references/illustration.md` §3.3 + `素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/perspective.yaml`）
```
景别: full body / upper body / cowboy shot(七分身) / close-up / portrait / wide shot
机位: from below / from above / from side / from behind / straight on /
      three-quarter view / dutch angle(德式倾斜) / pov / profile / foreshortening
特殊: fisheye(鱼眼) / blurry foreground(前景模糊) / multiple views(多视图) /
      three sided view(三视图) / cut-in(插入画面) / first-person view
```

**样本 5：构图方式标签（版式类）**（`素材/github/danbooru-diffusion-prompt-builder/data/tags/image-composition/composition.yaml`）
```
border(边框) / framed(画框) / outside border(画到框外) / rounded corners(圆角)
viewfinder(相机取景框) / reference sheet(设定图, alias: character sheet)
character chart(人设图) / diagram(图表) / relationship graph(关系表)
```

**样本 6：MJ 视角词（对照用）**（`素材/github/MidJourney-Styles-and-Keywords-Reference/Pages/MJ_V4/Style_Pages/Just_The_Style/Perspective.md`）
```
Worms-Eye View, Aerial View, Extreme Closeup, Epic Wide Shot, Low Angle,
High Angle, Isometric, One/Two/Three-Point Perspective, Forced Perspective,
Cross-Section, Cutaway, Exploded-View, Miniature Faking, Brenizer Method
```

## 4. 可复用模板骨架

### 4.1 单幅构图方案模板（构思阶段交付，自然语言）

```
画面意图：{要让人看懂的一句话}
第一焦点：{主体+位置}；衬托：{次要元素安排}
镜头：{景别} + {机位高低/方向} + {裁切/遮挡} + {主体占幅}
瞬间：{将起/进行中/接触/收势}，关键物件此刻{位置/朝向/支撑}
人物关系：{谁发起/接受/旁观}，表情视线{…}
光色：{主光方向性质} + {补光/第二光源} + {冷暖对比}
必看细节核对：{用户指定细节} 是否入镜
```

### 4.2 镜头词速查槽（NAI/Danbooru 语法）

```
{景别: full body|upper body|cowboy shot|close-up|wide shot},
{机位: from below|from above|from side|from behind|dutch angle|pov},
{占幅用句子写: she occupies the lower third of the frame, …}
```
规则（nai5）：机位归 tag，占画幅比例归句子；只要低机位不额外加脚部焦点；`portrait` 是人物近景、`landscape` 是风景题材，画幅宽高另在参数/附注交代。

### 4.3 漫画页分镜模板（文字分镜交付格式，comic-story.md）

```
一、剧情结构与必须保留的节点
二、建议总页数、分格数及每页核心内容
三、逐页逐格：
  页N-格k：{格形/大小/位置} | {景别+机位} | {可见人物+动作+状态}
          | {必要环境} | {文字：对白/拟声(引号原文)} | {承接上格的出口→本格入口}
连续性记录（不随格交付）：身份/服装/道具/状态/空间 五层
```

### 4.4 漫画提示词导出模板（NAI v5）

```
Base Prompt: {去重人数}, {布局模式如 2x2 comic, full color}, {全局场景/光}
Character 1:（框容器=格1）{格位置/大小}, {视图镜头}, {本格可见人物动作状态}, {环境}, "{文字}"
Character 2:（框容器=格2）…
附注：角色位置——Character Positions 改 Custom，框1左上/框2右上/框3左下/框4右下
UC 预设换 Light 或 None；图内有字→关质量词开关
```

### 4.5 多人互动模板（单幅，NAI v5）

```
主串: 2girls, {物件}, {场景光线}, {一两句英文: 谁在左右、谁对谁做什么},
Character 1: girl, {动作}, source#hug, {表情视线},
Character 2: girl, {动作}, target#hug, {表情视线},
（互相动作双方都写 mutual#holding hands / mutual#eye contact）
```

### 4.6 照图反推工作流模板

```
1 查元数据：有原文→声明"带原文"，默认照原文交付；要求盲推→先只看图
2 定范围：整张 or 局部（构图/光影/服装/姿势/文字…）
3 按字段落点表分配可见内容 → 写主串+角色栏
4 方向判断找可见依据（手背/掌心、顶面/底面）；不足则留缺口
5 原图回核：每个方向判断有依据；存疑项在提示词中保持未定
6 附注标"推测/看不清"，不冒充原文
```

## 5. 差异与注意点

- **NAI v5 vs v4**：角色位置从 5×5 网格格名变为自由画布；漫画人数按身份去重而非格数；`multiple views`/`halftone`/`screentone` 在 UC Heavy 预设里会压掉分格——画漫画必须换预设。这些是 v5 特有坑。
- **方法论 vs 词表**：nai5 是"怎么想"（GPL 技能文档），Danbooru/MJ 是"用什么词"。引擎应把 nai5 的决策链做成组装流程，把标签词做成槽位候选，二者不可互相替代。
- **MJ 对照**：MJ 无角色栏/分格容器概念，漫画分镜需逐格单独生成或靠 `--sref` 一致性（无素材）；MJ 视角词（Isometric、Cutaway）与 Danbooru 标签语义相近但词形不同。
- **精确修改契约**：nai5 要求局改只动指定范围、逐字保留原串（含空格权重），连带改动"只列不改"。引擎做提示词迭代时应继承此契约，避免顺手重写。
- **许可证 GPL-3.0**：nai5 方法可归纳转述（本篇即归纳），但整段复制 SKILL/references 进分发产品有传染风险；建议引擎存"流程骨架+规则要点"，细节回源。
- 素材中的创作立场（不强制戏剧机位、不凑元素、安静画面也成立）是对"提示词堆料"倾向的纠偏，引擎组装时应保留该约束。

## 6. 缺口

- **视频/动态分镜**（运镜脚本、动画 keyframe、影视 storyboard）无素材；nai5 只覆盖静态页漫/条漫。
- **SD ControlNet 构图控制**（openpose/depth/canny 骨架图工作流）为零，构图目前全靠文字。
- **MJ 分镜/多格一致性**（--cref、角色一致性工作流）无素材。
- **条漫（webtoon）垂直滚动节奏**仅在 comic-layout 提及媒介差异，无专门实例。
- 构图示例图全部未下载（nai5 仓库图片未收），方法对应的成图效果需实测建立"已验证构图库"。