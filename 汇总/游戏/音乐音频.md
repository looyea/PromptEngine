# 游戏 · 音乐音频

> 一级类 G（游戏）· 二级维度「音乐/音频」（与 M 类 AI 音乐生成共享部分技法）
> 素材路径相对 `素材/`；用户样本 `../游戏Spec01.md` 位于项目根。

## 1. 概述

「游戏音乐音频」解决的是：定义游戏的**声音身份**（音色 palette、音乐风格、情绪映射），设计**自适应音频系统**（随游戏状态分层/强度缩放/ducking），并把每个音效写成可制作的规格表（频率特征、时长、空间属性、变体、 loudness 目标）。它区别于"生成一首歌"：游戏音频要可触发、可分层、可循环、可混音、服务于玩法可读性。

素材里的三种做法：

- **音频总监 + 音效设计师 agent**（CCGS `audio-director`、`sound-designer`、`team-audio` 编排 skill）：总监管声音 palette、音乐方向、音频事件架构、混音策略、自适应设计、资产规格（格式/采样率/命名/LUFS/大小预算）；音效设计师管每个 SFX 的规格表、事件清单、混音文档、变体规划、环境音分层。
- **整游戏 spec 的音频硬规格**（`../游戏Spec01.md` 第七节）：直接规定 BGM ≥8 首（按场景分类）、音效 ≥25 种（按界面/移动/战斗/其他分类）、风格方向、无缝循环、音量标准化、程序作曲 fallback（MIDI + 免费音色库渲染）。
- **程序化音频实现**（playcanvas-prompts）：在无音频资产时用 WebAudio 程序合成全部 SFX（`src/audio.ts`），把音频事件与玩法事件接线（onShot→sfx.fire、onKill→kill sfx 等）。

> 注：本批 G 类素材侧重"音频方向/规格/事件架构"，**AI 音乐生成（Suno/Udio）的具体 prompt 技法在 M 类**（awesome-suno-prompts、suno-reference），本篇末尾给出衔接。

## 2. 共性结构与模式

归纳出游戏音频提示词的通用骨架：

1. **先定声音 palette 与情绪映射**：acoustic vs synthetic、clean vs distorted、sparse vs dense；为每个游戏情境/区域定义参考曲目与声音 profile；音乐风格、配器、动态音乐行为、每个游戏状态的情绪映射。
2. **音频事件架构**：什么触发声音、声音如何分层、优先级系统、ducking 规则；每个系统维护完整事件清单（触发、优先级、并发上限、冷却）。
3. **混音策略与玩法可读性**：音量层级、空间音频规则、频率平衡目标；**玩家必须总能听到玩法关键音频**（gameplay-critical audio 优先）。
4. **自适应音频**：音频如何响应游戏状态——强度缩放、区域过渡、战斗 vs 探索、健康状态。
5. **资产规格 + 命名 + loudness**：格式、采样率、命名 `[category]_[context]_[name]_[variant].[ext]`、loudness 目标（LUFS）、文件大小预算。
6. **SFX 规格表（每个音效）**：描述、参考声音、频率特征、时长、音量范围、空间属性（衰减曲线类型、min/max 距离、音量与高频 rolloff）、所需变体数。
7. **变体与重复避免**：变体数量、pitch 随机化范围、round-robin 行为；环境音分层（base layer、detail sounds、one-shots、过渡）。
8. **程序化 fallback + 标准化**：无资产时用代码合成（sfxr/jsfxr 合成音效、MIDI + FluidSynth 渲染音乐）；所有音频音量标准化、不爆音不忽大忽小、BGM 无缝循环。
9. **事件接线（实现层）**：把玩法事件映射到音频事件（onShot/onEmpty/onReload/onLand/onSlide/onDamaged/onKill/onPlayerHit→对应 sfx），形成可验证的触发链。

## 3. 代表样本

