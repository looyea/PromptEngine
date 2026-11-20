# 视频生成提示词（Kling / Veo / Sora / Runway / Seedance 平台公式）

> 一级类：V 影视/视频生成 · 二级：美术（镜头语言）+ 工作流
> 素材：`素材/github/visual-skills/video/`（CC-BY-4.0，需署名 Serge Shima / smixs/visual-skills）、`素材/github/lanshu-awesome-ai-video-kit/`（MIT，21 篇中文方法论 + 494 条 prompts JSON + 7 Skill）
> 用途：引擎按目标平台组装视频生成提示词的公式骨架、运镜词表与约束块。剧作/分镜层见同目录《剧作与分镜.md》。

## 1. 概述

两份素材互补，构成"通用规则层 + 平台语法层"：

- **visual-skills（英文，剧作优先派）**：主张"语法坐在通用规则之上，不替代规则"。`universal-rules.md` 给出跨模型 U1–U14 规则；`kling.md`/`veo.md`/`seedance*.md` 给各平台精确语法、失败模式与修复；`fixes-and-skeletons.md` 给跨模型骨架、压缩顺序与输出格式。核心立场：视频模型奖励**具体物理指令**，惩罚抽象词、矛盾与标签堆砌。
- **lanshu（中文，官方公式派）**：按火山方舟/快手/OpenAI/Google 官方指南整理。Seedance 四任务类型 + 8 要素公式、Kling 三套写法、Sora 两套官方写法、Veo 8 元素、后 6 模型（Runway/Pika/Hailuo/Hunyuan/Wan/即梦）速查；`05-运镜词典` 中英对照；`06-约束词清单`、`07-特殊字符规范`、`08-避坑12问` 是中文平台实战避坑主力。
- **prompts/data/all-prompts.json**：494 条结构化提示词，22 个模型、30 个分类（cinematic 80、action 49、product-commercial 43、dialogue-driven 30、image-to-video 29…），每条含 id/model/category/prompt/params/source.tier。**tier 分级**（official / official-platform / reputable-3rd-party / unverified-3rd-party）可直接用作引擎选例的可信度排序。
- ⚠️ **Sora 2 已于 2026-09-24 停服**（仓库明确标注），其 Shot List/参数化写法仅作结构参考，不作为可执行平台推荐。

## 2. 共性结构与模式

### 2.1 通用骨架与核心规则（visual-skills U1–U14 归纳）

所有平台共享的层序（U1）：**主体 → 动作 → 场景 → 镜头/焦段 → 光影 → 风格 → 声音 → 时长/画幅 → 连续性规则 → 负向约束**。配套规则：

| 规则 | 内容 |
|---|---|
| U2 权重前置 | 前 30-40% token 注意力最高：主体+动作开头，风格修饰结尾，镜头/光影居中 |
| U3 展示不讲述 | 情绪必须译成身体动作；一次情绪转变给 2-4 个可观察线索（下颌/手/呼吸/眼神） |
| U4 自然语言 | 视频模型不是图像模型，"masterpiece, 4k, cinematic" 标签堆砌无效，写完整电影化句子 |
| U5 单一运镜 | 每镜头一个主运镜，最多叠一个微调（轻微手持/缓焦点转换） |
| U6 焦段语言 | 24mm 广角沉浸 / 35mm 纪实 / 50mm 亲密 / 85mm 人像压缩 / 100mm 微距 / 40mm 变形宽银幕 |
| U7 角色锚 | 身份块（脸型/瞳色/肤色/发型/服装/配饰）放每条 prompt 开头；多片段序列**每条重复完整身份块**（模型无跨次记忆） |
| U8 无矛盾 | "静水+流水""特写+广角全景"必出伪影 |
| U9 物理后果 | 写动作的后果而非仅动作：轮胎掀起水幕、瓶盖真的拧开；动作链互为质检 |
| U10 时长纪律 | 默认 5-10s/条；30s=6×5s 拆条剪辑。例外：Seedance "Cut to" 单条多镜、Kling 3.0 至 6 镜 15s、Seedance 2.5 单段 30s |
| U11 终帧规则 | 每条必须写明结尾画面（情绪目的地），"end on his face frozen in blue fridge light" |
| U12 三细节审计 | 每镜头至少：1 环境压力（冷冰箱光/湿沥青/闪烁灯管）+ 1 身体微动作（指节发白/吞咽）+ 1 声音锚或视觉母题（2.3s 处胃鸣/玻璃倒影）。零细节=废镜头 |
| U13 参考角色纪律 | 每个参考素材写明"定义什么/忽略什么"，逐一绑定（"Character A 对应 @Image1，仅取外观"），禁止集体指代 |
| U14 优先级声明 | 过载时声明：必保主体 > 必保镜头 > 可自由发挥的过渡 > 强制终帧 |

