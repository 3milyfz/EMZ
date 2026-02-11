# Demo1 — Proof of Concept

## What we are validating (scope)

This proof of concept validates a **decision-to-action sourcing loop** for specialty ingredients:

1) A buyer (Viggy, Tocha Foods founder) **creates a Demand listing** for a specialty ingredient with constraints (quality baseline, expected cadence, pricing intent).  
2) A supplier (producer) **responds** to that listing with an offer.  
3) The buyer **matches** one response and **marks the request fulfilled**.

## What “working” means

The POC is considered working only if all of the following happen in the deployed product:

### 1. Authentication + onboarding gate correctly routes users
- A user can authenticate through the external auth flow and return to the app.
- A first-time user completes role registration (Buyer/Producer) and is then routed to the marketplace.
- A returning registered user lands in the marketplace without being re-onboarded.

**Why this matters:** The CUJ must start from a realistic identity state (not a hardcoded user), and role-derived behavior must be deterministic.

### 2. Demand listing creation persists and is retrievable
- The buyer can create a listing through the UI (form submission triggers a real backend mutation).
- After submission, the listing appears in the listings feed and is viewable on a detail route (fetch by ID).
- Refreshing the page does not erase the listing (no in-memory-only state).

**Why this matters:** This demonstrates real data-plane correctness (create → read) rather than UI-only prototyping.

### 3. Response → match → fulfill transitions persist server-side
- A non-owner can submit a response on an open listing (real write, not simulated).
- The listing owner can match one response (state transition persisted).
- The listing owner can mark the listing fulfilled (state transition persisted).
- The UI reflects status changes immediately after the API call, and those changes survive refresh.

**Why this matters:** This validates the marketplace “transaction primitive” and the minimum state machine needed for repeated sourcing.

## Architectural clarity (decoupling we enforce now)

We enforce a three-tier boundary that supports future decomposition:

- **Presentation tier (UI):** React pages for Home, Register, Listings, New Listing, Listing Detail. UI handles navigation, form UX, and rendering states.
- **Application tier (API contracts):** All reads/writes go through stable API calls. The UI depends on typed shapes for Listing/Response/User and does not reach into storage directly.
- **Data tier (persistence):** Listings/responses/users are stored with durable IDs. The system relies on persisted state for routing, ownership checks, and status transitions.

**Why this matters:** Later features (supplier profiles, reliability scoring, search/ranking, notifications) can be added as separate services or endpoints without rewriting the UI, as long as contracts remain stable.

## Known technical constraints / accepted limitations (technical honesty)

We accept these limitations for Demo 1 and document them explicitly:

1) **Decision-grade reliability signals are under-captured.** The current response model may not yet collect lead time, MOQ, recurring capacity, and delivery windows which are critical for Viggy’s “consistency first” decision-making.  
2) **No supplier credibility history view.** There is no profile surface for historical listings or deal history (e.g. completed deals), which limits trust for direct sourcing.  
3) **Units may be fixed (lb).** Quantity and price units are not yet selectable, increasing conversion errors for specialty procurement.   
4) **Discovery is minimal.** No ranking by distance, no specialty tags, no pagination, and limited filters.