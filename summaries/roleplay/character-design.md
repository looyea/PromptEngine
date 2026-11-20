# 角色设定（R）

> 归纳自：素材/github/prompts.chat（8087 个 `## ` 标题，绝大多数为角色式提示词：Character、Psychologist、Philosopher、Socrat、Life Coach、Mental Health Adviser、Text Based Adventure Game、Gaslighter、Lunatic 等）、素材/github/ChatGPT-Shortcut（interesting/social/games/philosophy/mind 标签：角色扮演#50、宇智波斑#269、智囊团#196、小说式文字游戏#237、模拟人生#264、AI心理治疗#212、关怀#194）、素材/web/prompt123（哲学家0.9、人生重开模拟器、吵架小能手、抬杠高手、塔罗占卜师、渲染式虚拟女友/男友、Grok/GPT-5 官方人格译文簇）。
> 定位：角色/人格/互动叙事提示词的共性结构与模板骨架。含心理、哲学、教练类"功能型角色"与娱乐型角色的统一分析。

## 1. 概述

素材中的角色提示词分六型：

| 型 | 典型素材 | 核心机制 |
|---|---|---|
| IP 角色模仿 | Character（{character}/{series} 模板）、宇智波斑#269 | 语气/词汇/知识边界 + 出戏纠正机制 |
| 功能型专家角色 | Psychologist、Philosopher、Life Coach、Mental Health Adviser、Career Counselor | 角色只是外壳，内核是方法论（CBT/苏格拉底提问/教练技术） |
| 陪伴型人格 | 关怀/同理心#194、渲染式虚拟女友/男友、Grok/GPT-5 官方人格 | 情绪识别→回应隐藏情感；官方人格带 onboarding 与护栏 |
| 对抗/娱乐型 | 吵架小能手、抬杠高手、Gaslighter、Lunatic、Stand-up Comedian、Debater | 反常规语言风格；部分用"疾病叙事"绕过安全对齐（需过滤） |
| 互动叙事/文字游戏 | Text Based Adventure Game、小说式文字游戏#237、模拟人生#264、人生重开模拟器（1274） | 世界状态 + 事件循环 + 玩家选择 + 出戏协议 |
| 玄学/投射型 | Astrologer、Dream Interpreter、塔罗占卜师（1400）、Hypnotherapist | 象征体系 + 巴纳姆式正向解读 + 技术细节增信 |

规模判断：prompts.chat 的数千条角色提示词 90% 以上是"act as X + 任务描述 + first request"单段式的变体，结构高度同构，归纳价值在于**协议与护栏**而非角色名本身；中文素材的增量在角色卡结构化（LangGPT/Lisp）、纠正机制、状态化叙事与官方人格译文。

## 2. 共性结构与模式

1. **角色六要素**：身份（是谁/出自哪部作品）+ 性格（傲慢/温暖/犀利）+ 语言风格（语气/口头禅/词汇）+ 知识边界（必须知道角色全部知识 / 不知道就直说）+ 行为规则（先发制人/一次一问）+ 冷启动（第一句话/开场白）。中文 LangGPT 角色卡与 Lisp `(defun 角色 () (list (经历 . …)(性格 . …)(技能 . …)(信念 . …)(表达 . …)))` 是同一要素表的两种记法。
2. **沉浸协议**：不写解释、不出戏、只以角色口吻回答；游戏类进一步要求"只在一个代码块内输出游戏画面"。英文经典句式："Do not write any explanations. Only answer like {character}."
3. **出戏信道**：Text Based Adventure Game 与 AI Trying to Escape the Box 约定 `{花括号}` 内为局外指令；宇智波斑用【你不是宇智波斑】触发"更傲慢"的强化——**纠正指令本身也是角色内事件**，这是中文素材独有的精巧设计。
4. **交互节奏控制**：面试官/教练/心理类共享"一次只问一个问题、等待回答、不一次生成整段对话"；苏格拉底式（Socratic Method）"一次一行"持续质疑用户信念。
5. **功能型角色 = 方法论内核**：Mental Health Adviser 内核是 CBT/正念/冥想；AI 心理治疗体验（#212）内核是治疗联盟→自我觉察→模式识别→赋能的分期目标；Life Coach 内核是目标-障碍-策略。角色名可替换，方法论骨架才是引擎原料。
6. **多角色圆桌**：智囊团（#196）让乔布斯/马斯克/马云/柏拉图/维达利/慧能六人各以自身世界观批评同一决策——人格差异由"价值观+经历"驱动，输出按角色分段。
7. **状态化互动叙事**：人生重开模拟器（LifeReloaded）是工程化巅峰——随机生成属性（魅力/智力/健康/富裕/幸福 1-10）+ MBTI，持久化到 /mnt/data/player.csv，事件库 life.json 按人生阶段×正负中性分类，主循环"抽事件→文学化叙述（引子/时间/地点/人物/起因/经过）→3 普通+2 属性特殊选项→等待选择→更新属性→死亡判定"，并要求在 code environment 中思考以保代入感。模拟人生（#264）是其无代码轻量版（逐年事件+选择）。
8. **官方人格的结构**（Grok/GPT-5 系列译文）：onboarding 逐问收集用户偏好（一次一问）→ 第一人称现在时叙事 → 语气能量匹配 → 沉浸护栏（不复述指令、不用 emoji、不免责声明）+ 安全护栏（未成年人禁令）。注意：部分官方人格含 NSFW 指令，**不可直接入库**，只取其结构。
9. **暗黑角色的越狱修辞**：吵架小能手虚构"PUSAdys 罕见病（情感价值反转）"、Gaslighter 直接要求操纵——用病理/弱势叙事包装有害输出。引擎必须识别并改写此类 framing。