**禁用空词**：cinematic / professional / high quality / masterpiece / stunning / epic / beautiful lighting / dynamic camera / "he is sad"。全部替换为具体物理事实。

### 2.2 图生视频通用律

只写**运动、镜头、光变化、声音**，不重描述图中已有内容；加 "preserve X"（剪影/标签文字/服装）连续性指令；20-40 词短 prompt。Kling/Veo/Hunyuan/Seedance 四家文档一致强调此点。

### 2.3 中文平台符号与语言规范（lanshu 07，Seedance 系）

| 信息类型 | 符号 | 示例 |
|---|---|---|
| 音乐 | `( )` | （背景播放快节奏摇滚乐） |
| 音效 | `< >` | <远处传来狗叫声> |
| 台词 | `{ }` | 轻声说{好久不见}；外语须标语种：用日语说道{こんにちは} |
| 字幕 | `【 】` | 【第二章：重逢】 |

台词语言统一、避免中英混用；多音字/生僻字读错时换同音常用字（"鳞龙山"→"吃龙山"，仅缓解）。

### 2.4 约束词模式（lanshu 06）

基础三约束：不要水印 / 不要 Logo / 保持无字幕。按问题追加：面部与比例稳定、动作连贯无穿模、风格不漂移、禁止双胞胎分身、镜头平稳、片尾音频淡出。**矛盾约束不叠加**：8mm 胶片感+4K 超清、film grain+ultra sharp、slow motion+high speed action、>2 个艺术家风格。

## 3. 代表样本

**样本 1：Kling 3.0 多镜头对白**（`素材/github/visual-skills/video/references/kling.md` §12，节选）
```text
[Character A: Investigator — late 30s, navy raincoat, tired blue eyes, stubble]
[Character B: Witness — early 20s, oversized hoodie, hands around a paper cup]
Master intent: a 12-second interrogation in a fluorescent-lit break room at 2am.
Shot 1 (0-3s). Two-shot, eye level, 35mm. Static frame.
Shot 2 (3-6s). OTS over A's shoulder. B's hands tighten on the cup.
Shot 3 (6-9s). Profile CU on A. He slides a photo across the table.
[Character A, low even voice]: "You were there."
Shot 4 (9-12s). Reverse — CU on B. She does not look up.
Immediately, [Character B, fragile broken voice]: "I didn't see anything."
Camera. Locked frames. Lighting. Cold fluorescent, slight flicker.
Audio. Distant radio chatter, tube buzz. No music.
```

**样本 2：Veo 散文体商业镜头**（`素材/github/visual-skills/video/references/veo.md` §10）
```text
A woman in a cream silk blouse stands before a morning window, lifting a
ceramic coffee cup to her lips. Medium close-up, 85mm, slow push-in.
Warm window key from frame-left, soft bounce fill from frame-right.
Cinematic naturalism, creamy palette, shallow depth of field.
Audio: distant city ambience, ceramic clink, one slow breath.
Says: She whispers to herself, "One more minute."
SFX: (spoon tapping ceramic at 2 seconds).
Duration: 6 seconds.
```

**样本 3：Sora 2 官方 Shot List 样板 so-001**（`素材/github/lanshu-awesome-ai-video-kit/prompts/data/all-prompts.json`，tier=official；平台已停服，仅作写法参考）
```text
Style: Hand-painted 2D/3D hybrid animation, warm tungsten lighting,
mid-2000s storybook aesthetic, watercolor wash, filmic motion blur.
Inside cluttered workshop with gear-laden shelves. Small round robot,
dented body, glowing blue eyes, fiddles with a humming light bulb.
Cinematography: Medium close-up, slow push-in, 35mm, shallow DOF.
Warm overhead practical key; cool window spill. Whimsical, suspenseful.
Actions:
- Robot taps bulb; sparks crackle
- Flinches, dropping bulb, eyes widen
- Bulb tumbles in slow motion; catches it
- Robot says: "Almost..."
Background Sound: workshop hum, diegetic only. No added score.
```

