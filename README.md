<div align="center">

# ppt-to-speech

**Turn your finished slides into words you can actually say.**

*"The deck is done. Now what do I say?"*

![](https://img.shields.io/badge/Claude%20Code-Skill-4CAF50?style=flat-square&labelColor=222222)
![](https://img.shields.io/badge/Runtime-Claude%20Code-7C3AED?style=flat-square&labelColor=444444)
![](https://img.shields.io/badge/Language-EN%20%2F%20ZH-2563EB?style=flat-square&labelColor=444444)
![](https://img.shields.io/badge/License-MIT-111111?style=flat-square&labelColor=444444)

ppt-to-speech converts a slide deck into a verbatim speech script with per-slide timing, speaker notes written back into your .pptx, and a Q&A prep kit with anticipated questions from novice, expert, and skeptic perspectives.

[Scenarios](#scenarios) · [What Makes It Different](#what-makes-the-output-different-from-just-ask-chatgpt) · [Install](#install) · [Theory](#theoretical-grounding) · [Structure](#structure)

---

**其他语言 / Other Languages:**

[简体中文](README.zh-CN.md)

</div>

---

## Scenarios

<table>
<tr>
<td width="50%" valign="top">

### 🎓 Thesis / Proposal Defense

**Script style: argumentative, not descriptive**

The same result slide sounds different at a defense. Instead of "this slide shows X," you say "based on Y, we argue X." Every sentence sounds like you're making a case, not reading aloud. The script gives your words the register of someone defending a position.

</td>
<td width="50%" valign="top">

### 📊 Academic Conference Talk

**Script style: every word knows the clock**

Core slides get full weight; transition slides move fast. The hardest part to improvise — the handoff between slides — is already written. You always sound like you're in control of the pace, not racing to finish.

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🗣️ Panel / Roundtable

**Script style: conversational, not presentational**

Same content, different mode: you're not delivering from start to finish — you're responding, building, redirecting. The script is a set of detachable argument cards, not linear paragraphs, so you can pick up any thread at any moment rather than waiting for your turn.

</td>
<td width="50%" valign="top">

### 🚀 Founding Pitch / Demo Day

**Script style: narrative arc first**

The same numbers slide reads differently in a pitch: conclusion first, then the figure — not the other way around. The script lets the emotional curve carry the audience; data slides slow down, context slides speed up. They follow your rhythm, not the deck's.

</td>
</tr>
</table>

Both **English and Chinese** are first-class, including the common mixed case (English slides, Chinese delivery).

## Usage

Install the skill, then ask Claude things like:

> "这是我的开题答辩 PPT，下周答辩，限时 10 分钟，中文讲。帮我写逐字稿，再准备评委可能问的问题。"

> "I'm pitching at demo day Friday, 5 minutes hard stop. Write me the full talk track and prep me for the brutal questions."

> "18 slides, 12-minute AERA slot — help me cut, write the talk, and prep Q&A."

Outputs: a Markdown rehearsal document + a copy of your deck with the script in the speaker-notes pane (your original file is never modified).

## What makes the output different from "just ask ChatGPT"

- **Never narrates the slide.** The audience can read; the script adds reasoning, transitions, and emphasis the slide can't show.
- **Time math that's real.** Word counts are reconciled against speaking pace (≈140 wpm EN / ≈200 字/min ZH); time is distributed unevenly — core slides get 2–3× the airtime.
- **Q&A that hurts a little.** Three-pass question generation (novice / expert / skeptic) finds the deck's weakest load-bearing claim and attacks it, with composed model answers and scenario-appropriate "I don't know" fallbacks.
- **Respects your work.** Existing speaker notes are treated as author intent; image-only slides trigger a question to you, never invented content.

## Theoretical grounding

The scenario guides are anchored in presentation and argumentation research rather than folklore:

- Mayer, R. E. — *Multimedia Learning* (redundancy principle: narration must not duplicate on-screen text)
- Sweller, J. — Cognitive load theory (spoken syntax simpler than written; listening is costly)
- Toulmin, S. — *The Uses of Argument* (Q&A generation = attacking warrants and backing)
- Hyland, K. — Hedging and metadiscourse in academic discourse (claim calibration in defenses)
- Swales, J. — CARS model (spoken motivation sections)
- Alley, M. — *The Craft of Scientific Presentations* (assertion–evidence slide/script division of labor)
- Monroe's Motivated Sequence (pitch narrative arc)
- Chen, X.-P., Yao, X., & Kotha, S. (2009). Entrepreneur passion and preparedness in business plan presentations. *Academy of Management Journal* (preparedness > displayed passion — why Q&A prep is half the product)
- Heath & Heath — *Made to Stick*; Duarte, N. — *Resonate* (concreteness, contrast structure)

## Structure

```
ppt-to-speech/
├── SKILL.md                       # workflow + scenario routing
├── references/
│   ├── academic/
│   │   ├── thesis-defense.md
│   │   ├── conference-talk.md
│   │   └── panel-discussion.md
│   ├── founding-pitch.md
│   └── qa-preparation.md          # the three-pass question method
└── scripts/
    ├── extract_slides.py          # pptx → JSON (text, tables, alt-text, existing notes)
    └── write_notes.py             # script JSON → speaker notes (non-destructive)
```

Requires `python-pptx` for the two helper scripts.

## License

MIT
