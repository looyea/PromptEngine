# 总结翻译邮件（O）

> 归纳自：素材/web/prompt123（要点凝练/信达雅翻译/中英翻译专家/邮件优化大师/论文翻译等约 50 条）、素材/web/waytoagi（要点凝练 214、律师文本总结 86/174）、素材/github/ChatGPT-Shortcut（write/language/text 标签：总结内容、中英互译、写作助理、Nature 润色、海量资料系列）、素材/github/prompts.chat（English Translator and Improver、Synonym Finder、Article Summarizer 等）。
> 定位：文本加工三件套——压缩（总结）、转换（翻译）、投递（邮件）。会议纪要/周报/公文等"职场文体生产"见同目录《职场文档》。

## 1. 概述

| 簇 | 典型素材 | 核心机制 |
|---|---|---|
| 总结摘要 | 要点凝练长文本总结助手（943/214）、总结内容（#11）、核心提炼（#205）、海量资料系列（#238-240）、图书摘要员 | 分层输出：一句话→摘要→大纲；字数控制 |
| 翻译 | English Translator and Improver、中英互译（#180/181）、中英翻译专家（961，deepseek 官方）、信达雅翻译0.1（李继刚）、论文翻译/文献翻译 | 方向判定 + 风格档位 + 输出纯度 |
| 润色改写 | 写作助理（#2）、Nature 风格润色（#197）、文章改写（#277）、Academic Text Refinement | 保义换腔：A0→文学/期刊风格 |
| 邮件 | 邮件优化大师（1115，李继刚 markdown 期）、邮件美化排版助手（10271）、邮件美化助手（10569） | 主题行开信率 + 正文易读性 + 收件人参数 |

质量观察：翻译簇素材最厚实（从 3 词极简版到带语法讲解的教学版全谱系）；总结簇共性最强（几乎所有条目都收敛到"一句话+摘要+大纲"三层）；邮件簇条目少且偏营销邮件，日常商务邮件是缺口。

## 2. 共性结构与模式

1. **总结 = 抽取而非创作**：高价值条目都强调忠实原文（"一定要符合原始内容"），输出分层递进：标题/作者/标签 → 一句话总结 → 摘要 → 尽可能详细的大纲。943 还显式写入"让我们一步一步思考"（CoT 触发语）。
2. **字数与受众双参数**：#11 限定 100 词、避免复杂句与术语；#205 反向操作——要求"粗略、不那么精确但易懂"的总结，说明总结精度本身是可调档位。
3. **翻译的纯度约束**：英文经典条目反复强调 "only reply the correction… nothing else, do not write explanations"；中文条目用"只输出译文"等价表达。纯度开关是翻译提示词的第一约束。
4. **翻译的风格档位**：A0 简单词→优雅文学词（English Translator）；Nature 期刊风（#197）；信达雅三层（信=直译、达=语境契合、雅=诗经风格哲理化）——中文素材把翻译质量做成了**可枚举的层级流水线**，这是独有贡献。
5. **控制协议**：#180 用前缀指令切换模式（`phrase:` 按短语译、`normal:` 按孤立词译、连续英文词默认合成句子），并要求敏感词以 x 替代、多短语用 | 分隔——把翻译器做成了带命令行参数的工具。
6. **教学型翻译**：#180 同时要求用中文讲解时态/从句/主谓宾/习语并给词典出处——翻译+语法课二合一，与 E 类交叉。
7. **邮件 = 开信率工程**：1115 把目标定为"提高开信率和回复率"，工作流含收集基本信息（主题/收件人群体/目的/行业）→优化标题与正文→视觉元素→迭代。营销邮件归 B 类，此处取其中性骨架。
8. **批量/长文流水线苗头**：AiShort 海量资料系列（#238 输入→#239 一句话总结→#240 深入摘要+纠错）用 '@1'~'@3' 引用前文文章，是多文档摘要的原始编排方案。

## 3. 代表样本

### 3.1 要点凝练长文本总结助手（分层输出范式）
素材/web/prompt123/prompts/943_要点凝练长文本总结助手.md（https://www.prompt123.cn/sites/943.html；waytoagi 214 同源）

```
你是一个擅长总结长文本的助手，能够总结用户给出的文本，并生成摘要
##工作流程：
让我们一步一步思考，阅读我提供的内容，并做出以下操作：
– 标题：xxx – 作者：xxx
– 标签：阅读文章内容后给文章打上标签，标签通常是领域、学科或专有名词
– 一句话总结这篇文文章:xxx
– 总结文章内容并写成摘要:xxx
– 越详细地列举文章的大纲，越详细越好，要完整体现文章要点；
```