**样本 A — 音频总监的声音 palette 与自适应职责**
`github/Claude-Code-Game-Studios/.claude/agents/audio-director.md`
```
1. Sound Palette Definition: acoustic vs synthetic, clean vs distorted, sparse vs
   dense. Document reference tracks and sound profiles for each game context.
3. Audio Event Architecture: what triggers sounds, how sounds layer, priority
   systems, and ducking rules.
4. Mix Strategy: volume hierarchies, spatial audio rules, frequency balance goals.
   The player must always hear gameplay-critical audio.
5. Adaptive Audio Design: intensity scaling, area transitions, combat vs exploration,
   health states.
6. Audio Asset Specifications: format, sample rate, naming, loudness targets (LUFS),
   file size budgets.
```

**样本 B — 音频命名约定**
`github/Claude-Code-Game-Studios/.claude/agents/audio-director.md`
```
[category]_[context]_[name]_[variant].[ext]
Examples:
- sfx_combat_sword_swing_01.ogg
- sfx_ui_button_click_01.ogg
- mus_explore_forest_calm_loop.ogg
- amb_env_cave_drip_loop.ogg
```

**样本 C — 音效设计师的 SFX 规格表**
`github/Claude-Code-Game-Studios/.claude/agents/sound-designer.md`
```
1. SFX Specification Sheets: for each sound effect, document description, reference
   sounds, frequency character, duration, volume range, spatial properties
   (attenuation curve type, min/max distance, volume and high-frequency rolloff),
   and variations needed.
2. Audio Event Lists: what triggers each sound, priority, concurrency limits, cooldowns.
4. Variation Planning: number of variants, pitch randomization ranges, round-robin.
5. Ambience Design: base layer, detail sounds, one-shots, and transitions.
```

**样本 D — 程序化音频与事件接线**
`github/playcanvas-prompts/blaster-range/prompt.md`
```
src/audio.ts      procedural WebAudio sfx (no audio assets exist)
Wiring: weapon onShot→sfx.fire, onEmpty→sfx.empty, onReload→reloadStart/End;
player onLand→sfx.land, onSlide→sfx.slide; bots onDamaged→hitmarker + hit/headshot
sfx, onKill→score pop + kill sfx, onBotShot→sfx.botFire, onPlayerHit→sfx.hurt.
```

**样本 E — 用户样本的音频硬规格与 fallback**
`../游戏Spec01.md`
```
- BGM 至少 8 首：标题、大地图、日常剧情、紧张/悲伤剧情、普通战斗、首领战、胜利、失败。
- 风格：交响乐加中世纪民谣感，参考氛围但不模仿具体旋律。可程序作曲（MIDI + 免费音色库）。
  曲目要能无缝循环。
- 音效至少 25 种：界面（光标/确认/取消/菜单）、移动（脚步按地形≥3 种）、
  战斗（剑击/弓箭/各元素魔法/治疗/暴击/闪避/倒地）、其他（升级/拾取/说服/胜负）。
- 所有音频做音量标准化，不能爆音，也不能忽大忽小。
```

**样本 F — 音频团队编排（方向→实现管线）**
`github/Claude-Code-Game-Studios/.claude/skills/team-audio/SKILL.md`
```
Orchestrate the audio team — audio-director, sound-designer, technical-artist,
gameplay-programmer — direction through implementation.
Decision Points: at each step transition, present the subagent's proposals as
selectable options; write full analysis in conversation, capture decision with labels.
collaborative: user must approve before next step. guided: advances unless BLOCKED.
autonomous: runs end to end, recording each step via log_decision.
```

## 4. 可复用模板骨架

