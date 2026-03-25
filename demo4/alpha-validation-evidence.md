# Demo 4 – Alpha validation evidence

This document provides evidence that the system meets alpha criteria across: **feature completeness**, **internal stability**, **test repeatability**, and functional **security / operations / reliability controls**.

---

## Locked feature set (Demo 4)

### Restaurants
- **CSV demand upload** so buyers can submit bulk demand without manual form entry
- **Automatic demand extraction** from the uploaded file into structured line items
- **Optimization-assisted shopping** returning three sourcing options:
  - **Lowest Cost**
  - **Best Match**
  - **Balanced Match**
- **Human-in-the-loop cart review at checkout** so buyers verify the final order

### Farmers
- **Image-based listing creation** starting from a produce photo (assisted draft flow)
- **LLM-generated title + description** to reduce writing burden
- **Speech-to-text support** to reduce heavy typing
- **Premium dynamic pricing** with daily market-based price updates

---

## Feature completeness (core workflows run end-to-end)

### Glean multi-chat (restaurant/farmer) is implemented end-to-end
- **Backend chat session CRUD** exists and is user-scoped:
  - `GET /api/glean/chats` lists sessions for the authenticated user.
  - `POST /api/glean/chats` creates a new session.
  - `POST /api/glean/chats/ensure` returns an existing or default chat per user+role.
  - `PATCH /api/glean/chats/:id` renames a chat (title capped).
  - `DELETE /api/glean/chats/:id` deletes a chat and also deletes its persisted cart.
  - Evidence: `pkgs/server/src/routes/glean.ts` (chat routes + ownership enforcement via `{ _id: id, user: userId }` queries).
- **Frontend active chat persistence** exists (reduces context loss / navigation resets):
  - The active chat is stored per role in `sessionStorage` under `glean:activeChatId:<role>`.
  - Evidence: `pkgs/app/src/features/agent-sourcing/hooks/useGleanChats.ts`.

### Chat can store structured payloads, not just plain text
- Messages support typed payloads (`text`, `product_grid`, `inventory_form`, `strategy_options`) plus structured fields (`items`, `draft`, `options`, `sourcingPlan`) and optional `imageId`.
- Evidence: `AppendMessageBody` schema and persistence in `pkgs/server/src/routes/glean.ts`.

### Sourcing optimization is implemented as a first-class API surface
- **Optimizer endpoint**:
  - `POST /api/sourcing/optimize` accepts either `orderDescription` (natural language) or structured `lineItems`, with constraints.
  - Validates request shape with Valibot and returns `400` with issue details for invalid inputs.
  - Evidence: `pkgs/server/src/routes/optimization.ts`.
- **CSV sheet parsing**:
  - `POST /api/sourcing/parse-sheet` parses uploaded CSV into normalized `lineItems`.
  - Includes header normalization + column detection (`item`, `qty`, etc.), strict file-type acceptance (CSV only), and informative `400/415` errors.
  - Evidence: `pkgs/server/src/routes/optimization.ts`.
- **Three sourcing options**:
  - Chat messages and payload schemas support a `strategy_options` message type and a structured `sourcingPlan`, enabling the UI to present a ranked set of options (mapped in product language to **Lowest Cost / Best Match / Balanced Match**) and keep a human in the loop at checkout.
  - Evidence: `type: "strategy_options"` + `options` + `sourcingPlan` fields in `AppendMessageBody` in `pkgs/server/src/routes/glean.ts`.

### Upload pipeline exists (enables image-driven flows)
- **Authenticated image upload**:
  - `POST /api/images/upload` accepts multipart form-data, validates mime-type, processes the image (rotate/resize), stores in GridFS, and returns an `imageId`.
  - Evidence: `pkgs/server/src/routes/images.ts`, `pkgs/server/src/services/gridfs.ts`.
- **Serving stored images**:
  - `GET /api/images/:id` returns cached image binary and `404` when not found.
  - Evidence: `pkgs/server/src/routes/images.ts`.

### Speech-to-text support exists in the chat input
- The chat input implements optional Web Speech API voice capture (including `webkitSpeechRecognition` fallback) and writes the transcript into the message draft.
- Evidence: `getSpeechRecognition()` + voice toggle logic in `pkgs/app/src/components/ui/multimodal-ai-chat-input.tsx`.

### Registration + account settings include location capture (postal + geocode)
- User registration includes `postalCode`, which is geocoded and persisted with `latLng`.
- Updating the user’s `postalCode` re-geocodes and propagates the location onto the user’s listings via `Listing.updateMany`.
- Evidence: `pkgs/server/src/routes/users.ts`.

---

## Internal stability (validation, predictable errors, safer defaults)

