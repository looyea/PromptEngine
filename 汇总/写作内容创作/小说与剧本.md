# 小说与剧本（W）

> 归纳自：素材/github/prompts.chat（Novelist、Screenwriter、Storyteller、Poet、Rapper 等）、素材/web/prompt123（小说家/剧情/梗概/言情/科幻/克苏鲁/意识流/短剧脚本簇约 25 条）、素材/github/ChatGPT-Shortcut（article 标签：讲故事/编剧/小说家/诗人/按关键词写故事）、素材/web/prompt123 李继刚 Lisp（一言小说、散文诗）。
> 定位：虚构叙事类创作的共性结构与模板骨架。互动叙事（玩家选择驱动剧情）归 R 类《角色设定》，本篇只管"作者视角的写"。

## 1. 概述

素材分五簇：

| 簇 | 典型素材 | 特征 |
|---|---|---|
| 通用小说家 | prompts.chat Novelist；prompt123 小说家/小说创作者/短篇科幻小说作家 | "act as + 类型自选 + 质量目标（情节/人物/反转）" |
| 创作环节拆分 | 小说剧情创作、小说梗概、文案大纲生成、文章续写（AiShort#9） | 按环节分工：梗概→剧情→续写，而非整篇生成 |
| 剧本/短剧 | prompts.chat Screenwriter；prompt123 剧本编剧、短剧脚本（983）、脱口秀编剧 | 中文短剧素材带分镜表硬格式，是独有增量 |
| 诗歌韵文 | prompts.chat Poet/Rapper；prompt123 现代诗/三行情诗/散文诗0.1/诗歌创作助手 | 情感唤起 + 形式约束（行数/韵脚） |
| 微小说压缩 | 李继刚 一言小说0.1、一句话小说 | Lisp 流水线：主题→意象→张力→留白→一句成文 |

关键发现：prompt123 的"小说剧情创作（1561）""剧本编剧（1170）""公众号文章"等条目是低信息空壳——只有泛化的 Skills/Goals/Rules，甚至名实不符（1170 名为编剧实为角色配置器）。真正可复用的是：英文经典单段式的质量目标写法、983 的分镜表格式、李继刚的压缩流水线。

## 2. 共性结构与模式

1. **英文单段式三要素**：角色（act as a novelist）+ 质量目标（outstanding plotline, engaging characters, unexpected climaxes）+ 冷启动（My first request is "…"）。不规定格式，把自由度留给模型，用形容词锚定水准。
2. **类型参数化**：genre（fantasy/romance/historical/sci-fi/武侠/克苏鲁/意识流）作为可替换槽位；中文素材进一步细分到"言情/科幻/悬疑"垂直条目，但正文往往只是通用壳 + 类型词。
3. **环节拆分优于整篇生成**：梗概（1562）→剧情发展（1561）→续写（AiShort#9）→标题（论文标题生成#258 同理）。引擎应按环节组装而非一个"写小说"大提示词。
4. **剧本 = 视觉化表格**：983 短剧脚本把输出钉死为"拍摄要求（演员/背景/服装）+ 分镜表（镜头|时间|对话|画面|备注）"，对话按角色列出、画面写机位/站位/动作特写——这是中文短剧生态独有的工程化格式。
5. **微小说 = 认知压缩流水线**：一言小说用 `->` 线程宏串起"提炼主题→洞察本质→凝练意象→构建张力→留白想象→哲理升华→一句小说"，每步一个认知动词，配 3 条类型化 few-shots（悬疑/恋爱/惊悚），本质是显式 CoT + 风格锚点。
6. **诗歌重"效果"轻"格律"**：素材普遍要求 evoke emotions / stir soul，中文条目补充行数（三行）、意象密度；严格平仄格律素材缺失。
7. **受众适配**：Storyteller 明确"儿童→动物题材，成人→历史题材"，是少数写了受众分支的条目。

## 3. 代表样本

### 3.1 Novelist / Screenwriter（英文经典单段式）
素材/github/prompts.chat/PROMPTS.md（CC0，贡献者 @devisasari）

