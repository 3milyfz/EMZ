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

| Step | User action | Context switch (Y/N + description) | Time | Evidence |
|---:|---|---|---:|---|
| 1 | Open the app and locate the control panel for setup | N | 5–10s | `[UI landing](/rapid-mvp/cuj/assets/step-01-landing.png)` |
| 2 | Enter **12 team names** (members/topic optional) | **Y — sustained focus shift into data entry** (high cognitive load) | 3–4m | `[Team entry panel](/rapid-mvp/cuj/assets/step-02-team-entry.png)` |
| 3 | Notice one team name is incorrect after entry | N | 5–10s | `[Incorrect team visible](/rapid-mvp/cuj/assets/step-03-wrong-team.png)` |
| 4 | Remove the incorrect team and **manually re-add** it with corrected name | **Y — rework loop** (must reconstruct team info; no “edit in place”) | 20–30s | `[Delete + re-add flow](/rapid-mvp/cuj/assets/step-04-delete-readd.png)` |
| 5 | Set presentation time to **3 min** and Q&A to **2 min** | N | ~5s | `[Timer inputs](/rapid-mvp/cuj/assets/step-05-timer-input.png)` |
| 6 | Try to edit timer fields via keyboard; pressing delete yields **undeletable 0** and formatting like `01`, `010` | N | 15–30s | `[Timer input formatting](/rapid-mvp/cuj/assets/step-06-timer-formatting.png)` |
| 7 | Click “Select next team” | N | ~1s | `[Selected team display](/rapid-mvp/cuj/assets/step-07-selected-team.png)` |
| 8 | Confirm remaining teams count decreases, dashboard list shows which team is live but doesn't show teams that have presented | N | 1s | `[Team selection changes](/rapid-mvp/cuj/assets/step-08-dashboard-changes.png)` |
| 9 | Start presentation timer; rely on auto 2-min warning | N | 2–3s | `[Timer running](/rapid-mvp/cuj/assets/step-09-timer-running.png)` |
| 10 | 2-minute warning appears during presentation (automatic) | N | 0s | `[2-min warning](/rapid-mvp/cuj/assets/step-10-warning.png)` |
| 11 | Instructor comments between presentation and Q&A; forgets to pause; timer **auto-transitions to Q&A** and students lose seconds | N | 5-10s lost | `[Phase transition](/rapid-mvp/cuj/assets/step-11-auto-transition.png)` |
| 12 | End of Q&A; round continues selecting next team | N | 1–3s | `[Round continues](/rapid-mvp/cuj/assets/step-12-next-round.png)` |

---