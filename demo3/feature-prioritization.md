# Feature Prioritization: Glean Pivot

*Aligned with [pivot-contract.md](./pivot-contract.md). The kill metric requires **3 of 4 core workflows** to work end-to-end in the chat-first interface.*

## Pivot Contract Core Workflows → Current Status

| Core Workflow | Implementation | Status | Notes |
|---------------|----------------|--------|-------|
| Produce shopping through chat | Restaurant: natural-language search → product grid | ✓ | Real listings via `fetchListingMatches`; stays in chat. |
| Listing creation through chat | Farmer: image + text → draft → Post | ✓ | Azure CV + LLM; image upload in chat; confirm/edit before publish. |
| Voice-to-text input into agent flow | Web Speech API in `MultimodalInput` | ✓ | Voice button in Glean chat; transcript flows to agent. |
| Quick checkout progression | Add-to-cart → Checkout UI | ⚠ Partial | Cart + checkout UX complete; **mock only** — no real order creation. |

**Kill metric assessment:** 3 of 4 workflows meet the falsifiable threshold. Checkout does not (no usable result — mock order). Pivot currently meets the kill metric; addressing checkout strengthens the demo and reduces reliance on a single margin.

---

## Gap Analysis with Quantified Performance Ratios

| Capability | Target | Current Ratio | Notes |
|------------|--------|---------------|-------|
| Restaurant search → real listings | 100% | **100%** | `fetchListingMatches` queries MongoDB; fuzzy match via produce taxonomy. |
| Farmer text → draft listing | 100% | **~85%** (LLM) / **~40%** (fallback) | With LLM key: high accuracy. Without: regex fallback (carrots, tomatoes, etc.). |
| Farmer image → draft listing | 100% | **100%** (Azure + LLM) | `getDraftSuggestedFieldsFromImage` + text merge; image-to-draft IS in Glean. |
| Voice-to-text → agent input | 100% | **100%** | Web Speech API; transcript sent to agent; stays in chat. |
| Glean cart persistence | 100% | **100%** | GleanCart per (user, chat); load/save via API. |
| Checkout → real order | 100% | **0%** | Mock flow; no order/response creation. |
| Glean routes in OpenAPI/SDK | 100% | **0%** | App uses raw `fetch`; SDK has Listings, Users, Default only. |
| Intro text variety (restaurant) | High | **~90%** | LLM varies phrasing; fallback is static. |
| Multi-chat per role | Optional | **Partial** | `ensure` favors one chat/role; list/create exist but UX downplays. |

---

## Feature Parity Assessment

### Glean vs. Legacy Marketplace

| Feature | Legacy (Listings + ChatThread) | Glean | Parity |
|---------|-------------------------------|-------|--------|
| Browse supply/demand listings | ✓ | ✓ (via agent product grid) | **Full** |
| Create supply listing | ✓ (New Listing) | ✓ (inventory_form → Post) | **Full** |
| Create demand listing | ✓ | ✓ (if create intent detected) | **Full** |
| Farmer–restaurant negotiation | ✓ (ChatThread per listing+response) | ✗ | **Gap** |
| Image-to-draft | ✓ (New Listing) | ✓ (chat attachment) | **Full** |
| Cart + checkout | ✗ | ✓ (UI + GleanCart) | **Glean ahead** |
| Chat persistence | ✓ (ChatThread) | ✓ (GleanChat) | **Full** |
| Natural-language discovery | ✗ | ✓ | **Glean ahead** |
| Real order from cart | N/A | ✗ | **Gap** |

---

## Critical vs. Acceptable Gaps

### Critical Gaps (Must address for kill metric / production)

1. **No real order creation** — Checkout is mock. Cart items (including real `listingId`s) do not create responses or orders. This is the **4th core workflow** in the pivot contract; without it, the product cannot credibly claim "quick checkout progression" for the demo.
2. **Glean routes outside SDK** — Raw `fetch` increases maintenance cost and type drift; onboarding and tooling suffer.
3. **LLM optional for farmer** — Without `GROQ_API_KEY`/`OPENROUTER_API_KEY`/`OPENAI_API_KEY`, farmer drafts are regex-based and low quality.

### Acceptable Gaps (Can defer)

1. **Single-chat-per-role UX** — `ensure` gives one default chat per role; multi-chat exists but is secondary. Acceptable for MVP.
2. **Static fallback when agent fails** — Client `fallbackAgentResponse` only triggers on network/API errors; rare in normal use.
3. **Geolocation placeholder** — Farmer drafts use `latLng: [0, 0]`; listings can be edited before post. Low impact for initial rollout.
4. **No `inventoryConstraints` in UI** — Backend supports `maxPricePerKg`, `preferredUnits`, etc.; UI does not expose. Nice-to-have.

---

## Strategic Feature Decisions with Justifications

| Decision | Choice | Justification |
|----------|--------|---------------|
| Dual chat systems | Keep ChatThread + GleanChat separate | ChatThread = listing/response-scoped negotiation (farmer–restaurant). Glean = user/role-scoped discovery + listing creation. Different lifecycles and participation models; merging would overcomplicate both. |
| Cart per chat | GleanCart scoped by (user, chat) | Users can try different searches per conversation without overwriting carts. Aligns with “chat as workspace” mental model. |
| Restaurant gets real listings first | Listing search before LLM intro | Delivers concrete value even without LLM. LLM enhances intro and follow-up answers; search is the core. |
| Farmer image + text merge | Azure CV + LLM extraction | Image gives title/item/description; text gives price, qty, delivery window. Best of both; avoids duplicate image-only flow. |
| Defer Glean → OpenAPI | Keep raw fetch for now | Shipping Glean faster; OpenAPI/SDK update is a follow-up task. Trade-off: type safety vs. velocity. |
| Mock checkout UX | Full UI, no backend order | Validates UX and cart persistence; order creation is a separate integration (payments, fulfillment). Reduces scope of initial Glean release. |
| Fallback on agent failure | Client regex draft / empty grid | Graceful degradation when backend unavailable; user still sees a response. Prefer over hard failure. |

---

## Recommended Roadmap (by Priority)

*Priorities align with pivot contract kill metric and fallback options.*

1. **P0** — Order creation: map Glean cart items (with `listingId`) to Listing responses or a dedicated Order model. Required for the 4th core workflow and kill metric.
2. **P1** — Add Glean routes to OpenAPI; regenerate SDK; migrate app to typed client.
3. **P2** — Make LLM effectively required for farmer (or improve fallback) via configuration guardrails.
4. **P3** — Expose `inventoryConstraints` in restaurant chat UI.
5. **P4** — Enhance multi-chat UX (e.g., chat switcher, clear “new chat” affordance).
