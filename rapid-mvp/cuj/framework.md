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