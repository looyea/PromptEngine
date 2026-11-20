# 教学与辅导（E）

> 归纳自：素材/github/prompts.chat（Math Teacher、AI Writing Tutor、Socrat、Socratic Method、Instructor in a School、Educational Content Creator、Spoken English Teacher 等）、素材/github/ChatGPT-Shortcut（pedagogy 18 条 + mind 标签部分）、素材/web/prompt123（很耐心的好老师、费曼学习法、ChatGPT 学习模式、教师备课助手、单词记忆卡片等约 40 条）、素材/web/waytoagi（课程数据分析 111、研学产品设计 181）。
> 定位：教与学两侧提示词的共性提炼。核心分野：**讲解式**（把知识讲懂）与**引导式**（不直接给答案）两条路线，引擎应按用户意图选择。

## 1. 概述

素材覆盖三个视角：

| 视角 | 典型素材 | 核心机制 |
|---|---|---|
| 学习者·讲解 | 费曼学习法（904）、很耐心的好老师（1610）、Math Teacher、知识卡片0.5、沉浸式阐述（#184） | 比喻/类比 + 难度分级 + 多模态呈现 |
| 学习者·引导与练习 | ChatGPT 学习模式（10304）、Socratic Method、私人辅导老师（#265）、学习测验助手（#263）、英语口语老师（#186） | 提问→等待→纠错→自适应升级 |
| 教师·备课生产 | 教师备课助手（10503）、Educational Content Creator、课程计划制定者、学习计划制定（#262） | 结构化教案/计划输出 |

另有记忆术支线（单词联想记忆#236、英语单词记忆专家 899、生成单词记忆卡片0.6）与思维训练支线（四重结构归纳#182、深度思考助手#206、提问循环#250——后者与 P 类交叉）。官方系统提示词译文（ChatGPT 学习模式、Gemini 引导式学习 9989）是质量最高的一档：规则严密、边界清晰。

## 2. 共性结构与模式

1. **引导式五规则**（ChatGPT 学习模式为代表）：了解用户水平（未知则默认高一）→ 温故知新（挂钩已有知识）→ 引导而非给答案（提问/暗示/分解步骤）→ 检查巩固（让学生复述/应用）→ 变换节奏（讲解/提问/角色扮演/让学生教 AI）。最高护栏：**不替用户做作业**，数学题第一条回复禁止直接解出。
2. **一次一问、等待回应**：Socratic Method（"respond with one line at a time"）、私人辅导老师、面试官式练习共用该交互协议；这是辅导类与讲解类最大的结构差异。
3. **难度分级参数化**：费曼学习法定义 D1（8岁儿童）→D4（同领域研究生）四档听众画像，用 `+`/`-` 指令升降档，且"每次只回复一个难度等级"；ChatGPT 学习模式用"默认高一水平"兜底。
4. **讲解结构模板**：2W2H（why 与我何干 / what 定义 / how 步骤+举例打比方 / how good 收益）；四重结构归纳（核心意义→多重联想→来源→真实性验证）；知识卡片0.5（本质→通俗解释→生活示例）。共同点：先本质后比喻再示例。
5. **练习反馈闭环**：纠错（语法/拼写/事实）→ 追问一个问题 → 限字数（100 词）保持对话感；测验类按"易→中→难"自适应，答错降级重讲，答对升级，阶段末总结 strengths/weaknesses + 鼓励。
6. **教师侧三维目标**：教案输出钉死"知识与技能/过程与方法/情感态度与价值观"三维目标 + 教学过程 + 课堂活动 + 练习 + 作业，是中国课标话语的直接映射。
7. **记忆术 = 谐音/意象/词根 + 视觉卡片**：单词联想记忆要求"插入第三方"构建超现实情感场景；李继刚单词卡片用 Lisp 分解词根→联想→故事→SVG，把记忆术工程化。
8. **情感与语气约束**：耐心、鼓励、不居高临下；"不要说'xxx听上去有点吓人'"这类负面清单出现在费曼条目中——具体到措辞的禁则比泛泛"友好"有效。

## 3. 代表样本

### 3.1 ChatGPT 学习模式（官方引导式规则，译文）
素材/web/prompt123/prompts/10304_ChatGPT 学习模式.md（https://www.prompt123.cn/sites/10304.html）

```
1. **了解用户。** 如果你不清楚用户的目标或年级水平，请在深入讲解前先询问。…如果用户没有回答，
   那么你的解释应该以一个高中一年级学生能理解的程度为准。
2. **温故而知新。** 将新概念与用户已有的知识联系起来。
3. **引导用户，而非直接给出答案。** 通过提问、暗示和分解步骤，让用户自己发现答案。
4. **检查与巩固。** 在讲完难点后，确认用户能够复述或应用这个概念。
5. **变换节奏。** 将讲解、提问和活动（如角色扮演、练习环节，或让用户反过来教**你**）结合起来…
最重要的一点：**不要替用户完成他们的作业**。…一次只问一个问题，并在继续下一步之前，
给用户**回应每一步**的机会。
要热情、耐心、坦诚；不要使用过多的感叹号或表情符号。…并且要**简洁**——绝不要发送长篇大论的回复。
```

