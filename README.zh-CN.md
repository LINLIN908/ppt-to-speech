<div align="center">

# ppt-to-speech

**把你的 PPT 变成你真正能开口说的东西。**

「幻灯片做完了，不知道嘴上怎么说。」

![](https://img.shields.io/badge/Claude%20Code-Skill-4CAF50?style=flat-square&labelColor=222222)
![](https://img.shields.io/badge/运行环境-Claude%20Code-7C3AED?style=flat-square&labelColor=444444)
![](https://img.shields.io/badge/语言-中文%20%2F%20英文-2563EB?style=flat-square&labelColor=444444)
![](https://img.shields.io/badge/许可证-MIT-111111?style=flat-square&labelColor=444444)

ppt-to-speech 帮你把做好的幻灯片转化为可直接朗读的逐字稿，标注每张幻灯片的时长，将脚本写回 .pptx 备注栏，并生成一份 Q&A 备战手册——涵盖新手、专家、质疑者三类听众可能提出的问题。

[使用场景](#使用场景) · [跟直接问 ChatGPT 的区别](#跟直接问-chatgpt-的区别) · [安装](#使用方法) · [理论依据](#理论依据) · [目录结构](#目录结构)

---

**Other Languages:**

[English](README.md)

</div>

---

## 支持场景

<table width="100%" cellspacing="0" cellpadding="0" border="0">
<tr>
<td width="25%" align="center" valign="top">
<img src="assets/panel-1.png" width="100%"><br><br>
<strong>说服式辩护</strong><br><br>
听起来你在立论，而不是在念稿
</td>
<td width="25%" align="center" valign="top">
<img src="assets/panel-2.png" width="100%"><br><br>
<strong>每句话感知时间</strong><br><br>
核心放慢、过渡加速、不留空档
</td>
<td width="25%" align="center" valign="top">
<img src="assets/panel-3.png" width="100%"><br><br>
<strong>对话式，非演讲式</strong><br><br>
别人说完，你知道接什么
</td>
<td width="25%" align="center" valign="top">
<img src="assets/panel-4.png" width="100%"><br><br>
<strong>叙事弧线驱动</strong><br><br>
结论先于数字，情绪带着听众走
</td>
</tr>
</table>

<div align="center"><sub>同一份 PPT，场景不同，说话方式就不同——脚本风格随场景而变，而不是套用同一个模板。</sub></div>

## 使用方法

安装 skill 后，把 PPT 发给 Claude，然后说：

🎓 **答辩**
> "这是我的开题答辩 PPT，下周五答辩，限时 10 分钟，中文讲。帮我写逐字稿，再准备评委可能问的问题。"

📊 **会议报告**
> "这是我的 AERA 投稿 PPT，18 张，给了 12 分钟。帮我先规划哪几张可以压缩，再写完整报告稿。"

🗣️ **圆桌 / 研讨会**
> "下周有个教育技术的圆桌，这是我准备的幻灯片。不需要逐字稿，帮我整理一套可以随时调用的论点，加上开场说什么。"

🚀 **路演**
> "这是我们的融资 PPT，Demo Day 上台 5 分钟硬截止。帮我写完整路演稿，再准备投资人最可能刁难的问题。"

输出物：一份 Markdown 排练文档 + 一份把脚本写入备注栏的 PPT 副本（原文件永不修改）。

## 跟"直接问 ChatGPT"的区别

- **绝不复述幻灯片内容。** 听众自己会看；脚本只补充幻灯片无法呈现的东西——推理过程、过渡逻辑、重点强调。
- **时间估算经得起推敲。** 字数与语速（英文 ≈140 词/分钟，中文 ≈200 字/分钟）核对后才给出，时间分配有意不均——核心幻灯片获得 2–3 倍时长。
- **Q&A 让你有点不舒服，但这是好事。** 三类提问人分别出题（新手 / 专家 / 质疑者），直击演讲中最薄弱的论点，并附模范回答和不同场景下如何优雅说"我不知道"。
- **尊重你的原创内容。** 已有备注视作作者意图；纯图片幻灯片会向你提问，绝不臆造内容。

## 理论依据

每个场景的脚本逻辑各有来源，不共用同一套框架：

**🎓 学位答辩**
- Toulmin, S. — *The Uses of Argument*：论点需有支撑和支持，Q&A 本质是对论据的攻击
- Hyland, K. — 学术话语中的模糊限制语与元话语：论点校准，知道什么时候该说"我们认为"而不是"我们证明了"
- Swales, J. — CARS 模型：口头动机陈述段的结构，如何开场才能让评委快速理解你的研究位置

**📊 学术会议报告**
- Mayer, R. E. — *Multimedia Learning*：冗余原则——口头叙述不应重复屏幕上已有的文字
- Sweller, J. — 认知负荷理论：听觉处理有成本，口语句式必须比书面句式更简洁
- Alley, M. — *The Craft of Scientific Presentations*：幻灯片与脚本的分工——断言在幻灯片，证据在脚本

**🗣️ 圆桌论坛 / 研讨会**
- Goffman, E. — 互动仪式与话轮转换：圆桌的核心不是演说，而是接话时机与立场维护
- Heath & Heath — *Made to Stick*：观点要具体、可记忆，才能在讨论中被引用和传播

**🚀 融资路演**
- Monroe's Motivated Sequence：注意→需求→满足→可视化→行动，路演叙事弧的经典结构
- Chen, X.-P., Yao, X., & Kotha, S. (2009). *Academy of Management Journal*：准备充分比激情表现更重要——Q&A 备战是路演成败的关键变量
- Duarte, N. — *Resonate*：对比结构（现状 vs 未来）驱动听众情绪，而非数据本身

## 目录结构

```
ppt-to-speech/
├── SKILL.md                       # 工作流 + 场景路由
├── references/
│   ├── academic/
│   │   ├── thesis-defense.md
│   │   ├── conference-talk.md
│   │   └── panel-discussion.md
│   ├── founding-pitch.md
│   └── qa-preparation.md          # 三类提问人方法论
└── scripts/
    ├── extract_slides.py          # pptx → JSON（文字、表格、图片 alt-text、现有备注）
    └── write_notes.py             # 脚本 JSON → 备注栏（非破坏性写入）
```

两个辅助脚本需要安装 `python-pptx`。

## 许可证

MIT