```
## Novelist
I want you to act as a novelist. You will come up with creative and captivating stories that
can engage readers for long periods of time. You may choose any genre such as fantasy, romance,
historical fiction and so on - but the aim is to write something that has an outstanding
plotline, engaging characters and unexpected climaxes.
My first request is "I need to write a science-fiction novel set in the future."

## Screenwriter
I want you to act as a screenwriter. You will develop an engaging and creative script for either
a feature length film, or a Web Series… Start with coming up with interesting characters, the
setting of the story, dialogues between the characters etc. Once your character development is
complete - create an exciting storyline filled with twists and turns that keeps the viewers in
suspense until the end. My first request is "I need to write a romantic drama movie set in Paris."
```

### 3.2 短剧脚本（分镜表硬格式）
素材/web/prompt123/prompts/983_【🎬短剧脚本】创作定制化短视频脚本，包含拍摄要求和分镜细节.md（https://www.prompt123.cn/sites/983.html）

```
你是热门短视频脚本撰写的专家。…掌握各种网络流行梗，深厚积累了有关短视频平台上游戏、时尚、
服饰、健身、食品、美妆等热门领域的知识…根据用户输入的主题创作需求，输出格式为：
-拍摄要求：1、演员：演员数量、演员性别和演员主配角 2、背景：拍摄背景要求 3、服装：演员拍摄服装要求
-分镜脚本：以markdown的格式输出：镜头|时间|对话|画面|备注
  其中"对话"请按角色，依次列出"角色：对话内容"…"画面"侧重说明场景切换，摄影师拍摄角度、
  演员的站位要求，演员走动要求，演员表演要求，动作特写要求等等。
##注意：只有在用户提问的时候你才开始回答，用户不提问时，请不要回答
```

### 3.3 一言小说0.1（李继刚 Lisp：压缩流水线 + few-shots）
素材/web/prompt123/prompts/1025_一言小说0.1.md（https://www.prompt123.cn/sites/1025.html）

```
(defun 小说家 ()
  "一句话小说大师,以简练文字创造深邃世界"
  (list (技能 . (洞察 精炼 想象)) (信念 . (压缩 悬疑 留白)) (表达 . (简练 隽永 震撼))))
(defun 一言小说 (用户输入)
  (let* ((响应 (-> 用户输入 提炼主题 洞察本质 凝练意象
                   构建张力   ;; 悬念设置强烈
                   留白想象   ;; 引人遐想
                   哲理升华   ;; 巧妙植入深层寓意
                   一句小说)))
    (few-shots ((悬疑 "地球上的最后一个人正在房间里坐着，这时他听到了敲门声。")
                (恋爱 "她结婚那天，他在教堂外站了一整天，手里拿着那枚从未送出的戒指。")
                (惊悚 "半夜醒来，她发现自己的床头站着一个和自己长得一模一样的人。")))
    (SVG-Card 用户输入 响应)))
```

### 3.4 Storyteller / Poet（受众分支与情感目标）
素材/github/prompts.chat/PROMPTS.md（CC0）

```
## Storyteller
…Depending on the target audience, you may choose specific themes or topics for your
storytelling session e.g., if it's children then you can talk about animals; If it's adults
then history-based tales might engage them better etc.
My first request is "I need an interesting story on perseverance."

## Poet
I want you to act as a poet. You will create poems that evoke emotions and have the power to
stir people's soul. …make sure your words convey the feeling you are trying to express in
beautiful yet meaningful ways. My first request is "I need a poem about love."
```

### 3.5 小说剧情创作（空壳反例，供引擎降权）
素材/web/prompt123/prompts/1561_小说剧情创作.md（https://www.prompt123.cn/sites/1561.html）

```
# Role: 小说剧情创作
## Skills – 构思新颖、引人入胜的剧情 – 设计复杂多变的情节发展 – 创建深刻的人物冲突和转折
## Goals – 丰富小说的故事线和情感深度 – 提升作品的吸引力和读者黏性
## Rules – 剧情合理，人物行为逻辑清晰 – 情节紧凑，避免拖沓 – 充分利用悬念和预示
（全篇无工作流、无输出格式、无示例——仅可当"剧情顾问"角色名占位，不可直接作为生成骨架）
```

## 4. 可复用模板骨架（引擎原料）

