# 引擎测试用例 01：战棋 RPG 战斗 BGM（跨域 G+M）

> 输入场景："我在做一款皇家骑士团式战棋 RPG（HD-2D），需要一段战斗 BGM，用 Suno 生成，纯器乐，要紧张但不吵。"
> 执行：ENGINE.md 七步流水线 · 2026-11-19

## 【场景解析】

| 槽位 | 值 |
|---|---|
| domain | 主 G 游戏（音乐音频）+ 副 M 音乐（AI音乐生成） |
| task | 游戏配乐 → Suno 风格串 + 元标签结构 |
| platform | Suno v4.5+（风格串上限 1000 字符） |
| deliverable | 纯器乐 battle BGM |
| autonomy | 协作式（用户试听后迭代） |
| constraints | 紧张但不吵 [假设：中低能量密度、无嘶吼人声、循环友好]；HD-2D 奇幻战棋语境 [假设：管弦+电子混合] |

路由：汇总/游戏/音乐音频.md（声音身份）+ 汇总/音乐/AI音乐生成.md §4（风格串骨架+元标签）+ 流派与场景词库.md（场景包）。

## 【提示词正文】

**Style of Music 字段：**

```
Cinematic tactical battle score, orchestral hybrid with dark fantasy edge,
driving staccato strings, french horn calls, taiko-like war drums,
subtle analog synth pulses underneath, tense but controlled dynamics,
no lead vocals, instrumental, minor key tension with heroic resolution,
tight modern mix, sidechained pads for clarity,
BPM: 132, Key: D Minor
```

**Lyrics 字段（结构控制，纯器乐）：**

```
[track]
[instrumental]
[sequence: intro A | battle A | bridge | battle B | outro]
[length: 180]

[intro: Sparse war drum heartbeat, low string drone, distant horn call building anticipation.]

[battle A | style: driving staccato strings, intensity: medium-high, dynamics: crescendo]
[ drums: taiko pattern locked to 132 bpm, horn counter-melody enters bar 9 ]

[bridge | intensity: low, instruments: solo cello over sustained pad, dynamics: decrescendo]

[battle B | style: full orchestral hybrid, intensity: high, harmony: modulation up half step]
[ final 8 bars: heroic resolution, brass fanfare over fading drums ]

[outro: Single drum hit, string harmonic fade.]
[end]
```

## 【组装说明】

骨架=音乐/AI音乐生成.md §2.1 风格串七要素 + §2.3 整歌组装模式；技术=平台词表注入（管道记法做段落局部覆盖）；游戏侧约束来自 游戏/音乐音频.md"混音可读性"（sidechain 保对白频段）与循环友好（outro 收束干净）。质检门：无矛盾风格词 ✓ 纯器乐双保险（no lead vocals + [instrumental]）✓ 指令写在标签内防被唱出 ✓ 长度 180s 防主歌循环 ✓。

## 【可选变体】

若试听偏吵：降 intensity 一档、删 brass fanfare、BPM 降至 120；若要"Boss 战"版：加 choir [choir] 与 [modulation] 多次转调。

---
**验证结论**：跨域路由、平台语法（管道记法/冒号内嵌）、质检门均按 ENGINE.md 执行成功。发现改进点：游戏音乐音频汇总的"事件架构"（战斗/胜利/失败状态切换）应在 loop 模式下展开为多条提示词，已记入 ENGINE.md loop 模式适用场景。