**样本 4：Seedance 中文多镜头（8 要素落地）**（`素材/github/lanshu-awesome-ai-video-kit/methodology/02-进阶公式.md` 官方实战例，节选）
```text
@图片1 中的女孩作为主角，@图片2 作为宿舍场景风格参考，参考 @视频1 的运镜方式。
镜头1：傍晚，女孩@图片1 脚步轻快走到宿舍门口@图片2，中景平稳跟拍，
       暖黄日光洒进走廊，她在门口停顿、深呼吸，表情略带紧张。
镜头2：推门进宿舍，切室内中景，舍友抬头笑问{考得怎么样啊，过了吗}，
       镜头在几人间缓慢切换半身特写。
镜头3：她先低头落寞，近景；随后抬头憋不住笑，哈哈大笑说{骗你们的}，
       镜头缓慢拉远，定格在欢声笑语的全景。
全程高清电影纪实风，色调温暖；面部稳定不变形，动作自然流畅，无字幕水印。
```

**样本 5：Veo JSON 结构（防概念串色）**（`素材/github/visual-skills/video/references/veo.md` §6，schema 归纳）
```json
{ "version": "veo-3.1",
  "output": { "duration_sec": 8, "aspect_ratio": "16:9" },
  "global_style": { "look": "...", "color": "...", "mood": "..." },
  "continuity": { "characters": [{"id": "man", "description": "..."}],
                  "props": ["..."], "lighting_constant": "cold fridge light as key" },
  "scenes": [ { "id": "01", "start": "0.0", "end": "3.0",
    "shot": {"type": "MCU", "camera": "slow push-in, 50mm"},
    "action": "...", "lighting": "...", "audio": "..." } ] }
```
用途：多场景、严格道具/角色连续性、散文 prompt 加情绪词导致主体变色时切 JSON。

## 4. 可复用模板骨架（引擎原料）

### 4.0 平台选型速查

| 需求 | 首选 |
|---|---|
| 多镜头对白+口型、中文剧情 | Kling 3.0（Turbo 预算口型 / Omni 参考编辑） |
| 原生音画同步、多人对话、精确音效 | Veo 3/3.1 |
| 多模态参考/视频编辑/延长、中文多镜头 | Seedance 2.0/2.5 |
| 编辑已有视频（加/删/换/重打光/换风格） | Runway Aleph |
| 创意特效爆款 | Pika 2.5（Pikaffects） |
| 极致物理（水火烟布料） | Hailuo 02 |
| 数字人播报口型 | Wan 2.7 |
| 国风中文/剪映闭环 | 即梦 3.0 |
| 开源本地/LoRA | Hunyuan 1.5 |

### 4.1 平台无关通用骨架（"镜头主体+运动+光影+风格+时长"式）

```text
[主体/角色：{{身份锚——脸型/发色/服装/配饰，2-3 个稳定特征}}]
[动作：{{一个主动词短语 + 物理后果}}]
[场景：{{地点+时间+3-5 个环境元素，含 1 个环境压力细节}}]
[镜头：{{景别}} + {{单一运镜}} + {{焦段}}]
[光影：{{具体光源+方向+色温}}]
[风格：{{媒介/年代/色调，放尾部}}]
[声音：{{环境音/音效/台词（平台语法见下）}}]
[时长/画幅：{{5-10s}} / {{16:9|9:16|1:1}}]
[连续性：{{必须保持不变项}}；终帧：{{明确的结尾画面}}]
[约束：{{平台负向语法}}]
```
自检：三细节齐全？无矛盾？无空词？终帧已写？（审计清单见《剧作与分镜.md》§4.7）

### 4.2 Kling 1.x–2.x 文生视频