### Input validation is enforced on critical surfaces
- Optimizer: Valibot schemas enforce presence/shape; returns structured `details` for invalid requests.
  - Evidence: `OptimizeBody` and `safeParse` handling in `pkgs/server/src/routes/optimization.ts`.
- Glean: message payload schemas enforce the allowed message types and required fields.
  - Evidence: `AppendMessageBody` in `pkgs/server/src/routes/glean.ts`.
- Postal codes: canonical normalization and validation rules exist as a reusable schema.
  - Evidence: `normalizeCanadianPostal` + `CanadianPostalSchema` in `pkgs/server/src/utils/canadianPostal.ts`.

### Error handling is explicit and consistent at the app layer
- Unhandled errors return JSON with an error message and **re-apply CORS headers** (prevents “opaque” browser failures).
- Evidence: `app.onError` in `pkgs/server/src/app.ts`.

### Failure modes are anticipated in upload flows
- Upload route handles:
  - malformed multipart bodies (`400`)
  - missing file field (`400`)
  - unsupported formats (`415`)
  - sharp decoding edge cases (HEIF / invalid buffers) mapped to `415`
- Evidence: `pkgs/server/src/routes/images.ts`.

---

## Test repeatability (manual tests are deterministic and re-runnable)

The following test cases can be run repeatedly against a local dev stack (or deployed env) with deterministic pass/fail outcomes because they rely on explicit validation rules and stable endpoints.

### API-level repeatable test plan (smoke)
- **Health**:
  - `GET /health` returns `{ healthy: true, time, authenticated }` and optionally `auth0Id` when a bearer token is present.
  - Evidence: `pkgs/server/src/app.ts`.
- **Chat sessions** (requires auth):
  - Create → list → rename → delete, verifying 404s for wrong IDs and ownership enforcement.
  - Evidence: `pkgs/server/src/routes/glean.ts`.
- **Optimizer**:
  - Send invalid bodies and confirm `400` + issue details; send valid bodies and confirm `200` response.
  - Evidence: `pkgs/server/src/routes/optimization.ts`.
- **CSV parse**:
  - Upload `.csv` with columns `item, qty` and confirm parsed `lineItems`.
  - Upload non-CSV (or missing field) and confirm `415/400` with useful error messages.
  - Evidence: `pkgs/server/src/routes/optimization.ts`.
- **Image upload**:
  - Upload JPEG/PNG/WEBP and verify `201` and returned `imageId`.
  - Upload unsupported format and verify `415`.
  - Evidence: `pkgs/server/src/routes/images.ts`.

---

## Security controls (functional, not placeholder)

### Auth boundaries are explicit
- Sensitive routes are protected with `authMiddleware()`:
  - chat CRUD + message append + cart persistence
  - image upload
  - user registration completion + profile update
  - Evidence: route definitions in `pkgs/server/src/routes/glean.ts`, `pkgs/server/src/routes/images.ts`, `pkgs/server/src/routes/users.ts`.
- Where auth is optional, behavior is constrained:
  - endpoints using `authMiddleware({ optional: true })` still validate inputs and return `401` when a user-only capability is requested.
  - Example: using `imageId` to generate an authenticated-only draft is rejected without a user session.
  - Evidence: auth gate in `POST /api/glean/agent` in `pkgs/server/src/routes/glean.ts`.

---

## Operations visibility + reliability controls (minimum viable but real)

### Logging exists for operational events
- Image upload emits a structured log event with `userId`, `imageId`, dimensions, and byte size.
- Evidence: `logJson("image_upload", ...)` in `pkgs/server/src/routes/images.ts`.

### Background scheduler is wired and safe to re-run
- Server startup triggers a daily price updater scheduler.
- The updater is designed to be **idempotent for a given date** and logs a completion summary including error count.
- Evidence: `startDailyPriceScheduler()` call in `pkgs/server/src/index.ts`; implementation in `pkgs/server/src/services/dailyPriceUpdater.ts`.

### Data-layer reliability for uploads
- GridFS helpers explicitly fail if MongoDB is not ready (no silent corruption), and upload/download are promise-wrapped with stream error handling.
- Evidence: `pkgs/server/src/services/gridfs.ts`.

---

## Conclusion (alpha criteria status)

Based on the concrete behaviors implemented since Mar 19 (multi-chat + persistence, structured chat payloads, optimizer endpoints + CSV parsing, authenticated upload pipeline, postal/geocode at registration, app-level error handling + health checks, and minimum operational logging + scheduler wiring), the system demonstrates:
- **Feature completeness** for the primary “chat → suggestions/plan → persistence” workflows.
- **Internal stability** via schema validation, predictable errors, and guarded failure modes.
- **Repeatable testing** via deterministic endpoint behavior and explicit error contracts.
- **Functional security/ops/reliability controls** appropriate for alpha (auth boundaries, health visibility, structured logs, scheduled maintenance work).

