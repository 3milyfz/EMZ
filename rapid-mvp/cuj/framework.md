# CUJ Framework 

## User persona & goal
**Persona:** A busy instructor running **12 in-class presentations in 60 minutes** with limited attention for troubleshooting.  
**Goal:** Quickly set up a round, **randomly select the next team (no repeats)**, view team info, and run a **dual-phase timer** (presentation + Q&A) with a **2-minute warning** during presentation.

---

## Product context (current MVP behavior)
### Team list + randomizer
- Team list is created via **manual entry** on the control panel:
  - Required: **team name**
  - Optional: member names, topic
- Randomizer behavior:
  - **No repeats:** selected team is removed from the pool in the backend
  - **No undo:** once a team is selected, cannot be brought back unless reset
  - **No explicit “presented” state:** selected teams are removed from the remaining pool, but not visually marked on the dashboard
  - **Reset:** reset button is located at the top of the window along with a "Teams Remaining" counter. In the backend, the reset button triggers `POST /api/reset`, restoring all IDs and clearing `lastSelected`

### Timer system
- Default durations:
  - Presentation: **7 minutes**
  - Q&A: **3 minutes**
- Warning trigger: during presentation when **remainingSeconds ≤ 120**
- Phase flow: `ready → presentation → qa → complete`
- Pause: by clicking the pause button
- Reset timer: stops timer, set timer phase to ready with displaying of 0

---

## Critical User Journey Audit (Unhappy Path)

### Unhappy path definition
Instructor launches the tool, enters all teams, notices a team name is wrong, tries to correct it, then runs selection + timers. Friction occurs because:
1) Correcting a team name requires **delete + re-add** (no direct edit flow / no undo).  
2) The **dashboard list doesn’t visually reflect** which teams have already been selected (only the remaining count changes).  
3) Timer duration input is functional but has **confusing keyboard behavior** (undeletable `0`, odd formatting like `01`, `010`).  
4) Timer can be forgotten during transitions; the tool **auto-switches to Q&A**, which can unintentionally reduce Q&A time if the instructor speaks between phases.  
5) Dark theme reduces accessibility/legibility in bright classrooms.

---

## Journey audit table
**Total Time to Task Completion (Unhappy Path): ~5–7 minutes before the first team fully completes presentation + Q&A**  
(Setup dominates time; selection itself is fast.)

| Step | User action | Context switch (Y/N + description) | Time | Evidence (Screenshots) |
|---:|---|---|---:|---|
| 1 | Open the app and locate the control panel for setup | N | 5–10s | [UI landing](assets/step-01-landing.png) |
| 2 | Enter **12 team names** (members/topic optional) | **Y — sustained focus shift into data entry** (high cognitive load) | 3–4m | [Team entry panel](assets/step-02-team-entry.png) |
| 3 | Notice one team name is incorrect after entry | N | 5–10s | [Incorrect team visible](assets/step-03-wrong-team.png) |
| 4 | Remove the incorrect team and **manually re-add** it with corrected name | **Y — rework loop** (must reconstruct team info; no “edit in place”) | 20–30s | [Delete + re-add flow](assets/step-04-delete-readd.png) |
| 5 | Set presentation time to **3 min** and Q&A to **2 min** | N | ~5s | [Timer inputs](assets/step-05-timer-input.png) |
| 6 | Try to edit timer fields via keyboard; pressing delete yields **undeletable 0** and formatting like `01`, `010` | N | 15–30s | [Timer input formatting](assets/step-06-timer-formatting.png) |
| 7 | Click “Select next team” | N | ~1s | [Selected team display](assets/step-07-selected-team.png) |
| 8 | Confirm remaining teams count decreases, dashboard list shows which team is live but doesn't show teams that have presented | N | 1s | [Team selection changes](assets/step-08-dashboard-changes.png) |
| 9 | Start presentation timer; rely on auto 2-min warning | N | 2–3s | [Timer running](assets/step-09-timer-running.png) |
| 10 | 2-minute warning appears during presentation (automatic) | N | 0s | [2-min warning](assets/step-10-warning.png) |
| 11 | Instructor comments between presentation and Q&A; forgets to pause; timer **auto-transitions to Q&A** and students lose seconds | N | 5–10s lost | [Phase transition](assets/step-11-auto-transition.png) |
| 12 | End of Q&A; round continues selecting next team | N | 1–3s | [Round continues](assets/step-12-next-round.png) |

---

## Highlights & lowlights (by severity)

| Severity | Finding | Why it matters |
|---|---|---|
| **Great** | Randomizer selection is instant and enforces **no repeats** | Reduces instructor mental bookkeeping; avoids double-calling teams |
| **Great** | 2-minute warning is automatic | Eliminates a common manual timing failure |
| **Moderate** | Manual entry for 12 teams is slow and cognitively taxing | Setup time eats into class time; easy to make typos |
| **Moderate** | Dark theme reduces readability in bright rooms | Instructors will squint/lean in; increases error likelihood |
| **Moderate** | Auto-switch to Q&A can silently steal time if instructor speaks between phases | Small time losses matter when Q&A is 1–2 minutes; feels unfair to students |
| **Severe** | No “presented” visual state on dashboard (list unchanged) | Instructor can’t audit progress at a glance; increases confusion + mistrust |
| **Severe** | Timer input UX: undeletable `0`, formatting like `01`/`010` | Causes hesitation and user confusion despite not a functional issue |
| **Severe** | No undo after selecting a team | Accidental click or mis-selection forces full reset, disrupting flow |

---

## Recommendations (2 high-impact changes)
1) **Add “Presented” visual state**
   - On selection, mark the chosen team as **Presented** (e.g., strikethrough, badge).
   - Result: reduces uncertainty and repeated checking; improves trust.

2) **Fix timer input UX + add a “Transition Buffer”**
   - Timer fields:
     - Support clearing to empty string, then validating on blur
     - Normalize display (no `01`/`010`), clamp to sensible ranges
     - Consider a dedicated mm:ss input or dropdown presets
   - Transition buffer:
     - When presentation hits 0, show modal: “Start Q&A?” with one-click start Or a configurable **+10s buffer** before auto Q&A
   - Result: avoids accidental time loss and reduces timing mistakes.

---

## Pro-tips for using the current MVP
- Enter team names once, then **double-check** before starting selection (since edits are delete + re-add).
- After selecting a team, rely on the **remaining counter** as the source of truth (dashboard list won’t show “presented”).
- For timer edits, if the keyboard delete leaves a 0 you can’t remove, just type the new value over it—the timer will still run correctly.
- If the instructor tends to comment between phases, **pause immediately** before presentation ends to prevent accidental Q&A countdown loss.
- For bright classrooms, increase screen brightness or zoom (dark theme may be harder to read).