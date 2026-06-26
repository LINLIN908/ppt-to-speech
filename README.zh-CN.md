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

每个场景的脚本逻辑各有来源，不共用同一套框架。

---

**🎓 学位答辩 / 开题答辩**

- **Toulmin 论证模型** — 六层论证结构，提前把推理链说出来，封住评委追问的入口
- **Hyland 元话语理论** — 动词匹配证据强度：*suggests* ≠ *demonstrates*，没有因果设计不能用 *leads to*
- **Swales CARS 模型** — 建立领域 → 找缺口 → 填补缺口，前 45 秒让评委定位你的研究

---

**📊 学术会议报告**

- **Mayer 冗余原则** — 口头重复屏幕文字会加重认知负担，脚本只写幻灯片上没有的内容
- **Sweller 认知负荷理论** — 听觉处理有成本，句子比论文更短，核心幻灯片放慢，过渡幻灯片加速
- **Alley 断言-证据框架** — 主张在幻灯片，推理在脚本，两者不重叠
- **Monroe 注意力步骤** — 前 30 秒用结果或悖论开场，而不是"今天我要讲……"

---

**🗣️ 圆桌论坛 / 研讨会**

- **Aristotle ethos 理论** — 公信力来自说出可被反驳的立场，而不是"我觉得这很重要"
- **Hyland Speaker positioning** — 每句话都在标记你与议题、与其他发言者的关系，提前确定措辞方向
- **Sweller 认知负荷理论** — 听众同时跟踪多位发言者，每条论点必须自成一个完整单元

---

**🚀 融资路演 / Demo Day**

- **Monroe Motivated Sequence** — 注意→需求→满足→可视化→行动，对应钩子→痛点→方案→牵引力→融资请求
- **Chen et al. (2009)** — VC 决策更受准备充分度影响而非当场激情，Q&A 备战与正稿同等重要
- **Duarte 对比结构** — 现状 vs 未来的情绪弧线驱动行动意愿，数字幻灯片前后设计情绪切换

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
