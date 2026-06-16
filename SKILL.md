---
name: ppt-to-speech
description: Turn a PowerPoint deck into a ready-to-deliver speech script plus Q&A preparation. Use this skill whenever the user has a .pptx file and wants to know what to SAY — requests like "write a speech script for my slides", "I don't know how to present this deck", "prepare my defense talk", "write my pitch narration", "帮我写演讲稿/逐字稿/讲稿", "准备答辩", "路演稿", or "prep me for the Q&A session". Also trigger when the user asks for speaker notes, a talk track, rehearsal material, anticipated audience questions, conference presentation prep (10–15 min paper talks, 会议报告), or panel/roundtable preparation (opening statements and talking points, 圆桌论坛). Covers four scenarios — thesis/proposal defense, academic conference talk, panel discussion, and founding pitch (investor pitch, demo day) — and outputs both a Markdown script document and speaker notes written back into the deck, plus a Q&A prep section with questions from novice, expert, and skeptic perspectives.
---

# PPT to Speech

Convert a slide deck into (1) a per-slide verbatim speech script, (2) speaker notes embedded back into the `.pptx`, and (3) a Q&A preparation section with anticipated questions and model answers.

The core insight: a good script is not a narration of what's on the slide. The audience can read. The script's job is to add what the slide *cannot* show — reasoning, transitions, emphasis, and story.

---

## Pipeline

Four agents run in sequence. Each agent has its own spec file in `agents/`.

```
intake_agent  →  script_writer_agent  →  qa_prep_agent  →  delivery_agent
```

| Agent | File | Responsibility |
|---|---|---|
| intake_agent | `agents/intake_agent.md` | Extract deck, detect scenario, collect parameters, produce Intake Record |
| script_writer_agent | `agents/script_writer_agent.md` | Write verbatim script (or position kit for panels), produce Script Record |
| qa_prep_agent | `agents/qa_prep_agent.md` | Attack deck from three archetypes, produce QA Record |
| delivery_agent | `agents/delivery_agent.md` | Assemble Markdown document + slides_script.json + CLI command |

Agent handoff contracts are defined in `shared/handoff_schemas.md`. A missing required field triggers `HANDOFF_INCOMPLETE` and the pipeline stops.

---

## Modes

Four modes, one per scenario. See `MODE_REGISTRY.md` for full trigger cues, oversight levels, and disambiguation rules.

| Mode | Deliverable type |
|---|---|
| `thesis-defense` | Per-slide verbatim script + committee Q&A |
| `conference-talk` | Triage-first script + backup slide map + peer Q&A |
| `panel-discussion` | Position kit (opening statement + talking points + reaction material) + panel Q&A |
| `founding-pitch` | Hook-first investor script + investor Q&A |

---

## ⚠️ IRON RULE checkpoints

These checkpoints cannot be skipped or bypassed:

**IR-1 — After intake_agent:**
Present the Intake Record to the user. Do not call script_writer_agent until the user explicitly confirms. If they correct anything, update and re-present.

**IR-2 — `panel-discussion` only, before script_writer_agent:**
Confirm the panel theme and co-panelists' positions before writing anything. Missing this forces a full restart.

**IR-3 — After script_writer_agent, if total runtime > 5% over time limit:**
Show the word count arithmetic and ask the user to approve the overrun or trim before proceeding to qa_prep_agent.

---

## Anti-patterns (enforced by script_writer_agent)

- **No "This slide shows..." / "As you can see here..."** — the audience can read
- **No restating visible slide text** — add the implication, not the repetition
- **No uniform pacing** — core contribution slides get 2–3× the time of housekeeping slides
- **No unverified time estimates** — word count must be counted and reconciled with stated minutes
- **No missing transitions** — every slide boundary needs an explicit bridge sentence
- **No flattering Q&A questions** — good prep questions hurt a little

---

## Reference files

| File | Used by |
|---|---|
| `references/academic/thesis-defense.md` | script_writer_agent, qa_prep_agent |
| `references/academic/conference-talk.md` | script_writer_agent, qa_prep_agent |
| `references/academic/panel-discussion.md` | script_writer_agent, qa_prep_agent |
| `references/founding-pitch.md` | script_writer_agent, qa_prep_agent |
| `references/qa-preparation.md` | qa_prep_agent |
| `templates/thesis-defense-template.md` | script_writer_agent |
| `templates/conference-talk-template.md` | script_writer_agent |
| `templates/panel-discussion-template.md` | script_writer_agent |
| `templates/founding-pitch-template.md` | script_writer_agent |