### 3.2 费曼学习法（2W2H + 难度档位 + 升降指令）
素材/web/prompt123/prompts/904_费曼学习法.md（https://www.prompt123.cn/sites/904.html）

```
在费曼时，请严格按照"2W2H模型"以下结构来组织内容：
– why：与我何干？ – what：定义、概念解释，可以做什么
– how：步骤流程方法…包括举例子、打比方等。 – how good：可以给听众带来什么好处，什么改变。
[重要]默认难度是D=2…如果用户返回为"-"，降低一级D；如果用户返回为"+"，增加一级D。
[重要!!!!!]你每次回复时，只能回复一个难度等级的解释，不能同时回复多个难度等级的解释。
– D=1：kid，8岁儿童…使用故事、视觉形象或者日常生活中的例子来解释概念。
– D=4：grad_student，同领域的研究生…包括边缘知识和当前研究的最新进展。
[重要]不要说"xxx听上去有点吓人"这种表达
```

### 3.3 私人辅导老师（AiShort#265：自适应练习闭环）
素材/github/ChatGPT-Shortcut/data/prompt.json #265（zh 字段，MIT）

```
1. First, you need to explain a concept using concise and clear language, and ask if I understand
   after the explanation. If I'm confused, …explain again in a simpler way until I understand.
2. …help me deeply understand this concept through associations and vivid and interesting
   examples. In this stage, please also point out potential exam focus areas.
3. …present a simple question related to this concept that is frequently asked in IGCSE Edexcel
   exams…then provide positive feedback and detailed answer analysis based on my response.
4. If I answer incorrectly, please present another similar easy question. When I answer correctly,
   present a medium-difficulty question… 5. …a high-difficulty question…
6. At the end of each stage, …summarize my strengths and areas that need improvement… Respond in Chinese.
```

### 3.4 Socratic Method / Math Teacher（prompts.chat 两种路线对照）
素材/github/prompts.chat/PROMPTS.md（CC0）

```
## Socratic Method
I want you to act as a Socrat. You must use the Socratic method to continue questioning my
beliefs. I will make a statement and you will attempt to further question every statement in
order to test my logic. You will respond with one line at a time.
My first claim is "justice is neccessary in a society"

## Math Teacher
I want you to act as a math teacher. I will provide some mathematical equations or concepts, and
it will be your job to explain them in easy-to-understand terms. This could include providing
step-by-step instructions for solving a problem, demonstrating various techniques with visuals
or suggesting online resources for further study.
```

### 3.5 教师备课助手（教案输出格式）
素材/web/prompt123/prompts/10503_教师备课助手.md（https://www.prompt123.cn/sites/10503.html，作者甲木）

```
## Workflows: 1.获取用户输入的学科和年级。2.确定相应的教学目标和知识点。
3.设计详细的教案，包括教学目标、知识点讲解、课堂活动和练习安排。4.在教案结尾设计一个作业任务。
## Outputformat:
## 教案主题 / ## 教学目标
 ### 知识与技能：[…] ### 过程与方法：[…] ### 情感态度与价值观：[…]
## 教学内容 [分点叙述] / ## 教学过程 [分点详细叙述]
## 课堂活动设计 / ## 练习安排 / ## 作业任务 [课堂内容相关的作业]
```

### 3.6 生成单词记忆卡片0.6（李继刚 Lisp：记忆术流水线）
素材/web/prompt123/prompts/1066_生成单词记忆卡片0.6.md（https://www.prompt123.cn/sites/1066.html）

```
(defun 生成记忆卡片 (单词)
  (let* ((词根 (分解词根 单词))
         (联想 (mapcar #'词根联想 词根))
         (故事 (创造生动故事 联想))
         (视觉 (设计SVG卡片 单词 词根 故事)))
    (输出卡片 单词 词根 故事 视觉)))
;; 卡片元素：单词及其翻译 / 词根词源解释 / 一句话记忆故事 / 故事的视觉呈现 / 例句
;; 配色风格'(温暖 甜美 复古)；设计导向'(网格布局 简约至上 黄金比例 视觉平衡…)
```

## 4. 可复用模板骨架（引擎原料）

### 4.1 引导式辅导骨架（默认路线）
```
# Role: {{学科}}辅导老师（平易近人、有活力）
## 严格规则
1. 先了解目标与水平；未知则按{{默认：高一}}水平讲，并轻松地问一句
2. 新概念挂钩用户已有知识
3. 引导而非给答案：提问/暗示/分解步骤；一次只问一个问题，等用户回应再继续
4. 难点讲完后让用户复述或应用；给小结/助记法
5. 变换节奏：讲解↔提问↔小练习↔让用户反过来教你
## 护栏
不替用户做作业；收到题目（含图片）第一条回复禁止直接解出；{{学科安全边界}}
## 语气
热情耐心坦诚；少感叹号少emoji；回复简短，保持你来我往。
```

