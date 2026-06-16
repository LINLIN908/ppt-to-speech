# Mode Registry

Single source of truth for all ppt-to-speech modes. SKILL.md and agent files reference this registry — do not define modes elsewhere.

## Modes

| Mode | Trigger cues (examples) | Primary deliverable | Agent sequence | Oversight |
|---|---|---|---|---|
| `thesis-defense` | "答辩" "defense" "committee" "proposal defense"; deck structured as RQs → method → results → limitations | Per-slide verbatim script + committee-calibrated Q&A | intake → script_writer → qa_prep → delivery | High |
| `conference-talk` | Named conference/venue; "10-minute slot" "paper presentation" "会议报告" "学术报告" | Triage-first script + backup slides map + peer Q&A | intake → script_writer → qa_prep → delivery | Medium |
| `panel-discussion` | "panel" "roundtable" "圆桌" "forum"; discussion theme rather than paper; few or no slides | Position kit: opening statement + talking points + reaction material | intake → script_writer → qa_prep → delivery | High |
| `founding-pitch` | "investors" "demo day" "路演" "pitch"; market / traction / team slides | Hook-first investor script + investor Q&A | intake → script_writer → qa_prep → delivery | Medium |

## IRON RULE checkpoints

These checkpoints cannot be skipped:

| Checkpoint | When | Rule |
|---|---|---|
| **IR-1** | After intake_agent finishes | Present Intake Record and STOP. Do not call script_writer until user explicitly confirms. |
| **IR-2** | `panel-discussion` only, before script_writer | Confirm panel theme and co-panelists' positions. This scenario changes the deliverable type; wrong assumptions require a full restart. |
| **IR-3** | After script_writer, if total runtime > 5% over time limit | Show the time arithmetic and ask user to approve or trim before writing Q&A. |

## Oversight levels

**High** (`thesis-defense`, `panel-discussion`): scenario requires specific prior knowledge — committee composition or co-panelist positions — that dramatically shapes the output. Missing this information forces a restart; stop and ask before proceeding.

**Medium** (`conference-talk`, `founding-pitch`): time limit is the only mandatory missing parameter; language and scenario can be inferred with reasonable confidence from deck structure and user message.

## Disambiguation rules

If the scenario is genuinely ambiguous, ask — do not guess. Sub-scenarios diverge enough that a wrong guess wastes the full generation:

- "I'm presenting my research next week" → ask whether it's a defense, a conference slot, or a lab talk
- Slides look like thesis structure but user says "conference" → trust the user's stated scenario; note the structural tension in the Intake Record
- No slides at all, or only 1–2 overview slides → `panel-discussion` is likely; confirm before proceeding
- "路演" without investor context → could be internal pitch or demo day; ask audience type
