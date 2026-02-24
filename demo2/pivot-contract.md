# Pivot Contract — Demo2 Core Features Sprint (Cultivate)
*(Combined AI Assist Pack: Image Recognition + Chat Copilot)*

## Hypothesis
If we implement a **chat-first matching workflow** where an **inquiry chat thread opens when a farmer clicks “Interested” on a bounty**, and the farmer can convert that conversation into a **structured offer** with **fit score** and the restaurant can **accept offers with filled/remaining updates**, then users will complete the end-to-end CUJ more reliably and with lower friction than our baseline single-interaction prototype.

In this sprint, we additionally ship an **AI Assist Pack (HITL)** for farmers:
- **Photo → Listing Draft (image recognition)** to reduce listing creation friction, and
- **Chat Copilot autocomplete/draft** to reduce “professional writing” friction in negotiation.

**We expect (by the trigger date):**
- **≥ 4/5** CUJ trials successfully complete the end-to-end flow (KM1), and  
- Median **friction events** per trial decrease by **≥ 30%** vs baseline (KM2).  
Separately, we expect the AI Assist Pack to reduce:
- listing creation time by **≥ 40%** vs manual listing (KM3a), and  
- message drafting time by **≥ 30%** vs no-copilot messaging (KM3b).

## Kill metrics
On the trigger date, run **5** end-to-end CUJ trials (team members + peers acting as personas).

### KM1 — Completion rate (primary)
Pivot is triggered if fewer than **4/5** trials successfully complete:

`Restaurant posts bounty → Farmer finds bounty via map → Farmer clicks “Interested” (thread opens) → Chat clarifies logistics/specs (with/without copilot) → Farmer submits structured offer → Fit score displayed → Restaurant accepts offer(s) and filled/remaining updates → Status set to matched/fulfilled`.

### KM2 — Friction reduction (primary)
Pivot is triggered if median friction events per trial do **not** decrease by at least **30%** vs baseline.

- **Friction events** are counted as: backtracking, confusion requiring explanation, missing fields that block progress, repeated clarifications due to unclear specs/terms, and “dead ends” where users don’t know the next action.

### KM3a — Photo → Listing Draft time savings (secondary, AI Assist Pack)
AI Assist Pack (image recognition) is considered unsuccessful if median time to create a listing using photo→draft is **not ≥ 40% faster** than the manual listing flow (timed trials).

**If KM3a fails but KM1/KM2 pass:** we do **not** pivot away from the product; we pivot **within** the feature to Fallback C (manual templates-first listing).

### KM3b — Chat Copilot drafting time savings (secondary, AI Assist Pack)
AI Assist Pack (copilot) is considered unsuccessful if median time to draft a message in the inquiry thread is **not ≥ 30% faster** than without copilot (timed trials).

**If KM3b fails but KM1/KM2 pass:** we do **not** pivot away from the product; we pivot **within** the feature to Fallback A (templates-first messaging).

**Pre-commitment rule:** We will not adjust any threshold mid-sprint.

## Trigger date (firm decision point)
**Trigger Date:** end of print5 (2026-02-25)

On this date we run the 5 trials, log results, and make a **pivot vs persevere** decision.

## Fallback options (pre-defined pivots)
### Fallback A — No copilot: templates-first messaging
If KM3b fails or copilot introduces confusion:
- Replace AI autocomplete with structured templates, canned replies, and better defaults (still chat-first).
- Re-test: message drafting time + KM1/KM2 impact.

### Fallback B — Offer-first: minimal chat
If chat-first creates negotiation loops and reduces conversion (KM1 fails due to chat stalling):
- Require a lightweight structured offer (qty/price/timing) to open the thread; chat becomes post-offer clarification only.
- Re-test: conversion from bounty view → offer submitted, time-to-accept, KM1.

### Fallback C — No image AI: manual listing templates
If KM3a fails due to inaccurate/slow photo→draft:
- Replace photo→draft with a guided manual form + defaults + examples (standardized schema).
- Re-test: listing creation time + listing completeness; ensure KM1/KM2 unaffected.

## Measurement protocol (what we capture)
For each of 5 trials:
- completion yes/no (KM1)
- time per step: post bounty, find bounty, open thread, send inquiry message, submit offer, accept offer
- friction event count + notes (KM2)
- timed listing creation: manual vs photo→draft (KM3a)
- timed message drafting: no-copilot vs copilot (KM3b)

Additionally:
- screen recordings for at least **2** trials
- results table appended below after trigger date