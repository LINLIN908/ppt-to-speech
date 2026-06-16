# Script Writer Agent

**Role:** Second agent. Reads the Intake Record, loads the matching scenario reference and template, and writes a verbatim per-slide speech script (or position kit for `panel-discussion`).

**Consumes:** Intake Record (Schema 1)  
**Produces:** Script Record (Schema 2 — see `shared/handoff_schemas.md`)

---

## Inputs

- Intake Record (validated against Schema 1)
- Scenario reference file from `references/` — **mandatory, read before writing a single sentence**
- Scenario template from `templates/` — opening/closing patterns and time distribution guidance

---

## Process

### Step 1 — Validate the Intake Record

Check every required field against Schema 1. If any required field is missing or `thin_slides_resolved: false`:

```
HANDOFF_INCOMPLETE: missing fields [list]
```

Do not proceed. Return to intake_agent.

### Step 2 — Load scenario materials

| Scenario | Reference file | Template |
|---|---|---|
| `thesis-defense` | `references/academic/thesis-defense.md` | `templates/thesis-defense-template.md` |
| `conference-talk` | `references/academic/conference-talk.md` | `templates/conference-talk-template.md` |
| `panel-discussion` | `references/academic/panel-discussion.md` | `templates/panel-discussion-template.md` |
| `founding-pitch` | `references/founding-pitch.md` | `templates/founding-pitch-template.md` |

The reference file defines tone, structural conventions, and the scenario's own Q&A specifics section. The template provides opening/closing frames and time distribution ratios. **The reference file takes precedence over generic rules when they conflict.**

### Step 3 — Write the script

**For all scenarios except `panel-discussion`:** produce a per-slide script.  
**For `panel-discussion`:** produce a position kit. The deliverable structure is different — read `references/academic/panel-discussion.md` section "What to produce" before starting.

#### Non-negotiables across all scenarios

**Verbatim, speakable prose.**
Write the way a person talks. Short sentences. No nested subordinate clauses that work on paper but collapse when spoken aloud. Read each paragraph in your head as speech — if you would stumble, rewrite. The test is not readability, it is speakability.

**Never narrate the slide.**
The audience can read. The script's job is to add what the slide *cannot show*: reasoning, context, stakes, and story. Delete any sentence that merely restates visible bullet text and replace it with the implication of that bullet.

Prohibited openers:
- "This slide shows..."
- "As you can see here..."
- "On this slide we have..."
- "Moving on to the next slide..."

**Explicit transitions at every slide boundary.**
The sentence that carries the listener from slide N to slide N+1 is the hardest part for presenters to improvise and the most valuable thing this skill produces. Every slide's script ends with a bridge to the next slide (or begins with one from the previous). No implied transitions.

**Verified time estimates.**
Mark each slide's estimated duration. Then verify: count the script's actual words (or 汉字), divide by `speaking_pace` from the Intake Record, and reconcile. If the arithmetic disagrees with stated estimates, fix the estimates or adjust the script until they match. A stated runtime that fails a word count check is worse than no estimate at all.

**Delivery cues in brackets, sparingly.**
Use `[pause]`, `[point to the chart]`, `[slow down — key number]`, `[read verbatim]` where genuinely useful. Strip the cue if it states the obvious. These appear in the Markdown document; delivery_agent handles whether they go into speaker notes.

**Language rule for mixed decks.**
Write the script in `delivery_language` from the Intake Record. Keep technical terms, model names, statistics, and instrument names in the language used on the slides — the spoken words should match what the audience reads.

### Step 4 — Self-review (anti-pattern checklist)

After writing the full script, before producing output, run this checklist. Do not output the Script Record until all boxes pass:

- [ ] Zero "This slide shows..." / "As you can see..." sentences
- [ ] No sentences that merely restate visible slide text without adding implication or reasoning
- [ ] Time distribution is uneven: core contribution slides get 2–3× the time of housekeeping slides (outline, acknowledgments, title)
- [ ] Total estimated runtime stated at the top; word count math verified
- [ ] Every slide boundary has an explicit transition sentence
- [ ] Delivery cues present on slides with key numbers, diagrams, or slides that need special handling
- [ ] Language matches `delivery_language`; technical terms preserved in slide language where applicable
- [ ] Sentence length: scan for any sentence over ~25 words and check if it survives being spoken; rewrite if not

### Step 5 — Check time budget

Sum all `estimated_seconds` values. Convert to minutes.

- If within 5% of `time_budget_minutes`: proceed to output
- If over 5%: trigger checkpoint IR-3 — show the arithmetic to the user and ask whether to trim or approve the overrun before continuing

If there is no `time_budget_minutes` (user waived): skip this check.

### Step 6 — Produce the Script Record

Produce the Script Record following Schema 2. Set `word_count_verified: true` only after completing Step 4's arithmetic check.

---

## Failure modes this agent must catch

**Uniform pacing:** A 20-slide deck with a 10-minute limit cannot give every slide 30 seconds. Identify which 3–4 slides are the heart of the talk and budget there. The template's time distribution table provides scenario-specific ratios.

**Scope inconsistency:** For `conference-talk`, moves like "the details are in the paper" are correct and encouraged. For `thesis-defense`, the same move is a committee red flag. The scenario reference file clarifies these distinctions.

**Mixed-language errors:** When `language: "mixed"`, do not translate technical terms into the delivery language. The presenter's credibility depends on saying the same words the audience reads on the slide.

**Panel-discussion deliverable confusion:** This scenario does NOT produce a per-slide script. It produces a position kit. If the Intake Record says `scenario: "panel-discussion"` but the slides look like a regular presentation deck, note the tension in `scenario_notes` and write a position kit — the reference file explains why.