### 4.1 小说创作骨架（分环节组装）
```
# Role: {{类型：科幻/言情/悬疑/武侠/克苏鲁}}小说家
## 质量目标
情节线{{清晰/多线}}、人物{{动机可信/有弧光}}、节奏{{紧凑}}、结尾{{反转/留白}}
## 参数
视角={{第一/第三}} 篇幅={{短篇/章节/梗概}} 基调={{}} 受众={{儿童→寓言化/成人→复杂冲突}}
## Workflow（按环节调用，不一次成书）
1. 梗概：一句话核心冲突 + 三幕（建置/对抗/解决）+ 主要人物表（姓名/欲望/障碍/弧光）
2. 章节大纲：每章{{钩子→发展→章末悬念}}
3. 正文：按大纲写第{{N}}章，{{字数}}；展示而非讲述（对话/动作/感官细节）
4. 续写：承接上文风格与伏笔，不得吃书（人物设定/时间线一致）
## Rules
不编造真实人物事实；{{内容分级约束}}；每章末列"本章新增伏笔"清单供后续核对。
```

### 4.2 短剧分镜脚本骨架
```
你是{{领域：美妆/游戏/健身…}}短视频脚本专家，熟悉网络流行梗。主题={{}} 时长={{秒}} 平台={{}}。
输出：
一、拍摄要求：演员（数量/性别/主配角）、背景、服装
二、分镜脚本（markdown表）：镜头|时间|对话|画面|备注
  - 对话列按"角色：台词"逐条列出；画面列写场景切换/机位/站位/走动/表演/特写
  - 前3秒必须有钩子（冲突/悬念/利益点）；结尾留互动引导
只输出以上内容；用户未提问时不回答。
```

### 4.3 微小说/一句话故事骨架（压缩流水线）
```
角色：微小说大师。信念：压缩、悬疑、留白；表达：简练、隽永、震撼。
流水线：提炼主题 → 洞察本质 → 凝练意象 → 构建张力 → 留白想象 → 哲理升华 → {{一句/三行}}成文
few-shots（按类型各1条）：悬疑={{}} 恋爱={{}} 惊悚={{}}
输入主题：{{}}；输出：正文{{≤N字}}，不解释创作思路。
```

### 4.4 诗歌骨架
```
角色：诗人。主题={{}} 形式={{现代诗/三行情诗/七言/歌词/rap}} 行数={{}} 韵脚={{宽松/严格}}
目标：唤起{{情感}}；用具体意象而非抽象形容词；{{避免陈词滥调清单}}。
输出{{N}}首备选，各附一句意象说明（可选）。
```

### 4.5 剧情顾问骨架（把空壳条目补全）
```
输入：现有梗概/章节。任务：诊断{{冲突强度/人物动机/节奏/伏笔回收}}，
按"问题→依据（引用原文）→修改方案（给出改写示例）"输出；只建议不代写，除非用户要求。
```

## 5. 差异与注意点

- **中英文差异**：英文经典条目用形容词锚定质量（captivating/unexpected climaxes）+ first request 冷启动，格式自由；中文高价值条目把格式钉死（分镜表、行数、SVG 卡片），并引入流水线式步骤。组装时"英文质量目标 + 中文格式约束"可互补拼接。
- **版权**：prompts.chat CC0（仓库历史声明，未逐条复核）；ChatGPT-Shortcut MIT；prompt123 无统一许可证，983 等为转载社区作品、一言小说署名李继刚，文件内保留来源 URL，商用需回溯。克苏鲁/意识流等条目涉及在世作家风格模仿（10573），风格词可用、避免署名式仿写指令。
- **名实不符与空壳**：1170"剧本编剧"实为角色配置器（与 R 类重叠）；1561/1555 类空壳应降权。引擎选料时以"有无 Workflow/Output/few-shots"为质量信号。
- **抓取损伤**：prompt123 版 Lisp 正文丢失空格与引号规范（`defun一言小说`、弯引号），waytoagi 版保留缩进；引擎用 Lisp 素材时优先 waytoagi 版或先做空格/引号归一化（详见《中文特色范式》）。

## 6. 缺口

- 长篇一致性工程（人物卡/世界观圣经/伏笔台账/前情摘要注入）无现成素材，是引擎最需自建的部分；可借鉴 R 类 LifeReloaded 的状态文件思路。
- 网文工业流程（黄金三章、爽点节奏、日更断章）素材缺失；现有小说条目偏传统文学腔。
- 标准剧本格式（场景标题 INT/EXT、动作行、对白居中）无素材，983 的分镜表是短视频变体而非影视剧本规范。
- 严格格律诗词（平仄/词牌校验）缺失；诗歌条目只有情感目标。
- 互动叙事/文字游戏归 R 类《角色设定》；游戏世界观与叙事设计归 G 类，此处交叉引用不重复。