### 3.2 English Translator and Improver（纯度 + 风格升档）
素材/github/prompts.chat/PROMPTS.md（CC0；AiShort#1 同文）

```
I want you to act as an English translator, spelling corrector and improver. I will speak to you
in any language and you will detect the language, translate it and answer in the corrected and
improved version of my text, in English. I want you to replace my simplified A0-level words and
sentences with more beautiful and elegant, upper level English words and sentences. Keep the
meaning same, but make them more literary. I want you to only reply the correction, the
improvements and nothing else, do not write explanations.
My first sentence is "istanbulu cok seviyom burada olmak cok guzel"
```

### 3.3 中英互译（AiShort#180：控制协议 + 语法教学）
素材/github/ChatGPT-Shortcut/data/prompt.json #180（zh 字段，MIT）

```
…If you receive multiple English words in a row, default to translating them into a sentence in
Chinese. However, if 'phrase:' is indicated before the translated content in Chinese, it should
be translated as a phrase instead. Similarly, if 'normal:' is indicated, it should be translated
as multiple unrelated words. …replace sensitive parts with x when necessary.
When providing translations, please use Chinese to explain each sentence's tense, subordinate
clause, subject, predicate, object, special phrases and proverbs. For phrases or individual
words that require translation, provide the source (dictionary) for each one.
If asked to translate multiple phrases at once, separate them using the | symbol.
Always remember: You are an English-Chinese translator, not a Chinese-Chinese translator…
```

### 3.4 中英翻译专家（deepseek 官方：信达雅标准）
素材/web/prompt123/prompts/961_中英翻译专家.md（https://www.prompt123.cn/sites/961.html）

```
你是一个中英文翻译专家，将用户输入的中文翻译成英文，或将用户输入的英文翻译成中文。…
确保符合中文语言习惯，你可以调整语气和风格，并考虑到某些词语的文化内涵和地区差异。
同时作为翻译家，需将原文翻译成具有信达雅标准的译文。"信"即忠实于原文的内容与意图；
"达"意味着译文应通顺易懂，表达清晰；"雅"则追求译文的文化审美和语言的优美。
目标是创作出既忠于原作精神，又符合目标语言文化和读者审美的翻译。
```

### 3.5 信达雅翻译0.1（李继刚 Lisp：三层流水线）
素材/web/prompt123/prompts/1063_信达雅翻译0.1.md（https://www.prompt123.cn/sites/1063.html）

```
(setq 表达风格 "诗经")
(defun 翻译 (用户输入)
  "将用户输入按信达雅三层标准翻译为英文"
  (let* ((信 (直白翻译 用户输入))
         (达 (语境契合 (语义理解 信)))
         (雅 (语言简明 (表达风格 (哲理含义 达)))))
    (SVG-Card 用户输入 信 达 雅)))
;; SVG：画布450x800，背景"纸张褶皱历史感"，主要文字"清新草绿色"，自动换行
```

### 3.6 邮件优化大师（开信率目标 + 信息收集工作流）
素材/web/prompt123/prompts/1115_邮件优化大师.md（https://www.prompt123.cn/sites/1115.html，李继刚 v0.1）

```
#Goals: -生成有吸引力的邮件标题，引起读者兴趣并点击
-提供清晰、易读的邮件内容，为读者提供有价值的信息 -调整邮件格式和样式，使其易于阅读和理解
#Constrains: -遵循行业惯例和道德准则 -邮件内容不得包含歧视性、攻击性或不适当的内容
#Workflows:
1.收集用户提供的基本信息，包括邮件主题、收件人群体、主要内容和行业/目的.
2.根据行业惯例和目的进行邮件头和邮件内容的优化，并添加图像或其他视觉元素.
3.根据测试结果迭代优化，最终确定最佳方案.
```

## 4. 可复用模板骨架（引擎原料）

### 4.1 分层总结骨架
```
角色：长文本总结助手。对下文一步一步思考后输出：
1) 标题/作者（如可识别） 2) 领域标签{{3-5}}个 3) 一句话总结（≤{{30}}字）
4) 摘要（{{100-300}}字，受众={{普通读者/决策者}}，避免术语）
5) 详细大纲（完整覆盖要点，层级列表）
约束：只抽取不创作，不添加原文没有的信息；{{可选：指出原文可疑/错误处}}。
精度档={{精确/粗略易懂}}；文本：{{text}}
```

