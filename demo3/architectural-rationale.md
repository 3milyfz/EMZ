# Architectural Rationale: Glean Pivot Design Decisions

*Aligned with [pivot-contract.md](./pivot-contract.md). Glean implements the chat-first farm agent under test.*

## What Changed

The Glean pivot introduced a chat-first farm agent layer alongside the existing marketplace. Principal changes:

**1. New `/api/glean` subsystem** — GleanChat and GleanCart models enable persistent, role-scoped chat and cart state. Chats are user- and role-scoped (farmer vs restaurant), not tied to a listing or response.

**2. Dual chat systems** — Legacy `/api/chat` (ChatThread) remains for listing- and response-scoped farmer–restaurant negotiations. Glean chat is for agent-assisted discovery and listing creation. We kept them separate: different lifecycles, participants, and data models.

**3. Server-side Glean agent** — The agent runs in `gleanAgent.ts` on the backend. For restaurants, it searches real Listings via fuzzy match (produce taxonomy) and returns actual supply items; an optional LLM generates varied intro text. For farmers, it either (a) uses LLM to extract a draft from text, or (b) merges Azure Vision draft (title, item, description) with LLM-extracted price, qty, and delivery window from text when an image is uploaded. Fallback when the LLM is unavailable: regex-based draft (farmer) or empty product grid (restaurant).

**4. Cart and checkout** — GleanCart stores cart state per (user, chat). The UI supports add-to-cart and checkout flow, but checkout is mock-only; no orders or Listing responses are created yet.

## Why It Was Necessary

- **Differentiation:** "Chat with Glean to source and sell" positions Cultivate as a chat-first farm commerce agent, not just browse-and-respond.
- **Persistence:** Chat and cart survive reloads without modifying Listing or ChatThread.
- **Role-based UX:** The `ensure` endpoint provides a default chat per (user, role), streamlining onboarding for farmers and restaurants.

## Alternatives Considered

- **Extend ChatThread for agent use:** Rejected—ChatThread is listing/response-bound; agent conversations are pre-listing or cross-listing.
- **Client-only agent:** Rejected—server-side agent enables real listing search, LLM integration, and image processing without exposing keys.
- **Single cart per user:** Rejected—cart-per-chat lets users explore different searches without overwriting each other.

## Technical Debt Introduced or Resolved

**Introduced:** Glean routes are not in the generated OpenAPI/SDK; the app uses raw `fetch`. Farmer drafts use `latLng: [0, 0]` as placeholder. Client `fallbackAgentResponse` duplicates minimal logic for offline/error cases.

**Resolved:** Suggestions toggle and chat persistence reduce empty-state friction. Cart persistence avoids loss on navigation or reload.

## Limitations for Future Sprints

1. **Mock checkout** — Cart items (including real listing IDs for restaurants) do not create orders or responses.
2. **LLM optional for farmer** — Without an API key, farmer drafts rely on regex fallback; quality is low.
3. **Glean outside SDK** — Typed client would improve maintainability.
4. **No `inventoryConstraints` UI** — Backend supports filters; UI does not expose them.

Addressing checkout and SDK coverage will materially close the Glean pivot gaps.

---

*Summary: Glean implements the chat-first farm agent with server-side LLM, real listing search, persisted chat and cart, image-to-draft in chat, and voice-to-text input — supporting 3 of 4 pivot contract core workflows. Dual chat systems (ChatThread vs Glean) are intentional. Remaining work: real order creation (4th workflow), SDK parity, and optional LLM fallback improvements.*
