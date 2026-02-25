# Feature Prioritization — Cultivate (Demo2)

## 1 Why these features (strategy + competitive benchmark)
Our customer discovery indicates that **Facebook Marketplace** is the most-used alternative channel for our target segment. In our **Competitive Review CUJ**, we found that while Facebook Marketplace is popular and feature-rich, it requires **manual entry** of key listing fields (item type, title, description). Many farmers therefore spend minimal time on listings, producing **missing/unstructured** posts that increase back-and-forth and reduce buyer confidence.

Cultivate’s strategy is to win on **time-to-post** and **listing completeness** (without removing user control). We prioritize a **Human-in-the-Loop (HITL)** workflow where AI generates the basic fields, and the farmer acts as an evaluator before publishing—freeing attention for the rest of the selling process (responding to demand, negotiating logistics, and planning future crops).

This sprint therefore prioritizes two core friction reducers:
- **Friction 1: Listing creation & “business language”** → Photo → Draft listing (HITL) + Price hints
- **Friction 2: Negotiation & coordination overhead** → Chat thread (created on offer) + Copilot autocomplete (HITL)

We treat the end-to-end market workflow as an epic, but we strategically build only the minimum scaffolding needed to validate value around those friction points.

---

## 2 Implementation timeline (this sprint vs deferred)
### Built / Focused This Sprint (P0)
- **Auto-Lister (Photo → Listing Draft, HITL)**
- **Price hint suggestion (StatCan reference range)**
- **Offer submission (minimal structured response)**
- **Chat created on offer submission + message icon on responses**
- **Chat Copilot autocomplete/draft (HITL) + deterministic fallback**

### Minimum scaffolding to support P0 validation (P0/P1 as needed)
- Minimal bounty view (restaurant post + farmer view)
- Message persistence (thread history)

### Deferred to future sprints (P2)
- Fit Score (full version)
- Filled vs remaining tracking (full version)
- Ag Guide as AI agent (tool-using evaluator)

### Out of Scope for MVP (P3)
- ESG validation at registration
- Escrow payments

---

## 3 Feature-to-value mapping (for every identified feature)
> For each feature: CUJ step(s), user problem, hypothesis, success metric, priority, and build decision.

### P0 — Core value features (directly reduce top friction)

#### F1 — Auto-Lister: Photo → Listing Draft (HITL)
- **CUJ step(s):** Phase 2.1 (Supply creation)
- **User problem:** Farmers avoid lengthy manual typing; manual listings become incomplete/unstructured (Competitive Review vs Facebook Marketplace).
- **Hypothesis:** Auto-generating item/title/description from a photo (with farmer review/edit) reduces time-to-post and increases listing completeness.
- **Success metric:** Median time-to-post listing decreases vs manual baseline; required fields are populated before publish.
- **Priority rationale:** Core differentiator vs Facebook Marketplace and directly targets the largest creation friction.
- **Build decision:** **Build this sprint (P0).**

#### F2 — Price hint suggestion (StatCan reference range)
- **CUJ step(s):** Phase 2.1 (Listing finalization)
- **User problem:** Pricing uncertainty increases negotiation overhead and reduces confidence.
- **Hypothesis:** Showing a reference range + suggested price reduces uncertainty and accelerates posting.
- **Success metric:** Fewer edits to price field; faster publish; fewer price clarification messages.
- **Priority rationale:** Low cost, high leverage; complements Auto-Lister by reducing another high-friction field.
- **Build decision:** **Build this sprint (P0).**

#### F3 — Offer submission (minimal structured response)
- **CUJ step(s):** Phase 3.2 (Commitment)
- **User problem:** Pure “interest chats” can be low-signal; buyers need a concrete response (qty/price/timing) to decide.
- **Hypothesis:** Requiring a minimal structured offer reduces ambiguity and improves coordination quality.
- **Success metric:** Offers include required fields; restaurant can compare offers at least at a basic level.
- **Priority rationale:** Enables the “chat created on offer” design and prevents spam threads.
- **Build decision:** **Build this sprint (P0).**

