# Handoff Schemas

Agent-to-agent data contracts for the ppt-to-speech pipeline. Producers validate output before handoff; consumers validate input on receipt. Missing required fields → `HANDOFF_INCOMPLETE`.

---

## Schema 1 — Intake Record

**Producer:** intake_agent  
**Consumers:** script_writer_agent, qa_prep_agent, delivery_agent

```
Intake Record {
  schema_version: "1"                        // required
  scenario: "thesis-defense"                 // required; one of the four MODE_REGISTRY modes
              | "conference-talk"
              | "panel-discussion"
              | "founding-pitch"
  language: "en" | "zh" | "mixed"           // required; "mixed" = English slides, Chinese delivery
  delivery_language: "en" | "zh"            // required; the language the presenter will speak
  time_budget_minutes: <number>              // required; null only if user explicitly waived the constraint
  speaking_pace: {                           // required
    words_per_min: <number>,                 // 140 for English, 200 for Mandarin
    chars_per_min: <number>                  // for zh scripts
  }
  target_word_count: <number>               // required; computed: time_budget_minutes × speaking_pace × 0.90
  slide_count: <number>                      // required
  thin_slides: [<number>, ...]              // required; empty array if none; thin = is_text_thin: true
  thin_slides_resolved: true | false        // required; must be true before IR-1 checkpoint clears
  existing_notes_present: true | false      // required; signals delivery_agent to preserve originals
  source_file: "<path>.pptx"               // required
  slides_json_path: "<path>.json"          // required; output of extract_slides.py
  scenario_notes: "<string>"               // optional; user-supplied context, co-panelists, committee names, etc.
  slides: [ <slides array from slides.json> ] // required; full array
}
```

**Validation rules:**
- `thin_slides_resolved` must be `true` before handoff — if thin slides exist, user must have confirmed one-line descriptions or approved blank handling
- `time_budget_minutes` cannot be null for `thesis-defense` or `conference-talk`; for `founding-pitch` it can be null only if user explicitly said timing is flexible
- `scenario_notes` is required (not null) for `panel-discussion`; must include panel theme and known co-panelist positions if available

---

## Schema 2 — Script Record

**Producer:** script_writer_agent  
**Consumers:** qa_prep_agent, delivery_agent

```
Script Record {
  schema_version: "1"                        // required
  intake_record_version: "1"                 // required; must match consumed Intake Record
  scenario: <string>                         // required; copied from Intake Record
  delivery_language: "en" | "zh"            // required; copied from Intake Record
  total_estimated_minutes: <number>          // required; sum of all slide estimates
  time_budget_minutes: <number | null>       // required; copied from Intake Record
  within_budget: true | false               // required; true if total_estimated_minutes ≤ time_budget_minutes × 1.05
  word_count_verified: true | false         // required; true = writer counted words and reconciled estimates
  deliverable_type: "per-slide-script"       // required; "per-slide-script" for all modes except panel-discussion
                  | "position-kit"           // panel-discussion only
  
  // For deliverable_type = "per-slide-script":
  slides: {
    "<slide_number>": {
      script: "<verbatim spoken text>",      // required
      estimated_seconds: <number>,           // required; verified against word count
      delivery_cues: ["[pause]", ...],       // required; empty array if none
      transition_to_next: "<text>"           // required for all slides except the last; empty string for last slide
    },
    ...
  }

  // For deliverable_type = "position-kit" (panel-discussion):
  position_kit: {
    opening_statement: {
      text: "<verbatim spoken text>",        // required
      estimated_seconds: <number>            // required
    },
    talking_points: [
      {
        id: "TP1",                           // required
        bridge_in: "<one-line attach phrase>", // required
        text: "<verbatim spoken prose>",     // required
        estimated_seconds: <number>          // required
      },
      ...
    ],
    moderator_questions: [
      {
        question: "<anticipated question>",  // required
        model_response: "<verbatim text>",   // required
        estimated_seconds: <number>          // required
      },
      ...
    ],
    reaction_material: [
      {
        co_panelist: "<name or role>",       // required if co-panelists named in Intake Record
        build_on: "<text>",                  // required
        respectful_disagreement: "<text>"    // required
      },
      ...
    ]
  }
}
```

**Validation rules:**
- `word_count_verified` must be `true` — writer must have counted actual words and reconciled estimates
- `within_budget` must be `true`; if false, checkpoint IR-3 fires before qa_prep_agent runs
- All slides in Intake Record's `slides` array must appear as keys in `slides` (no missing slides)
- `transition_to_next` must be non-empty for all slides except the final slide
- For `position-kit`, `talking_points` must have at least 3 entries

---

## Schema 3 — QA Record

**Producer:** qa_prep_agent  
**Consumer:** delivery_agent

```
QA Record {
  schema_version: "1"                        // required
  scenario: <string>                         // required; copied from Intake Record
  delivery_language: "en" | "zh"            // required; copied from Intake Record
  total_questions: <number>                  // required; 6–9 standard, 9–12 if user requested thorough prep
  archetype_counts: {                        // required
    novice: <number>,
    expert: <number>,
    skeptic: <number>
  }
  questions: [
    {
      id: "Q1",                              // required; sequential Q1, Q2, ...
      archetype: "novice" | "expert" | "skeptic",  // required
      likely_from: "<who asks this>",        // required; e.g. "committee member outside the subfield"
      question: "<question text>",           // required
      why_they_ask: "<one line>",            // required
      model_answer: "<2–4 spoken sentences>", // required; same language and register as Script Record
      fallback: "<one fallback line>"        // required; scenario-appropriate "I don't know" formula
    },
    ...
  ]
}
```

**Validation rules:**
- `archetype_counts.skeptic` must be ≥ 1 (always at least one skeptic question)
- `archetype_counts.novice` must be ≥ 1
- `archetype_counts.expert` must be ≥ 2
- All `model_answer` fields must be in `delivery_language`; technical terms may stay in slide language
- `fallback` must match the scenario's graceful "I don't know" formula from the scenario reference file

---

## Validation failure handling

If a consumer receives an artifact that fails schema validation:

1. State the missing/invalid fields explicitly: `HANDOFF_INCOMPLETE: missing fields [list]`
2. Do not proceed with the pipeline step
3. Return to the producing agent with the validation error
4. The producing agent corrects and re-delivers

Silent acceptance of malformed handoffs is forbidden.
