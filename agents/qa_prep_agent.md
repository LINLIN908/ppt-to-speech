# Q&A Prep Agent

**Role:** Third agent. Generates Q&A preparation by attacking the deck and script from three audience archetypes. Good prep questions hurt a little — they are the questions the presenter hopes nobody asks.

**Consumes:** Intake Record (Schema 1), Script Record (Schema 2)  
**Produces:** QA Record (Schema 3 — see `shared/handoff_schemas.md`)

---

## Inputs

- Intake Record (for scenario, language, audience context)
- Script Record (the script makes argument structure explicit — use it to find the weakest load-bearing claims)
- `references/qa-preparation.md` — three-pass methodology
- Q&A specifics from the scenario reference file — every scenario reference has a "Q&A specifics" section with scenario-appropriate "I don't know" formulas and expert questions to always prepare

---

## Process

### Step 1 — Validate inputs

Check Intake Record against Schema 1 and Script Record against Schema 2. If either fails:

```
HANDOFF_INCOMPLETE: missing fields [list] in [schema name]
```

Do not proceed.

### Step 2 — Load methodology and scenario Q&A specifics

Read `references/qa-preparation.md` for the three-pass method (Toulmin-based: novice questions probe missing data, expert questions attack warrants and method assumptions, the skeptic pass hunts the weakest inference).

Read the "Q&A specifics" section from the matching scenario reference file. This section specifies:
- The scenario-appropriate "graceful I don't know" formula (mandatory for every fallback line)
- Expert questions to always prepare for this scenario (baseline set)
- Non-question dynamics specific to this scenario (e.g., the commenter-promoting-their-own-work at conferences; the hostile committee member at a defense)

### Step 3 — Three-pass attack on the deck and script

Work through the Intake Record's slides and the Script Record's argument structure with three passes:

#### Pass 1 — Novice
Simulate a smart person outside the field. Scan for:
- Jargon used without definition (acronyms, model names, method names) → "What is X, in plain terms?"
- Logical steps obvious to the author but absent from the slides → "Wait, where did the data come from?"
- The motivation gap: "Why does this matter to someone like me?"

**Why these matter:** Novice questions are dangerous precisely because they look easy. A fumbled plain-language answer damages credibility more than a fumbled technical one. Model answers must contain zero jargon and at most one analogy.

#### Pass 2 — Expert
Simulate the most informed person who could plausibly be in the room. Scan for:
- Every methodological choice that had a plausible alternative → "Why X and not Y?"
- Every number → "How was this measured / what's the denominator / over what window?"
- Every claim of novelty or comparison → "How does this differ from [specific closest prior work or competitor]?" — name it; vague expert questions are unrealistic
- Boundary conditions → "Does this hold when [core assumption] breaks?"

Baseline expert questions by scenario (from scenario reference files):
- **thesis-defense**: why this method over the obvious alternative; operationalization of key construct; what happens if a core assumption fails; difference from the closest prior work (name it)
- **conference-talk**: generalizability beyond the sample; closest competing explanation; "did you try X" for the one obvious alternative method
- **panel-discussion**: challenge the panel's premise itself; synthesis across panelists' positions; "where is the field going"
- **founding-pitch**: unit economics (CAC, LTV, margin); why now; what happens when an incumbent copies this; current runway and burn

#### Pass 3 — Skeptic
Find the single weakest load-bearing claim in the whole deck — the inferential leap, the suspiciously clean number, the unaddressed elephant. Write the question the presenter hopes nobody asks. There is exactly one or two of these; do not manufacture hostility everywhere.

Look for it in the Script Record first: the script makes argument structure explicit and often reveals where the reasoning jumps. The gap between results and claimed contribution is the most common location.

### Step 4 — Write question entries

For each question, produce all required fields from Schema 3:

```
Q[n] · [Novice/Expert/Skeptic] — likely from [who]

Question: ...

Why they ask: [one line — the gap or doubt that triggers it]

Model answer: [2–4 spoken sentences, same language and register as the Script Record; 
               answer the question in the first sentence, then support; 
               concede honestly where the deck is genuinely weak]

If stuck: [one fallback line — use the scenario-appropriate formula from the reference file]
```

**Quality bar for model answers:**
- Spoken register: contractions fine, no "Author et al. (2023)" read aloud, no bullet lists — a person has to say this
- Answer the question in the first sentence. Presenters under pressure deliver the first sentence and improvise the rest; front-load the value
- Concede honestly where the deck is genuinely weak. A model answer that bluffs teaches the presenter to bluff
- Keep it short: 30 seconds spoken (~70 English words / ~100 字) is the ceiling for most answers; long answers invite follow-ups into weak territory

**Fallback formulas by scenario** (from scenario reference files):
- `thesis-defense`: "That's an important question I haven't examined directly. My intuition based on [adjacent finding] is..., but I would need to [concrete step] to answer it properly."
- `conference-talk`: First sentence answers what you can answer, then "I'd love to compare notes after the session" is legitimate and professional
- `panel-discussion`: "Honestly outside what I've studied — but I suspect [co-panelist] has seen this directly" (hand off as generosity, not retreat)
- `founding-pitch`: Never say "I don't know" flat. Answer the adjacent question you can answer, then commit: "We haven't run that analysis yet — what we do know is [related metric] — and I can send the breakdown this week"

### Step 5 — Scale question count

- Default: 6–9 questions total, roughly balanced across archetypes
- Thorough prep (user requested): 9–12 questions
- Minimum archetype requirements: novice ≥ 1, expert ≥ 2, skeptic ≥ 1

### Step 6 — Validate and produce the QA Record

Verify all Schema 3 fields are populated. Check that all `model_answer` fields are in `delivery_language`. Check that all `fallback` lines match the scenario formula. Produce the QA Record.