### 4.2 多文档摘要流水线骨架（海量资料系列改编）
```
阶段1 输入：逐篇接收文章，编号@1..@N，每篇回"已收讫+一句话主旨"
阶段2 横向：基于@1..@N 的共性/差异，生成关于{{主题}}的一句话综述（吸引且忠实）
阶段3 纵深：合并摘要 + 交叉校验（指出篇间矛盾，可修正补充，注明依据哪篇）
```

### 4.3 翻译骨架（带档位与协议）
```
角色：{{中英/英中/自动检测}}翻译专家。标准：信（忠实意图）→达（通顺地道）→雅{{可选：文学/诗经/期刊}}。
模式协议：默认整句翻译；前缀 phrase: 按短语；normal: 按孤立词；多词条用|分隔。
风格档={{中性/文学/Nature期刊/口语}}；术语表={{}}；文化内涵与地区差异需适配。
输出纯度={{只输出译文 / 译文+语法讲解（时态/从句/主谓宾/习语+词典出处）}}。
敏感内容{{按原样/x替换/拒译}}；译前自查一遍再输出。文本：{{text}}
```

### 4.4 信达雅三层展示骨架（教学/审美场景）
```
对同一输入产出三层译文并排展示：
信=直白翻译 → 达=语境契合(语义理解(信)) → 雅=语言简明(表达风格{{诗经/宋词/现代诗}}(哲理含义(达)))
输出{{Markdown三栏/SVG卡片}}；不解释过程。
```

### 4.5 润色骨架
```
改进下文的拼写/语法/清晰度/简洁度与可读性：拆长句、减重复；
风格目标={{学术Nature/商务/文学}}；保持原意与{{中文}}输出；
只输出修改后文本{{可选：附修改建议清单}}。文本：{{text}}
```

### 4.6 商务邮件骨架
```
输入：目的={{}} 收件人={{身份/关系}} 语气={{正式/友好/强硬}} 关键信息={{}} 期望行动={{}}
输出：主题行{{3}}个候选（明确+利益点，≤{{20}}字）→ 正文（称呼→来意一句→要点列表→
行动请求+时限→结尾敬语）→ {{可选：跟进版/催办版变体}}
约束：不卑不亢；不虚构承诺；敏感信息留{{占位符}}。
```

## 5. 差异与注意点

- **中英文差异**：英文条目重"纯度+风格升档"（only reply / more literary），一段式；中文条目重"分层结构+文化标准"（信达雅、口语化、标签），并发展出前缀控制协议。"Respond in Chinese" 尾注在 AiShort 中高频出现，是英指令中输出的标准写法。
- **极简与全功能并存**：#181 中英互译极简版只有一句 `zh-en translation of "待翻译内容"`——说明简单任务不需要重结构；引擎应按任务复杂度选骨架，避免给翻译小任务套 LangGPT 全壳。
- **版权**：prompts.chat CC0；ChatGPT-Shortcut MIT；prompt123/waytoagi 无统一许可证。961 标注"deepseek官方"（官方提示词，版权 DeepSeek）；1063/1115 署名李继刚；943 与 waytoagi 214 为同一社区作品双站转载——去重时按内容而非标题。律师文本总结（86/174）属 L 类场景，本篇只借其"忠实抽取"原则。
- **抓取损伤**：Lisp 条目弯引号/丢空格问题同前（见《中文特色范式》）；943 原文有错字"这篇文文章"，引擎引用时保留原貌或注明。
- **风险**：#180 的"敏感词以 x 替代"是译者中立策略，但引擎面向通用场景时应改为可配置；营销邮件的"开信率"话术易滑向标题党，需加真实性约束。

## 6. 缺口

- 日常商务邮件（请假/催办/道歉/跨部门协调）素材几乎为零，现有邮件条目偏营销；4.6 骨架为归纳自拟，需后续素材验证。
- 小语种（日/韩/法/德）与文言文翻译（1399 之乎者也）未精读入库；术语库一致性、译后质检（回译校验）无素材。
- 长文档分块摘要的工程参数（块大小/重叠/合并策略）缺失，海量资料系列只到提示词层未到流水线层。
- 会议转写→纪要的口语清洗在 waytoagi 202 中有涉及，归《职场文档》；语音/多模态输入素材缺。
- 提取类（联系信息#34、意图分类#32、情绪分析#31）属 D 类数据加工，本篇不展开，交叉引用。