```text
{{主体+身份锚}}. {{一个干净动词短语的运动}}. Scene: {{3-5 环境元素}}.
Camera: {{单运镜+焦段}}. Lighting: {{光源+性质}}. Atmosphere: {{情绪}}.
Negative field: blurry, distorted hands, extra fingers, melted face,
watermark, subtitles, jitter.
```
规则：2.5 Turbo ≤3-4 元素/50-80 词；2.6 Pro 5-7 元素；1.6 极简。**负向栏写实体词不写 "no X"**（栏位自动解释为排除）；负向堆太长会削弱运动与细节。动作写时序流（起→中→止）并给显式 endpoint（"then settles back into place"，防死循环）。

### 4.3 Kling 3.0 多镜头+对白（P1–P4 协议）

```text
[Character A: {{名+完整身份块}}]
[Character B: {{名+完整身份块}}]
Master intent: {{一句话戏剧意图}}.
Shot 1 (0-{{t1}}s). {{景别+角度+焦段}}. {{主体+动作}}.
Shot 2 ({{t1}}-{{t2}}s). {{不同景别/角度}}. {{动作}}.
[Character A, {{音色情绪}}]: "{{台词}}"
Immediately, [Character B, {{音色情绪}}]: "{{台词}}"
Camera. {{总体运动策略}}. Lighting. {{光源}}. Audio. {{环境音}}. No music.
```
P1 每角色唯一标签；P2 台词前绑定唯一视觉动作（防口型漂移）；P3 音色写进说话人标签；P4 句间加衔接词（Immediately/Then）防台词重叠。每镜必须"景别+主体+运动"，相邻两镜景别与角度都相同会被合并成一镜。各镜时长总和 ≤15s。API：cfg_scale 0.3-0.4 创意 / 0.7-1.0 严格；"no music" 不可靠，需干净音轨就关 generate_audio 后期配。

### 4.4 Veo 散文体 / JSON

```text
{{主体+动作}} in {{环境}}. {{景别+焦段+运镜}}. {{光方向+色温}}. {{风格+色调}}.
Audio: {{多层声音——底噪+定向音效}}.
Says: {{角色}} says{{, in a {{语气}} voice}}: "{{台词，≤8 秒语速}}".
SFX: ({{定点音效}}).
Duration: {{4|6|8}} seconds.
```
规则：50-200 词甜蜜点；对白必须"引导动词（says/whispers/shouts）+双引号"，冒号式更稳；音频必须显式 `Audio:/SFX:/Says:` 标签否则可能无声；镜头指令放句首更有效；复杂连续性切 §3 样本 5 的 JSON schema（mood 隔离在 global_style，防串色）。3.1 图生视频加 "maintain the subject from the first frame"；多参考用 reference_1/2/3 逐一指代。

### 4.5 Sora 式 Shot List（写法参考，平台已停服）

```text
Style: {{视觉锚点+美学传承+胶片质感——最强杠杆}}
Inside/At: {{场景+关键物件}}
Cinematography: Camera {{framing+运动+镜头规格}} / Lighting {{主光+补光}} / Mood {{1-2 词}}
Actions:
- {{Beat 1}}
- {{Beat 2}}
- {{角色}} ({{语气}}): "{{短台词}}"
Background Sound: {{仅 diegetic 场景内声音}}. No added score.
```
要点：Beats>Seconds（模型对先后顺序比绝对时间敏感）；80-150 词最优，>200 词出视觉幻觉；每 4s  clip ≤2-3 行短对白。参数化变体（Format & Look / Lenses / Grade / Lighting / Framing / Wardrobe / Sound）适合复刻胶片质感。

### 4.6 Seedance 四任务句式 + 8 要素

任务判定：只有文字→文生视频；有图/视频/音频做新片→`参考 <图片N> 中的 <主体>，生成…`；改原片→`严格编辑 <视频N>，将 <原特征> 修改为 <新特征>`（**勿写"参考"**，会被误判）；续写→`向前/向后延长 <视频N>`；组合→`参考 <图片N> 的 [维度]，严格编辑 <视频X>…`。视频输入 ≤3 段、总时长 ≤15s。