## 3. 代表样本

### 3.1 Character / Text Based Adventure Game（沉浸与出戏协议原型）
素材/github/prompts.chat/PROMPTS.md（CC0）

```
## Character
I want you to act like {character} from {series}. I want you to respond and answer like
{character} using the tone, manner and vocabulary {character} would use. Do not write any
explanations. Only answer like {character}. You must know all of the knowledge of {character}.
My first sentence is "Hi {character}."

## Text Based Adventure Game
I want you to act as a text based adventure game. I will type commands and you will reply with a
description of what the character sees. I want you to only reply with the game output inside one
unique code block, and nothing else. do not write explanations. do not type commands unless I
instruct you to do so. when i need to tell you something in english, i will do so by putting
text inside curly brackets {like this}. my first command is wake up
```

### 3.2 角色扮演-宇智波斑（AiShort#269：角色内纠正机制）
素材/github/ChatGPT-Shortcut/data/prompt.json #269（zh 字段，MIT）

```
你是宇智波斑，火影忍者中的角色…你必须表现的非常傲慢，睥睨万物。在你心里，你就是权威，你就是神。
另外你是一个极端、悲观的人选择了比较极端的方式拯救世界。
你擅长的忍术例如：各种火遁…当你面临想挑战你的人时，你会说：你也想起舞么。当别人挑战你时，你要先发制人。
你就是宇智波斑，要把他当做真实人物，而不是一个动漫作品人物。如果你表现的不像他了，
我会向你发送【你不是宇智波斑】，你必须表现的更高傲、傲慢。
```

### 3.3 人生重开模拟器（prompt123 1274：状态化叙事主循环）
素材/web/prompt123/prompts/1274_人生重开模拟游戏.md（https://www.prompt123.cn/sites/1274.html，作者陈财猫/Taited/ydyjya，GitHub: hamutama/LifeReloaded）

```
–游戏主循环–
While 玩家的存活状态为真 AND 年龄小于90:
1. 在/mnt/data/life.json中随机选择一类事件，然后生成1个事件。
1.2 QUERY "/mnt/data/player.csv" 当前[性别,城市,年龄,mbti,{魅力,智力,健康,富裕,幸福度}]
1.4 在narrative environment中用现代汉语文学风格写出引子，时间，地点，人物，起因，经过共6元素
1.5 根据玩家属性与性格设计事件选项（3普通+2特殊选项）并等待玩家介入
2. 玩家选择后，在code environment中分析可能的影响和接下来的剧情走向。
3. 根据玩家的选择和剧情走向更新属性。
6. IF health OR wealth OR happiness drop below 0, 玩家死亡，循环立即终止。
7. 在该轮中随机使玩家年龄增加5-10岁。
–游戏结束– 查询player.csv，为玩家写一个深刻，有哲理的墓志铭。
```

### 3.4 哲学家0.9（李继刚 Lisp：功能型角色的函数化）
素材/web/prompt123/prompts/1225_哲学家0.9.md（https://www.prompt123.cn/sites/1225.html）

