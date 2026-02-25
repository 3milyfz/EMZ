# Feature Prioritization — Cultivate (Demo2)

## 1 Why these features (strategy + competitive benchmark)
Our customer discovery indicates that **Facebook Marketplace** is the most-used alternative channel for our target segment. In our **Competitive Review CUJ**, we found that while Facebook Marketplace is popular and feature-rich, it requires **manual entry** of key listing fields (item type, title, description). Many farmers therefore spend minimal time on listings, producing **missing/unstructured** posts that increase back-and-forth and reduce buyer confidence.

Cultivate’s strategy is to win on **time-to-post** and **listing completeness** (without removing user control). We prioritize a **Human-in-the-Loop (HITL)** workflow where AI generates the basic fields, and the farmer acts as an evaluator before publishing—freeing attention for the rest of the selling process (responding to bounties, negotiating logistics, and planning future crops).

This sprint therefore prioritizes two core friction reducers:
- **Friction 1: Listing creation & “business language”** → Photo → Draft listing (HITL) + Price hints
- **Friction 2: Negotiation & coordination overhead** → Chat thread + Copilot autocomplete (HITL)

We treat the end-to-end market workflow as an epic, but we strategically build only the minimum scaffolding needed to validate value around those friction points.

---

## 2 Implementation timeline (this sprint vs deferred)
### Built / Focused This Sprint (P0)
- **Auto-Lister (Photo → Listing Draft, HITL)**
- **Price hint suggestion (StatCan reference range)**
- **Chat thread tied to bounty (“Interested” opens thread)**
- **Chat Copilot autocomplete/draft (HITL) + deterministic fallback**

### Minimum scaffolding to support P0 validation (P0/P1 as needed)
- Minimal bounty view + “Interested” CTA (to trigger chat)
- Basic message persistence (thread history)

### Deferred to future sprints (P2)
- Fit Score
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
- **CUJ step(s):** Farmer creates supply listing (Supply creation)
- **User problem:** Farmers avoid lengthy manual typing; manual listings become incomplete/unstructured (Competitive Review vs Facebook Marketplace).
- **Hypothesis:** Auto-generating item/title/description from a photo (with farmer review/edit) reduces time-to-post and increases listing completeness.
- **Success metric:** Median time-to-post listing decreases vs manual baseline; required fields are populated before publish (even after edits).
- **Priority rationale:** This is a core differentiator vs Facebook Marketplace and directly targets the largest creation friction.
- **Build decision:** **Build this sprint (P0).**

#### F2 — Price hint suggestion (StatCan reference range)
- **CUJ step(s):** Farmer finalizes listing fields before publish
- **User problem:** Farmers may be unsure about pricing; inconsistent pricing increases negotiation friction and reduces trust.
- **Hypothesis:** Showing a reference range and suggested price (from StatCan averages) reduces uncertainty and accelerates posting and negotiation.
- **Success metric:** Reduced edits to price field; fewer clarification messages about price; faster time-to-publish.
- **Priority rationale:** Low implementation cost; complements Auto-Lister by reducing another high-friction input field.
- **Build decision:** **Build this sprint (P0).**

#### F3 — Chat thread opens on “Interested” (bounty-driven inquiry)
- **CUJ step(s):** Farmer engages with bounty; coordination begins
- **User problem:** Negotiation needs context; messaging across platforms loses details and creates repeated clarifications.
- **Hypothesis:** Opening a thread tied to the bounty when the farmer clicks “Interested” keeps logistics/specs conversations in one place and increases conversion to offers.
- **Success metric:** Users can complete inquiry without leaving the product; fewer “where are we meeting/what specs?” loops.
- **Priority rationale:** Required to validate the core coordination friction hypothesis and makes Copilot meaningful.
- **Build decision:** **Build this sprint (P0).**