```text
将 <图片1> 中的 {{2-3 个稳定静态特征}} 定义为 <主体1>（多主体分别定义，全程用标签指代）。
镜头1：{{运镜/切换}} + {{主体动作+表情（部位+幅度/速度/力度）}} + {{空间位置}} + {{音频}}
镜头2：…（按事件顺序，不强制每段秒数；台词用{}，音乐()，音效<>）
整体：{{风格}} + {{光影色调}} + {{画质}}；
约束：面部稳定不变形；动作连贯；风格不漂移；无水印Logo字幕；禁止双胞胎分身。
```
素材配置四角色：角色锚定图 1-2（**大头照+全身照，勿用多视图**）+ 场景图 1 + 运镜视频 1 + 音频 1，共 4-5 个，勿用满上限。参考人物 >4 人先分组生图再图生视频。

### 4.7 Runway Gen-4 / Aleph

```text
Gen-4: {{Subject 具体细节}} {{Action 清晰动词}} {{Setting}}
       {{Camera framing+movement}} {{Motion 随时间演化}} {{Style 胶片/年代}} {{Constraints}}
Aleph: {{add|remove|change|replace|re-light|re-style}} {{目标元素+程度}},
       preserve {{其余一切}}, transition {{how natural}}.
```
Aleph 三原则：说什么要改 + 说什么保持稳定 + 说转换程度。

### 4.8 图生视频通用骨架（Kling/Veo/Hunyuan 通用）

```text
Preserve {{剪影/标签文字/服装/主体}}. {{单一运镜+焦段}}.
{{1-2 个运动动词，写图中场景如何演化}}. {{光变化/氛围线索}}.
{{Audio 块（平台支持时）}}. Negative: {{实体词清单}}.
```

### 4.9 多片段拆条链（长叙事）

```text
Clip {{i}}/{{N}}（每条自包含）:
[完整身份块+服装+场景+光照常量——逐字重复]
{{本条镜头内容（4.1 骨架）}}
终帧：{{与下一条首帧衔接的画面状态}}
---
剪辑注：Clip i 结束于 {{X}}，Clip i+1 开始于 {{X 的延续}}，同光照。
```
拆分默认 5s/条；文戏用平台"延长"功能一镜到底，武戏/转折分段拼接。延长衔接跳变：前段尾删 6 帧+后段头删 1 帧；续写以切镜时刻结尾。多次续写画质劣化：控制次数或转白模再续。

### 4.10 运镜词典精选（中英对照，lanshu 05 + visual-skills U6）

- **景别**：EWS 极广角（孤立/宏大）· WS 广角（交代环境）· MS 中景（关系/手势）· MCU 中特写（意图/手部）· CU 特写（情感）· ECU 极特写（纹理/微表情）
- **运动**：dolly in/push in 推（强调）· pull back 拉（揭示/疏离）· pan 横摇 · tilt 纵摇 · crane 升降 · tracking 跟踪 · orbit/arc 环绕（产品/角色介绍）· handheld 手持（纪实/紧张）· gimbal/Steadicam 稳定流畅 · rack focus 焦点转换（引导注意）· slow push-in 缓推（悬念）· aerial/drone 航拍
- **灯光**：golden hour 暖怀旧 · blue hour 冷峻孤独 · overcast 纪实漫射 · hard key 黑色电影 · soft window light 平静亲密 · neon spill 赛博朋克 · volumetric 史诗神圣 · rim/backlight 剪影轮廓 · practical 实景自然 · fluorescent 冷白（办公/医院/压抑）· tungsten 暖黄复古
- **风格**：cinematic/film grain/anamorphic/35mm · documentary/handheld/vérité · cyberpunk/neon-lit · vintage/8mm/VHS · anime/cel-shaded/Ghibli-inspired · ink wash 水墨 · film noir 高对比黑白 · Wes Anderson 对称粉彩
- **色彩分级**：teal and orange（好莱坞标配）· warm/cool grading · desaturated/muted · monochrome
- **角度情绪**（Kling 3.0 理解）：low angle=支配，high angle=脆弱，eye-level=中性——按权力关系选，不随机。

### 4.11 修复速查（失败模式 → 修法）

