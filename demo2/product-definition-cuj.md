# Product Definition CUJ — Cultivate (Demo2)
*(Complete end-to-end journey + feature-to-value mapping + Competitive Review CUJ vs Facebook Marketplace)*

## 1 Personas
### Persona A — Self-sufficient Farmers
Self-sufficient growers with surplus/specialty produce who want to sell before spoilage, reduce listing/admin burden, and coordinate quickly with buyers.

### Persona B — Family-owned Restaurants & Food Startups
Buyers sourcing specialty ingredients who want transparent local supply, reduced distributor cost/opacity, and faster coordination with credible sourcing signals.

## 2 Value-driven goal
Enable restaurants to express specialty demand and coordinate local sourcing efficiently, while enabling farmers to participate with low friction (fast listing + fast professional communication), reducing time-to-sale and sourcing overhead.

## 3 Competitive Review CUJ (benchmark vs alternative)
### Core question
**How do we compare to alternatives?**

### Competitor selected
**Facebook Marketplace** (most-used alternative by our target users per interviews).

### Controlled journey (same persona, same goal)
**Goal:** Farmer posts a listing to sell specialty produce and responds to a buyer inquiry.

We benchmarked:
- **Task completion time**
- **Friction points**
- **Feature availability**
- **Structured data quality**

### Key gap found (drives our prioritization)
Facebook Marketplace requires manual entry for item type/title/description, leading to incomplete/unstructured listings when users don’t invest time. Cultivate prioritizes **HITL generation** to auto-fill basic fields and keep farmers as evaluators before publishing, improving listing completeness and saving time for selling/coordination.

---

## 4 CUJ success criteria
A run is successful when:
1) Restaurant can post demand (bounty) with clear fields.
2) Farmer can post supply quickly (photo→draft) OR find a bounty.
3) Farmer can initiate inquiry (thread opens on “Interested”).
4) Chat supports quick clarification and decisions (copilot assists but user approves).
5) Workflow reaches a committed outcome (offer/accept in future, at minimum a clear matched status for demo).
6) Users can review what happened (thread history; order/history over time).

---

## 5 End-to-end Product Definition CUJ (all journey steps, not just this sprint)

### Phase 0 — Entry & onboarding
**Step 0.1 Role selection**
- Farmer or Restaurant.

**Step 0.2 Registration + profile**
- Restaurant: location, sourcing preferences.
- Farmer: farm basics, location.
- Future: ESG validation at registration.

---

### Phase 1 — Demand creation (Restaurant)
**Step 1.1 Create bounty**
- Fields: item, qty, timing, location, constraints/substitutions.

**Step 1.2 Bounty discoverable**
- Map/feed discovery, filters, distance visualization.

**Step 1.3 Ag guide panel on bounty (capability support)**
- Tiered guidance to help farmers interpret feasibility and next steps (future: AI agent).

---

### Phase 2 — Supply creation (Farmer)
**Step 2.1 Create supply listing**
- Photo → draft item/title/description (HITL) + farmer edits.
- Price hint reference suggested from StatCan (range + suggested).

**Step 2.2 Supply discoverable**
- Visible to restaurants via map/filters.

---

### Phase 3 — Interest → Coordination (Chat-first)
**Step 3.1 Farmer becomes interested in bounty**
- Farmer clicks “Interested” on bounty to start coordination.

**Step 3.2 Inquiry thread opens**
- Thread is tied to bounty; all details stay in-product.

**Step 3.3 Copilot assists communication (HITL)**
- Autocomplete/draft suggested replies; user approves/edits.

---

### Phase 4 — Commitment & matching (structured transaction)
**Step 4.1 Convert inquiry to structured offer**
- Offer fields: qty (partial allowed), price, timing, logistics.

**Step 4.2 Fit score shown**
- Fit Tier + “why” explanation supports feasibility checks and comparability.

**Step 4.3 Restaurant accepts offer(s)**
- Updates filled vs remaining; supports multi-farm fill.

---

### Phase 5 — Completion
**Step 5.1 Payment**
- Future: escrow.

**Step 5.2 Status + history**
- Matched/fulfilled/expired states; order history for repeat workflow.

---

## 6 Feature-to-value mapping (for every identified feature)
> This section enumerates all features implied by the CUJ and ties each to user value. (See `feature-prioritization.md` for sprint decisions.)

### Auto-Lister (Photo → Draft, HITL)
- Value: reduces listing friction and improves completeness vs Facebook Marketplace manual entry.

### Price hints (StatCan reference)
- Value: reduces pricing uncertainty, speeds posting, reduces negotiation overhead.

### Bounty creation
- Value: lets restaurants express specialty demand clearly.

### Map discovery
- Value: reduces search friction and supports local matching.

### Chat + Copilot autocomplete (HITL)
- Value: reduces “professional writing” friction; speeds confirmations/substitutions.

### Fit score + explanation
- Value: reduces over-commitment and improves restaurant decision speed.

### Filled vs remaining tracking
- Value: makes partial fulfillment manageable; reduces admin burden.

### Ag Guide (tiered guidance; future agent)
- Value: reduces “how do I meet spec?” friction; grows supply depth over time.

### ESG validation at registration
- Value: reduces greenwashing risk and increases credibility.

### Escrow
- Value: reduces payment dispute/non-payment risk.

---

## 7 This sprint focus (what we validated)
This sprint prioritizes validating proof-of-value around:
- **Creation friction reduction** (Auto-Lister + Price hints)
- **Coordination friction reduction** (Chat thread + Copilot autocomplete)
while keeping other CUJ steps minimally scaffolded to support end-to-end demonstration.