#### F4 — Chat created on offer + message icon on responses
- **CUJ step(s):** Phase 4.1–4.2 (Coordination)
- **User problem:** Messaging loses context and creates repeated clarifications; but opening chat too early can create noise.
- **Hypothesis:** Creating chat only after an offer anchors discussion to a plausible transaction and reduces low-signal back-and-forth.
- **Success metric:** Users coordinate inside a thread tied to offer; restaurant can open chat via message icon for any response.
- **Priority rationale:** Directly supports the sprint’s coordination friction reduction goal.
- **Build decision:** **Build this sprint (P0).**

#### F5 — Chat Copilot autocomplete/draft (HITL) + deterministic fallback
- **CUJ step(s):** Phase 4.3 (Communication)
- **User problem:** “Professional writing” and repetitive confirmations are high friction for farmers.
- **Hypothesis:** Copilot reduces drafting time and increases clarity; user remains editor/approver.
- **Success metric:** Drafting time decreases; fewer rewrites; fewer missing-info clarifications.
- **Priority rationale:** Targets the second major friction point; supports our “professional wrapper” positioning vs informal channels.
- **Build decision:** **Build this sprint (P0).**
- **Reliability note:** We are experimenting with multiple LLM APIs; for demo reliability, we maintain a rule-based template fallback.

---

### P0/P1 — Enabling scaffolding (unblocks CUJ steps needed to test P0)

#### F6 — Minimal bounty creation + bounty view
- **CUJ step(s):** Phase 1.1 & Phase 3.1
- **User problem:** Without a bounty surface, offers and chat threads cannot exist.
- **Hypothesis:** Minimal bounty fields are enough to test the offer→chat flow.
- **Success metric:** Restaurant can post; farmer can view; offer submission works end-to-end.
- **Priority rationale:** Necessary scaffolding; keep minimal to avoid bloat.
- **Build decision:** **Build this sprint (P0 scaffolding).**

#### F7 — Message persistence (thread history)
- **CUJ step(s):** Phase 4 (Coordination)
- **User problem:** Without persistence, decisions are lost and the demo is not reproducible.
- **Hypothesis:** Persisted history reduces repeated clarifications and enables copilot context.
- **Success metric:** Thread renders reliably; last messages available for copilot prompt.
- **Priority rationale:** Required for defensible coordination and copilot usefulness.
- **Build decision:** **Build this sprint (P0 scaffolding).**

---

### P1 — Valuable but not required for this sprint’s core bet

#### F8 — Map discovery (bounties/listings)
- **CUJ step(s):** Phase 1.2 / Phase 2.2 (Discovery)
- **User problem:** Finding local demand/supply is time-consuming.
- **Hypothesis:** Map filters reduce time-to-find relevant opportunities.
- **Success metric:** Lower discovery time; fewer irrelevant clicks.
- **Priority rationale:** Useful, but we can validate this sprint’s primary value (listing + chat friction reduction) using seeded bounties/listings.
- **Build decision:** **If capacity remains (P1).**

---

### P2 — Deferred (future sprints; high value but not required for this sprint’s validation)

#### F9 — Fit score (full version)
- **CUJ step(s):** Phase 3.3 / Phase 5.1 (Evaluation/acceptance)
- **User problem:** Restaurants need consistent comparisons; farmers need feasibility feedback.
- **Metric:** Reduced mismatches; faster acceptance.
- **Build decision:** **Defer (P2).**

#### F10 — Filled vs remaining tracking (full version)
- **CUJ step(s):** Phase 5.1 (Multi-supplier fulfillment)
- **User problem:** Managing partial fulfillment across farms is heavy.
- **Metric:** Reduced admin overhead; fewer coordination loops.
- **Build decision:** **Defer (P2).**

#### F11 — Ag Guide (AI agent: evaluator + tiered guidance)
- **CUJ step(s):** Phase 1.3 (Capability-building)
- **User problem:** Farmers need guidance to scale or switch crops; buyers want long-run supplier depth.
- **Risk:** Tool-using agent increases grounding risk and is hard to validate in one sprint.
- **Build decision:** **Defer (P2)**; MVP uses tiered rule-based checklists/curated links if needed.

---

### P3 — Out of Scope for MVP

#### F12 — ESG validation at registration
- **CUJ step(s):** Trust/credibility
- **Build decision:** Out of scope (P3).

#### F13 — Escrow payments
- **CUJ step(s):** Payment completion
- **Build decision:** Out of scope (P3).