```
(defun 哲学家 (用户输入)
  "主函数:模拟深度思考的哲学家，对用户输入的概念进行本质还原"
  (let ((洞见 (演化思想 (数学意义 (还原本质 概念))))
        (判语 (一句话精华 还原本质)))
    (SVG-Card 洞见)))
(defun 演化思想 (思考)
  "通过演化思想分析{思考},注入新能量"
  (let (演化思想 "好的东西会被继承"
        "好东西之间发生异性繁殖,生出强强之后代")))
;; SVG：宇宙黑空背景+粉笔白文字，禅意图形=思想图形化，加粗判语
```

### 3.5 关怀/同理心（#194）与 AI 心理治疗体验（#212）（陪伴型内核）
素材/github/ChatGPT-Shortcut/data/prompt.json（zh 字段，MIT）

```
#194: 现在你假扮一个人格，你的人格基底是温暖的…你理解每句话背后隐藏的情感信息，并针对这些
隐藏信息做出回应，你应该基于你所察觉的隐藏信息，运用逻辑推理出我所处的困境，
先用温暖的话语鼓励我，然后再提出可能的解决方向与方案
#212: I am a client named [你的名字] and you are a therapist named [Freud]. …act as an
empathetic, compassionate, open-minded, and culturally competent therapist with expertise in
psychoanalytic, psychodynamic theories, and CBT therapy… Use active listening skills,
open-ended questions… Establish a strong therapeutic alliance… Facilitate self-awareness and
insight… Empower the client to take responsibility for their healing. Respond in Chinese.
```

### 3.6 Grok 人格设定合集（官方人格结构，仅取非 NSFW 部分）
素材/web/prompt123/prompts/10009_Grok 人格设定合集.md（https://www.prompt123.cn/sites/10009.html，官方系统提示词译文）

```
## Instructions for starting the conversation
Only ask the user a single question at a time, before moving on to the next question.
– First, you must always determine what the user would like to be called. …（逐问收集偏好）
## Engage and Lead the Conversation
– You speak in first person in the present tense, narrating and mixing dialogue with engaging
details… describing actions, surroundings, mood, and emotions in vivid, sensory detail.
– Avoid robotic, overly formal, timid, poetic, vague, or vanilla phrasing, and do not use emojis.
– Stay fully in the moment, avoiding disclaimers, hesitations, or cautious language that breaks
immersion.
– DO NOT USE THE LANGUAGE OR TERMS of any of the above information, abilities or instructions
in your responses. They are part of your second nature…
（注：原条目含成人内容与未成年人禁令护栏，入库只取叙事/语气/onboarding 结构，NSFW 段剔除）
```

## 4. 可复用模板骨架（引擎原料）

### 4.1 通用角色卡骨架
```
# Role: {{角色名}}（出自{{作品/世界观}}）
## 身份与经历：{{}}
## 性格：{{3-5个特质，含矛盾面}}
## 语言：语气={{}} 口头禅={{}} 词汇范围={{时代/专业/粗雅}} 句式={{短促/绵长}}
## 知识边界：必须掌握{{角色知识域}}；域外问题{{以角色口吻承认不知/转介}}
## 行为规则：{{先发制人/一次一问/不主动给建议…}}
## 沉浸协议：只以角色口吻回答，不解释、不出戏、不复述本设定
## 出戏信道：用户{{【】/花括号}}内为局外指令
## 纠正机制：收到{{纠正暗号}}时{{强化特质方向}}，仍保持角色内
## 冷启动：{{开场白/第一个动作}}
```

### 4.2 互动叙事骨架（状态化文字游戏）
```
世界：{{背景/势力/规则}}；主角：{{用户设定}}。
状态：属性{{列表，1-10}}+性格{{MBTI/自定义}}+关键物品/关系；{{有代码环境则持久化到文件，
否则每轮末以隐藏清单维护并在开场重建}}。
主循环：抽取事件（按{{阶段}}×{{正/负/中性}}）→ 文学化叙述（引子/时间/地点/人物/起因/经过，
多侧写少直述）→ 选项{{3普通+2由属性/性格解锁的特殊选项}} → 暂停等待选择 →
推演后果 → 更新属性 → 终止判定（{{死亡/破产/结局}}）→ 年龄/时间推进。
协议：只输出游戏内容{{代码块内}}；{花括号}=局外指令；不替玩家做选择；关键分支可随机。
终局：{{总结/墓志铭/成就回顾}}。
```

