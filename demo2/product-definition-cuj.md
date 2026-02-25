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
3) Farmer can submit an offer (structured commitment) to a bounty.
4) Submitting an offer creates a chat thread; both sides can coordinate inside the product (copilot assists but user approves).
5) Restaurant can review responses/offers and open chat via a message icon; workflow reaches a committed outcome (at minimum matched status for demo).
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

### Phase 3 — Commitment first (Offer submission)
**Step 3.1 Farmer views a bounty**
- Farmer opens bounty details and decides whether they can fulfill.

**Step 3.2 Farmer submits a structured offer**
- Offer fields: qty (partial allowed), price, timing, logistics notes.

**Step 3.3 Fit score shown (future/full)**
- Fit Tier + “why” explanation supports feasibility checks and comparability (deferred if needed).

---

### Phase 4 — Coordination after commitment (Chat created on offer)
**Step 4.1 Offer submission creates a chat thread**
- A thread is created and tied to the offer/bounty context.

**Step 4.2 Restaurant reviews responses and opens chat via message icon**
- Each response/offer has a **message icon**; clicking it opens the thread.

**Step 4.3 Copilot assists communication (HITL)**
- Autocomplete/draft suggested replies; user approves/edits.

---

### Phase 5 — Matching and completion
**Step 5.1 Restaurant accepts offer(s)**
- Updates filled vs remaining; supports multi-farm fill (future/full).

**Step 5.2 Payment**
- Future: escrow.

**Step 5.3 Status + history**
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

### Offer submission (structured commitment)
- Value: converts interest into a concrete response; reduces ambiguity and enables later comparability.

### Chat created on offer + message icon on responses
- Value: ensures coordination happens only when a transaction is plausible; reduces spam/low-signal chats and keeps context anchored to an offer.

### Chat + Copilot autocomplete (HITL)
- Value: reduces “professional writing” friction; speeds confirmations/substitutions.

### Fit score + explanation (future/full)
- Value: reduces over-commitment and improves restaurant decision speed.

### Filled vs remaining tracking (future/full)
- Value: makes partial fulfillment manageable; reduces admin burden.

### Ag Guide (tiered guidance; future agent)
- Value: reduces “how do I meet spec?” friction; grows supply depth over time.

### ESG validation at registration (future)
- Value: reduces greenwashing risk and increases credibility.

### Escrow (future)
- Value: reduces payment dispute/non-payment risk.

---

## 7 This sprint focus (what we validated)
This sprint prioritizes validating proof-of-value around:
- **Creation friction reduction** (Auto-Lister + Price hints)
- **Coordination friction reduction** (Chat thread created on offer + Copilot autocomplete)
while keeping other CUJ steps minimally scaffolded to support end-to-end demonstration.