#### F4 — Chat Copilot autocomplete/draft (HITL) + deterministic fallback
- **CUJ step(s):** In-thread negotiation/confirmation
- **User problem:** “Professional writing” and repetitive confirmations create friction; farmers may feel mismatched to restaurant procurement language.
- **Hypothesis:** Copilot reduces drafting time and increases clarity by suggesting professional completions; user remains editor/approver.
- **Success metric:** Time-to-send decreases; fewer message rewrites; fewer missing-info clarifications.
- **Priority rationale:** Directly targets the second major friction point; aligns with competitive positioning (“professional wrapper” vs informal channels).
- **Build decision:** **Build this sprint (P0).**
- **Reliability note:** We are experimenting with multiple LLM APIs; for demo reliability, we maintain a **rule-based template fallback** if the LLM layer is unstable.

---

### P0/P1 — Enabling scaffolding (unblocks CUJ steps needed to test P0)
#### F5 — Minimal bounty view
- **CUJ step(s):** Restaurant posts demand; Farmer discovers bounty; Farmer initiates inquiry
- **User problem:** Without a bounty surface, we cannot validate the chat-first inquiry loop.
- **Hypothesis:** A minimal bounty card/page is sufficient to trigger the coordination workflow.
- **Success metric:** Farmer can open bounty and start a thread in ≤2 clicks.
- **Priority rationale:** Necessary to test P0 chat features; keep minimal to avoid bloat.
- **Build decision:** **Build this sprint (P0 scaffolding).**

#### F6 — Message persistence (thread history)
- **CUJ step(s):** Coordination within thread
- **User problem:** Without persistence, the workflow is not reproducible and decisions aren’t logged.
- **Hypothesis:** Persisted thread history reduces repeated clarifications and supports demo reliability.
- **Success metric:** Messages render reliably; thread context available for Copilot.
- **Priority rationale:** Required for Copilot context and for defensible CUJ completion.
- **Build decision:** **Build this sprint (P0 scaffolding).**

---

### P1 — Improves workflow credibility but not required to validate this week’s core bet
#### F7 — Map discovery (bounties/listings)
- **CUJ step(s):** Discovery stage
- **User problem:** Finding local demand/supply is time-consuming.
- **Hypothesis:** Map filters reduce time-to-find relevant opportunities.
- **Success metric:** Lower discovery time; fewer irrelevant clicks.
- **Priority rationale:** Valuable, but we can validate Auto-Lister and Chat-first loop using seeded bounties/listings during demo.
- **Build decision:** **If capacity remains (P1).**

---

### P2 — Deferred (Focus for future sprints; high value but not required for this sprint’s validation)

#### F9 — Fit score (full version)
- **CUJ step(s):** Offer evaluation
- **User problem:** Restaurants need consistent comparisons; farmers need feasibility feedback.
- **Metric:** Reduced mismatches; faster acceptance.
- **Build decision:** **Defer (P2)** for this sprint focus.

#### F10 — Filled vs remaining tracking (full version)
- **CUJ step(s):** Multi-supplier fulfillment
- **User problem:** Managing partial fulfillment across farms is heavy.
- **Metric:** Reduced admin overhead; fewer coordination loops.
- **Build decision:** **Defer (P2)** for this sprint focus.

#### F11 — Ag Guide (AI agent: evaluator + tiered guidance)
- **CUJ step(s):** Capability-building (future supply expansion)
- **User problem:** Farmers need guidance to scale or switch crops; buyers want long-run supplier depth.
- **Risk:** Tool-using agent increases hallucination/grounding risks and is hard to validate in one sprint.
- **Build decision:** **Defer (P2)**; MVP uses static tiered checklists/curated links if needed.

### P3 — Out of Scope for MVP
#### F12 — ESG validation at registration
- **CUJ step(s):** Trust/credibility
- **Build decision:** **Defer (P2)** (out of MVP scope currently)

#### F13 — Escrow payments
- **CUJ step(s):** Payment completion
- **Build decision:** **Defer (P2)** (out of MVP scope currently)