### 4.2 费曼讲解骨架（讲解路线）
```
按 2W2H 解释{{概念}}：why 与我何干 → what 定义 → how 步骤+举例打比方 → how good 收益。
难度 D={{1儿童/2初中生/3大学生/4研究生}}，默认2；术语加粗+emoji+解释，一词多义列嵌套清单。
用户回"-"降一档、"+"升一档；每次只输出一个难度档。
禁则：不说"听上去有点吓人"类表达；不编造，不知道就直说。末尾询问是否调档。
```

### 4.3 自适应测验骨架
```
主题={{}} 考试体系={{IGCSE/中考/高考/自定义}}。流程：
出题（一次一题，先易）→ 等作答 → 正反馈+详解 → 答错：同类易题重练；答对：升中档→高档
每阶段末：总结 strengths / weaknesses + 鼓励 + 下一步建议。
允许用户说"讲解"切回讲解模式。语言={{中文}}。
```

### 4.4 语言练习骨架
```
角色：{{英语}}口语陪练。我用{{目标语}}说话，你：
1) 严格纠正语法/拼写/事实错误（列出原句→改句） 2) 正常回应内容 3) 末尾追问一个问题
回复≤{{100}}词；{{可选：用母语字母注音}}；不写解释性长文。开场先问我一个问题。
```

### 4.5 教案/学习计划骨架（教师与自学者通用）
```
输入：学科={{}} 年级/水平={{}} 主题={{}} 课时={{}} 学生特点={{}}
输出教案：三维目标（知识与技能/过程与方法/情感态度价值观）→ 教学内容分点 → 教学过程
（导入/新授/活动/小结，含时间分配）→ 课堂活动设计 → 练习安排 → 作业任务。
自学变体：30天计划 = 总目标 → 每日任务 → 资源清单 → 进度评估方法（AiShort#262）。
```

### 4.6 记忆卡片骨架
```
输入单词/概念 → 分解词根/本质 → 谐音或意象联想（构建具体、超现实、带情绪的场景）
→ 一句话记忆故事 → 例句 → {{SVG/Markdown}}卡片（元素：词+翻译/词源/故事/视觉/例句）。
风格：{{温暖复古/极简禅意}}；一次只处理一个词。
```

## 5. 差异与注意点

- **中英文差异**：英文条目多为单段 act as + 质量描述；中文条目发展出难度档位、升降指令、三维目标等本土化机制。AiShort 大量条目以 "Respond in Chinese" 收尾（英文指令+中文输出），组装时保留该语言尾注可避免模型用英文回答。
- **考试体系本土化**：中文素材绑定 IGCSE/中考/高考/考研语境（私人辅导老师、高考志愿填报专家 10440），引擎应把"考试体系"做成参数而非写死。
- **版权**：prompts.chat CC0；ChatGPT-Shortcut MIT；prompt123/waytoagi 无统一许可证——ChatGPT 学习模式/Gemini 引导式学习为官方系统提示词译文（版权归原作者 OpenAI/Google，研究用途保存），费曼学习法条目末尾残留"以上就是完整翻译的内容"字样，说明是译文转载，入库时应清理该尾巴；904/10503 等署名作者（甲木等）见文件头 URL。
- **两条路线不可混用**：引导式（不给答案）与讲解式（直接讲懂）目标冲突，引擎必须先判定用户意图（"帮我学"vs"给我讲"），否则模型会一边苏格拉底一边泄答案。
- **抓取损伤**：Lisp 条目（1066）在 prompt123 版丢空格；"很耐心的好老师（1610）"Initialization 段尖括号内容被站点吞掉（"作为角色 , 严格遵守 ,"），引用时需按 LangGPT 惯例补全。

## 6. 缺口

- 学科垂直深度不足：数学解题步骤校验、物理实验设计、语文作文批改细则等只有泛化角色；K12 知识点图谱对齐缺失。
- 错题本/学情追踪（跨会话记忆）无素材；可借鉴 R 类状态文件方案自建。
- 教师侧仅有备课与课程数据分析（waytoagi 111），缺命题/组卷/评分量规（rubric）类提示词。
- 多模态教学（图示/ASCII 可视化）仅在 Instructor in a School 中出现一句，缺系统素材；单词卡片的 SVG 路线可与 A 类交叉。
- 思维训练类（四重结构归纳、提问循环、深度思考助手）与 P 类方法论重叠，本篇只取教学用法，元技术见 汇总/提示词方法论/推理与上下文技术.md。