| 症状 | 修法 |
|---|---|
| 多镜头塌成一镜 | 显式 `Shot n (ts)` / `Cut to.` + 每镜不同景别角度 + "must be multi-shot with visible hard cuts" |
| 角色换脸 | 每条重复身份块；Kling Element Binding 3-4 参考图；Seedance 大头照前置 |
| 双胞胎/克隆人 | 每主体独立绑定参考 + "禁止外形着装完全一致人物" 全局约束 |
| 静态场景镜头乱漂 | "locked static frame / camera fixed, no movement" |
| 口型漂移/台词重叠 | Kling P2 视觉锚+P4 衔接词；Veo 引导动词+砍台词到 8s |
| 手融化/多指 | Kling 负向栏实体词；Seedance/Veo 正向 "anatomically correct hands" |
| 莫名字幕/配乐 | 直接禁止并在尾部重复；Seedance 1.x/Kling **不要提及**不想要的文字/音乐（提及即召唤）；参考素材先去掉文字；横屏生成字幕概率更低 |
| 快速动作形变 | 慢动作生成后期加速 |
| 光照跨条漂移 | 命名主光+方向，每条逐字重复 "Lighting constant: …" |
| prompt 过长被挑拣 | 按压缩顺序砍：先砍哲学/形容词/导演引用，保角色连续性>动作>时码>光>镜头>剪辑语法>声音 |

## 5. 差异与注意点

- **负向语法三派**：Kling 专用负向栏写实体词（"robots" 而非 "no robots"）；Veo/Seedance 2.5 正文 "No subtitles" 可靠；Seedance 1.x 需把负向全部反转为正向措辞。跨平台搬运必须换算。
- **对白语法三派**：Veo `says: "…"`（引导动词+引号）；Kling 3.0 `[Character A, 音色]: "…"`（标签内音色）；Seedance `{台词}`+语种标注。混用会失效。
- **时码分歧**：visual-skills/Kling 3.0 推荐显式 `Shot n (0-3s)`；lanshu/火山官方明确 Seedance **不强制每段秒数**（精确时间支持不稳定），Sora 用 Beats 不用秒。引擎按平台选择：Kling 3.0 写时码，Seedance 写"镜头N"顺序，Sora 式写 beat 列表。
- **时长口径分歧**：visual-skills 记 Veo 3/3.1 单条 4/6/8s；lanshu 记 Veo 3.1 可至 60s（链式延长）、Kling 智能分镜至 2 分钟。单条生成 vs 链式延长口径不同，组装时注明"单条时长"与"成片时长"两个槽位。
- **元素容量**：Kling 2.5 Turbo 3-4 / 2.6 Pro 5-7 / 3.0 结构优先；Seedance 参考人物 ≤4、素材 4-5 个； overload 一律融化。参考/Elements 开启会降画质——一次性片段用 in-prompt 标签更干净。
- **许可证**：visual-skills **CC-BY-4.0，引用/改写须署名**（Serge Shima, github.com/smixs/visual-skills）；lanshu MIT（保留版权声明即可）。JSON 选例优先 tier=official/official-platform，unverified-3rd-party 条目只作灵感不作权威语法依据。
- **Sora 2 停服**：不推荐为执行平台；其 Shot List 结构可迁移到 Kling 3.0/Seedance 多镜头写法。
- 模型版本迭代快（Kling 3.0 Turbo/Omni、Seedance 2.5 均为 2026 新增），语法以素材取证日 2026-11-19 为准，执行前建议核对平台当前文档。

## 6. 缺口

- Seedance 1.5/2.5 精确语法（visual-skills seedance.md/seedance-25.md，21K+31K）与 lanshu 两篇 masterclass 本篇未逐条展开，引擎需要 30s 单段/50 参考位/视频编辑细节时回源读取。
- Runway/Pika/Luma 在 JSON 中样本少（rw 12+2、pk 12、luma 2），Aleph 编辑与 Pikaframes 关键帧玩法缺实测反馈。
- 无生成结果评测数据：494 条 prompt 的 tier 是来源可信度，不是效果评分。
- 音乐/音效设计层薄（music-video 仅 8 条、asmr 7 条），配乐向需求需跨 M 类（音乐）组装。
- visual-skills 的 image/ 分支（图像艺术指导、de-slop、Nano Banana/GPT Image 语法）属 A 类素材，尚未汇总，可另立篇目。
- 即梦/Kling 4.0/minimax-h3 等国内新模型各仅 12-16 条，公式深度不及 Kling 3.0/Seedance。