```markdown
# 音频设计：<游戏/区域/系统名>
## 1. 声音身份与情绪映射
- 声音 palette：acoustic/synthetic、clean/distorted、sparse/dense。
- 音乐风格与配器；每个游戏状态/区域的情绪映射 + 参考曲目（取氛围，不模仿具体旋律）。
- 动态音乐行为：如何随状态分层/过渡。

## 2. 音频事件架构
- 触发源 → 事件；分层规则；优先级系统；ducking 规则。
- 每系统事件清单：触发 / 优先级 / 并发上限 / 冷却。
- 玩法关键音频优先：玩家必须总能听到（gameplay-critical）。

## 3. 混音与自适应
- 音量层级 / 空间音频规则 / 频率平衡目标 / bus 分配 / 频率遮蔽考量。
- 自适应：强度缩放、区域过渡、战斗 vs 探索、健康状态。

## 4. 资产规格与命名
- 格式 / 采样率 / loudness 目标（LUFS）/ 文件大小预算。
- 命名：`[category]_[context]_[name]_[variant].[ext]`（sfx_/mus_/amb_）。
- BGM 无缝循环；全部音量标准化，不爆音不忽大忽小。

## 5. SFX 规格表（每个音效）
- 描述 / 参考声音 / 频率特征 / 时长 / 音量范围 /
  空间属性（衰减曲线类型、min/max 距离、音量与高频 rolloff）/ 变体数。
- 变体规划：变体数量、pitch 随机化范围、round-robin。
- 环境音分层：base layer / detail sounds / one-shots / 过渡。

## 6. 制作路径（二选一或混合）
- 资产路径：免费素材（核对许可证，CC0 优先）或定制制作。
- 程序化 fallback：sfxr/jsfxr 合成音效；MIDI + 免费音色库（FluidSynth）渲染音乐。

## 7. 事件接线与验证（实现层）
- 玩法事件 → 音频事件映射表（onShot/onKill/onPlayerHit…→对应 sfx）。
- 验证：实际运行听辨触发链、检查 loudness/循环/无爆音；无音频资产时确认程序合成可用。
```

## 5. 差异与注意点

- **方向层 vs 实现层**：audio-director/sound-designer 产出"规格与事件架构"（文字），playcanvas 的 `audio.ts` 才是"程序合成实现"。引擎编排先方向后实现；audio-director 明确"不做音频决策之外的视觉判断"，与 art-director 在"视觉情绪匹配"处协作但各管一摊。
- **资产 vs 程序化**：Spec01 与 playcanvas 都给了"无资产/无法联网时程序合成"的 fallback，这是无人值守游戏开发的关键韧性设计；M 类 AI 音乐生成则是另一条"生成成品曲目"路径，三者可互补。
- **玩法可读性是硬约束**：audio-director"玩家必须总能听到玩法关键音频"、混音 ducking 服务于它——游戏音频不同于纯音乐欣赏，关键反馈音不能被 BGM 淹没。
- **许可证**：CCGS **MIT**；playcanvas-prompts **未声明**（其音频为程序合成、无第三方资产）；`../游戏Spec01.md` 要求免费曲目核对许可证、Freesound 筛 CC0、禁用商业游戏旋律、逐条记 CREDITS.md。
- **与 M 类衔接**：本篇是"游戏音频工程"（事件/分层/混音/规格）；若要用 Suno/Udio 生成 BGM 成品，风格描述/元标签/结构标签技法见 `汇总/音乐/`（M 类，素材 awesome-suno-prompts、suno-reference）。引擎可把 M 类生成的曲目接回本篇的"事件架构 + loudness + 循环"规格。

## 6. 缺口

- 缺少**具体音频中间件**（Wwise/FMOD）的集成提示词（RTPC、state、switch、blend container 映射）——CCGS 偏引擎无关的事件架构，未落到中间件参数。
- 缺少**自适应音乐分层谱**（combat/exploration 强度层如何 crossfade、stinger 如何插入）的成品范本。
- 缺少**配音（VO）管线**提示词（录音规格、对白与口型同步、本地化多语言 VO 管理）——writer 提"可配音"但无 VO 制作规格。
- 缺少**音频性能预算**（并发声源数、CPU/内存上限、流式 vs 加载）的规约模板。
- 本批 G 类未含 Suno/Udio 成品 prompt，BGM"生成"环节需跨类引用 M 类素材。