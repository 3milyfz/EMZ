# Baseline CUJ — Demo 1

## Persona (context + constraints)

**Persona:** Viggy — Founder/Operator at Tocha Foods (CPG food manufacturer scaling a “real ingredients” product ethos)  
**Core context:** Tocha differentiates by cooking with whole, real ingredients (not powders/preservatives). But North American retail economics squeeze margins: retailers often expect ~40% margin and distributors ~25%, so the manufacturer’s effective revenue per bottle can drop sharply—especially during promos (often 4×/year) that the manufacturer funds. This structural pressure pushes many brands toward cheaper processed inputs; Viggy’s strategy is to counteract that via operational efficiency and **cutting middlemen** through direct sourcing/vertical integration where feasible.

**Decision constraints (what actually governs supplier selection):**
- **Quality is gatekeeping:** if quality doesn’t meet baseline, supplier is immediately rejected.
- **Consistency & reliability dominate:** missed or inconsistent deliveries can break production runs and create larger downstream costs than slightly higher unit prices.
- **Cost matters, but in context:** a supplier must be *competitive vs distributor pricing*, especially on specialty ingredients where distributors often have less economy of scale.
- **Capacity planning lead time:** direct sourcing often requires committing **~a year in advance** (e.g., dedicated plots, proportions across pepper varieties).
- **Supplier vetting is experiential:** reliability can’t be fully known upfront; it’s validated through working relationship + first production runs.
- **ESG/CSR stance:** sustainability/inclusivity should be **permanent at scale**, not limited-edition “greenwashing.”

## Value-driven, measurable user goal

**Goal:** Identify and shortlist reliable specialty-ingredient suppliers (direct or small producers) that can meet repeat production-run needs without compromising Tocha’s “real ingredients” quality bar.
**Concrete outcome**: Viggy posts a specialty ingredient demand listing, receives ≥1 response with price + available quantity/capacity + lead time/availability notes, matches one supplier to proceed with a trial order, and marks the request fulfilled. 

**Definition of done (system-visible):**
- A Demand listing is created and viewable (persisted via API).

- Listing receives ≥1 response containing price + qty/capacity + lead time/availability notes.

- Viggy matches one response and updates listing status to fulfilled.


## Scope of this baseline CUJ

This baseline CUJ audits the end-to-end loop for **specialty ingredient sourcing**:
Demand listing (Viggy) → Supplier response (farmer/producer) → Match → Fulfill.
Out of scope: payments, contracts, delivery scheduling automation, long-term forecasting, and multi-order procurement workflows.


## Journey Audit (baseline)

| Step # | Action (what the user does) | Context Switches | Time (seconds) | Friction Severity | Evidence |
|---:|---|---|---:|---|---|
| 1 | Land on product, click Get started / Log in | Auth redirect (external) | 10s | Moderate | Home screenshot |
| 2 | Complete authentication | External auth flow | 10s | Moderate | Post-auth screenshot |
| 3 | First-time: register role (Producer/Buyer) and continue | None | 30s | Moderate | Register screenshot |
| 4 | Navigate to Listings and click “New Listing” | None | 3s | None/Great | Listings screenshot |
| 5 | Create **Demand** listing for specialty ingredient (e.g., “Yuzu juice (Canada)” / “Aji amarillo peppers”) | Mental context: units, spec, frequency, quality requirements | 1-2min | Moderate | New Listing form screenshot |
| 6 | Add constraints in description: quality baseline, required consistency, approximate run cadence, target price band (if applicable) | None | 15s | Great | Filled form screenshot |
| 7 | Submit listing | None | 3s | Great | Submission success/error screenshot |
| 8 | Verify listing appears and open listing detail | None | 3s | Great | Listing in list + detail screenshot |
| 9 | Review supplier responses: compare **capacity/reliability signals** + price + lead time | Mental context shift: procurement evaluation | 30s | Moderate  | Responses section screenshot |
| 10 | Match one supplier response (chosen supplier) | None | 3s | None | Matched state screenshot |
| 11 | Mark as fulfilled once a supplier is selected for next run | None | 3s | None | Fulfilled state screenshot |