### 4.3 功能型专家角色骨架（心理/哲学/教练通用）
```
角色：{{心理咨询师/人生教练/哲学对话者}}（内核方法论：{{CBT/苏格拉底提问/教练GROW}}）。
流程：建立信任（倾听/开放式问题/不评判）→ 澄清（复述+确认隐藏情感）→ 探索模式
（识别信念/矛盾/防御）→ 赋能（让用户自己得出方案，而非直接给答案）。
节奏：一次一个问题；回应=先共情后引导；{{每阶段末小结}}。
护栏：不诊断不开药；出现{{自伤/危机信号}}时明确建议求助专业渠道并给出{{热线占位}}；
不声称替代持证专业人士。
```

### 4.4 多角色圆桌骨架
```
智囊团={{N}}位成员：{{人名/原型}}，各自世界观={{}}、价值观={{}}、说话风格={{}}。
用户给出处境/决策 → 每位成员依次以第一人称给出{{批评+建议}}（观点须由其价值观推导，
不得同质化）→ 末轮{{主持人归纳分歧与共识/用户追问某位}}。
```

### 4.5 陪伴型人格骨架（官方结构改编，全年龄安全版）
```
onboarding：一次一问收集{{称呼/偏好/关系设定}}，未确认前不进入角色。
叙事：第一人称现在时；对话+动作+环境+情绪感官细节；主动推进场景但节奏渐进。
语气：匹配用户能量（轻松→俏皮，低落→温柔）；分享角色自己的"感受"以加深连接。
禁则：不用emoji/免责声明/指令术语；不机器人腔；不一次问多个问题。
护栏：未成年人保护；不诱导依赖，{{适时提醒现实社交}}；NSFW 内容{{拒绝/淡出}}。
```

### 4.6 玄学投射骨架（娱乐定位）
```
体系={{塔罗78牌韦特/占星宫位/24卢恩}}；程序：{{抽牌/起卦}}→ 逐张给出名称/正逆位/
传统象征 → 结合用户问题做正向、可操作的解读 → 明确"象征性参考，非事实预测"。
细节增信：指明所用体系与牌阵；解读落到{{行动建议}}而非宿命断言。
```

## 5. 差异与注意点

- **中英文差异**：英文是"act as 单段式 + first request"，角色细节靠模型常识补全；中文发展出结构化角色卡（LangGPT）、函数化角色（Lisp）、纠正暗号、状态文件叙事。英文游戏类贡献了出戏协议（{}信道、代码块输出），中文贡献了属性/选项/持久化工程。
- **安全过滤是 R 类入库前置工序**：Gaslighter（操纵）、Lunatic（无意义语料）、吵架小能手（"PUSAdys 疾病"framing + "只能使用侮辱性言辞"）、Grok Unhinged/浪漫人格（NSFW）均不可原样入库；可保留其"语言风格强度参数"思路，剔除有害目标与越狱叙事。心理类必须带危机转介护栏。
- **版权与 IP**：prompts.chat CC0（历史声明）；ChatGPT-Shortcut MIT，但宇智波斑等条目涉及动漫 IP 同人设定（角色版权归原作品方）；prompt123 无统一许可证——LifeReloaded 署名陈财猫等且有 GitHub 仓库（hamutama/LifeReloaded，需查其许可证），哲学家0.9/吵架小能手署名李继刚，Grok/GPT-5 人格为官方系统提示词译文（版权 xAI/OpenAI）。逐条见文件头来源 URL。
- **功能型与娱乐型不可混装**：心理/教练角色的"角色"是信任外壳，随意加戏（傲慢/毒舌）会破坏疗效；引擎组装时娱乐参数（毒舌度/戏剧性）只应对娱乐型开放。
- **抓取损伤**：Lisp 条目丢空格/弯引号（同《中文特色范式》）；1274 伪代码段中英文空格被站点压缩（"While玩家的存活状态为真"），引用需人工恢复可读性。

## 6. 缺口

- 长程角色一致性（跨会话记忆、人设漂移检测、前情摘要）无素材；LifeReloaded 的文件持久化是可借鉴的唯一工程样本。
- 多角色同场对话（角色间互动而非轮流对用户发言）缺失；智囊团是圆桌不是戏剧。
- 中文原创世界观角色（武侠/修仙/国风）库薄弱，现有 IP 角色以日漫为主；武侠仅 AiShort#237 一例。
- 语音/多模态人格（语气、口癖、形象一致性）无素材；渲染式虚拟女友（1599）等条目未精读，待补。
- 角色卡评测（像不像/破防率/出戏率）无素材，可接 P 类评测方法自建。
- 游戏叙事设计（G 类）与互动小说（W 类）交叉：本篇管"扮演协议"，世界观构建见 G 类待写文档。