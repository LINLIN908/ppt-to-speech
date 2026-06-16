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

| 场景 | 你会得到 |
|---|---|
| 🎓 学位答辩 / 开题答辩 | 每个论点都有支撑句，不只是陈述结论；局限性被主动纳入脚本，而不是被追问时才临时应对；评委三类提问（澄清 / 方法挑战 / 正面质疑）全部演练过，附带如何优雅说"我不确定"的模板 |
| 📊 学术会议报告 | 先做时间手术再动笔——超时幻灯片给出合并或跳过方案；每张幻灯片末尾有交接句，这恰恰是现场最难即兴发挥、也最容易出现尴尬停顿的地方 |
| 🗣️ 圆桌论坛 / 研讨会 | 交付的不是线性脚本，而是一套可现场重排的论点卡：有明确立场的开场声明、按需调用的分论点、接住其他嘉宾发言的过渡句——适配圆桌的非线性节奏 |
| 🚀 融资路演 / Demo Day | 30 秒内出现钩子；数据幻灯片放慢讲、背景幻灯片加速过；Q&A 直击你最薄弱的那一页——单位经济、护城河，和你最不想被问到的那个问题，都有备好的回答 |

中英双语均为一等公民，完整支持"英文幻灯片、中文讲"的常见混合场景。

## 使用方法

安装 skill 后，直接跟 Claude 说：

> "这是我的开题答辩 PPT，下周答辩，限时 10 分钟，中文讲。帮我写逐字稿，再准备评委可能问的问题。"

> "18 张幻灯片，AERA 给了 12 分钟 —— 帮我裁切、写稿、备 Q&A。"

> "下周五 Demo Day，5 分钟硬截止。写完整路演稿，再帮我准备最难回答的问题。"

输出物：一份 Markdown 排练文档 + 一份把脚本写入备注栏的 PPT 副本（原文件永不修改）。

## 跟"直接问 ChatGPT"的区别

- **绝不复述幻灯片内容。** 听众自己会看；脚本只补充幻灯片无法呈现的东西——推理过程、过渡逻辑、重点强调。
- **时间估算经得起推敲。** 字数与语速（英文 ≈140 词/分钟，中文 ≈200 字/分钟）核对后才给出，时间分配有意不均——核心幻灯片获得 2–3 倍时长。
- **Q&A 让你有点不舒服，但这是好事。** 三类提问人分别出题（新手 / 专家 / 质疑者），直击演讲中最薄弱的论点，并附模范回答和不同场景下如何优雅说"我不知道"。
- **尊重你的原创内容。** 已有备注视作作者意图；纯图片幻灯片会向你提问，绝不臆造内容。

## 理论依据

场景参考文件建立在演讲与论证研究的基础上，而非经验之谈：

- Mayer, R. E. — *Multimedia Learning*（冗余原则：口头叙述不应重复屏幕文字）
- Sweller, J. — 认知负荷理论（口语句式需比书面句式更简洁；听觉处理有认知成本）
- Toulmin, S. — *The Uses of Argument*（Q&A 生成 = 攻击论据和支撑）
- Hyland, K. — 学术话语中的模糊限制语与元话语（答辩中的论点校准）
- Swales, J. — CARS 模型（口头动机陈述段的结构）
- Alley, M. — *The Craft of Scientific Presentations*（幻灯片与脚本的分工：断言 vs 证据）
- Monroe's Motivated Sequence（路演叙事弧）
- Chen, X.-P., Yao, X., & Kotha, S. (2009). Entrepreneur passion and preparedness in business plan presentations. *Academy of Management Journal*（准备充分比激情表现更重要——这就是为什么 Q&A 备战占了产品价值的一半）
- Heath & Heath — *Made to Stick*；Duarte, N. — *Resonate*（具体化、对比结构）

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
