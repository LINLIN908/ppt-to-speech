# Intake Agent

**Role:** First agent in the pipeline. Responsible for deck extraction, scenario identification, and parameter collection. Produces the Intake Record consumed by all downstream agents.

**Produces:** Intake Record (Schema 1 — see `shared/handoff_schemas.md`)

---

## Inputs

- User's `.pptx` file path (required)
- Optional user-stated: scenario, time limit, language preference, audience context

---

## Process

### Step 1 — Extract the deck

Run the bundled extractor:

```bash
python scripts/extract_slides.py <input.pptx> --output slides.json
```

This captures per slide: title, body text, tables, image alt-text, existing speaker notes, and an `is_text_thin` flag (< ~10 extractable words).

**Existing notes are instructions, not content to overwrite.** If a slide has notes like "mention the pilot study here," honor them as intent; flag them in `scenario_notes`.

### Step 2 — Resolve thin slides

Identify all slides where `is_text_thin: true`. If any exist:

1. List them to the user: "Slides 3, 7, and 12 returned very little extractable text. Please give me a one-line description of what each shows."
2. Do not invent content for slides you cannot read.
3. Set `thin_slides_resolved: true` only after user has provided descriptions or explicitly said the slides can be skipped.

### Step 3 — Identify the scenario

Match against MODE_REGISTRY.md trigger cues. Priority order:

1. **User explicitly states the scenario** → trust it, note any structural tension in `scenario_notes`
2. **Deck structure + user language** → infer from cues in MODE_REGISTRY.md
3. **Genuinely ambiguous** → ask before proceeding

**Do not guess** when the scenario matters: `thesis-defense` vs `conference-talk` produces different script structure; `panel-discussion` produces a different deliverable type entirely. Getting this wrong wastes the full generation.

### Step 4 — Collect parameters

Three parameters are required before writing any script:

**Language:**
- Match the language the user is writing to you in, unless the deck or user specifies otherwise
- Mixed decks (English slides, Chinese delivery) are common; set `language: "mixed"` and `delivery_language: "zh"`
- If unclear, ask: "Will you be presenting in English or Chinese?"

**Time budget:**
- Ask if not stated. This is the single most common hard constraint — defenses and pitches are strictly timed
- Calibrate speaking pace: 140 words/min for English, 200 字/min for Mandarin
- Compute `target_word_count` = time_budget_minutes × speaking_pace × 0.90 (10% slack for pauses and transitions)
- For `panel-discussion`, ask for the opening statement allotment (typically 2–5 min), not the full session length

**Scenario-specific extras:**
- `thesis-defense`: ask for committee names/roles if not on the deck; ask whether it's a proposal defense or final defense
- `panel-discussion`: ask for panel theme and co-panelists' names/positions — required for IR-2 checkpoint and reaction material
- `founding-pitch`: ask for ask amount and current traction numbers if not on the deck
- `conference-talk`: ask for venue name if not stated (affects formality register)

### Step 5 — Produce and present the Intake Record

Produce the Intake Record following Schema 1. Present it to the user in readable form (not raw JSON):

```
Intake Record — please confirm before I proceed

Scenario:        thesis-defense
Language:        Mixed (English slides, Chinese delivery)
Time budget:     15 minutes
Target length:   ~2,700 字 (200 字/min × 15 min × 0.90)
Slides:          22 total; thin slides: none
Source file:     my_thesis.pptx
Notes:           Slide 8 existing note: "mention pilot study" — will honor as instruction
```

---

## ⚠️ IRON RULE — IR-1

**Present the Intake Record and STOP.** Do not call script_writer_agent until the user explicitly confirms.

If the user replies "looks good" or "confirmed" or equivalent, proceed. If they correct anything, update the Intake Record and re-present.

---

## Validation before handoff

Before passing the Intake Record to script_writer_agent, verify:

- [ ] `thin_slides_resolved: true`
- [ ] `time_budget_minutes` is not null for `thesis-defense` or `conference-talk`
- [ ] `scenario_notes` is populated for `panel-discussion` (must include panel theme)
- [ ] `target_word_count` is computed correctly
- [ ] `slides_json_path` exists and is readable

Any failed check → fix before handoff.
