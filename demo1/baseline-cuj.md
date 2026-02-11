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

---

## Journey Audit

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

---

## Highlights & Lowlights

### None / Great
| Area | Why it’s good | Keep / extend |
|---|---|---|
| Clear gating logic (Home → Register → Listings) | Prevents “half-authenticated” state confusion; deterministic navigation | Extend access to the listings to the public, only make the creation of listing and response of listing log-in users only |
| Listing Detail ownership logic | Owners see Edit/Delete; non-owners see Respond | Keep |

### Moderate friction
| Pain point | Where it shows up | Why it matters | Proposed fix (implementable) |
|---|---|---|---|
| Pricing mental model is too rigid for demand listings | New Listing form frames price as required “price per unit (per lb)” even when the buyer is posting a demand | For Viggy, demand posts often start as a *target range* or “contact for quote,” especially for specialty ingredients. A mandatory fixed price can cause hesitation or inaccurate posts, reducing response quality. | Make price optional for demand listings and change copy to “Target price (optional)” or “Target price range (optional)”; support “Request quote” toggle that hides price field but keeps listing valid. |
| Units are fixed to lb for both price and quantity | New Listing form uses a single implicit unit (lb) for qty and price | Specialty sourcing often uses different units (kg, case, pallet, bottle, L, gal). A fixed unit forces mental conversion and increases posting errors, which hurts supplier trust and makes comparisons unreliable. | Add a unit selector for **quantity** and **price unit** (e.g., lb/kg/case/L/gal). Store as `qtyValue + qtyUnit` and `priceValue + priceUnit`. Display units consistently in listing cards and responses. Default to lb for speed but allow override. |


### Severe friction
| Pain point | Why it causes real business risk for Viggy | Proposed fix |
|---|---|---|
| Missing fields to judge consistency/reliability | Viggy’s #1 criterion is reliability; without capturing lead time, minimum order quantity, capacity, and recurring availability, responses are not decision-grade and increase the risk of missed production runs | Add structured fields to Response: `lead_time_days`, `moq`, `recurring_capacity`, `delivery_window` (and optional `certs/handling_notes`). Render these fields in a side-by-side comparison layout for responses. |
| No supplier credibility/history view (no historical listings / Completed deals) | Direct sourcing requires trust, but reliability can’t be fully vetted upfront. Without visibility into a supplier’s past activity (completed matches, fulfillment rate, recent listings), Viggy can’t de-risk onboarding and may avoid engaging altogether | Add a supplier profile page linked from responses (e.g., `/users/:id`): show historical listings, deal history (matched/fulfilled count), fulfillment rate, average lead time (if tracked), and optional references. Start with minimal stats (counts + recent activity) to avoid overbuilding. |


---
## Product recommendations (next iteration)

1) **Decision-grade reliability fields in responses (fix Severe friction: user viability for Viggy)**  
   - Change: Extend Response with structured fields: `lead_time_days`, `moq`, `recurring_capacity`, `delivery_window`. Display these in a **side-by-side response comparison** layout.  
   - Why it reduces friction Reliability is Viggy’s #1 criterion; without these, matching is not decision-grade and risks production disruption.  
   - Acceptance criteria: A buyer can evaluate at least one response without off-platform follow-up because it includes the minimum reliability information.
2) **Supplier credibility/profile view (fix Severe friction: trust + de-risk onboarding)**  
   - Change: Add a lightweight supplier profile page linked from responses (e.g., `/users/:id`) showing: recent listings, **deal history (matched/fulfilled count)**, fulfillment rate, and (if tracked) average lead time.  
   - Why it reduces friction: Direct sourcing requires trust, but reliability is validated over time. Without history, buyers hesitate to onboard new suppliers.  
   - Acceptance criteria: From any response, the buyer can view supplier history in ≤2 clicks and use it to inform matching decisions.

3) **Unit selection for quantity + price (reduce conversion errors + improve comparability)**  
   - Change: Add unit selectors for **quantity unit** and **price unit** (e.g., `lb`, `kg`, `case`, `L`, `gal`). Persist as structured fields: `qtyValue + qtyUnit`, `priceValue + priceUnit`. Render units consistently on listing cards, detail pages, and responses.  
   - Why it reduces friction Specialty ingredients are commonly negotiated in multiple units; a fixed unit forces mental conversion and increases posting mistakes, undermining trust and making responses hard to compare.  
   - Acceptance criteria: Users can create listings and responses using non-lb units; all views display the chosen units consistently.
4) **Flexible demand pricing (fix rigid pricing mental model in Steps 5–6)**  
   - Change: For *demand* listings, make price optional and update label to **“Target price (optional)”** or **“Target price range (optional)”**. Add a **“Request quote”** toggle that hides the price input but keeps submission valid.  
   - Why it reduces friction Specialty sourcing often begins with a target band or quote request. Forcing a fixed “price per lb” increases hesitation and leads to inaccurate posts, which reduces supplier response quality.  
   - Acceptance criteria: A demand listing can be created with (a) no price, (b) a single target price, or (c) a price range; suppliers still understand the intent.
  
5) **Public browsing, gated actions (improve supplier acquisition)**  
   - Change: Make `/listings` and `/listings/:id` publicly viewable (no login required). Require authentication only for **Create Listing**, **Respond**, **Match/Fulfill**, and **Edit/Delete**.  
   - Why it reduces friction Viggy (and suppliers) can quickly assess market relevance before committing to auth. It also makes listings shareable (email intros, trade shows, referrals), which is critical for reaching small suppliers who won’t create accounts just to “peek.”  
   - Acceptance criteria: Unauthenticated users can browse listings and open detail pages; attempts to post/respond trigger an auth CTA and do not mutate data.

---
## Quantitative success metrics (Go/No-Go validation)

### 1.Time thresholds
- **T_post (Steps 4–8):** Create + submit a demand listing and open its detail page in **≤ 150 seconds**.  
  - *Target after iteration:* **≤ 120 seconds**
- **T_eval (Step 9):** Evaluate responses and decide on a match in **≤ 60 seconds**, once responses exist.
- **T_discover (public-browse improvement):** Landing → viewing a relevant listing detail page in **≤ 20 seconds** (no login).

### 2. Friction severity limits
- **Severe friction:** **0 allowed** in Steps 4–8 (posting + viewing must never hard-stop).
- **Moderate friction:** **≤ 3 total** across the full CUJ.

### 3. Context switch maximum
- **≤ 2 total** (Auth0 redirect counts as 1; everything else should remain in-product).

### 4. Decision-grade response requirement (user viability)
To count the CUJ as validated for Viggy’s supplier selection workflow:
- At least **one** supplier response must include:  
  **price (or quote intent) + qty/capacity + lead time/availability**.  
If responses lack these fields, the system may be technically functional but **not viable** for real supplier selection.

### 5. Security 
- **0 unauthorized mutations:** Unauthenticated users must not be able to create/edit listings, post responses, match, or fulfill.
