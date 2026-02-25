# Architectural Rationale — Demo 2

## 1 Primary architectural change: Azure Computer Vision tagging + “draft listing” pipeline
**What changed.** Compared to Demo 1’s manual listing creation, Demo 2 introduces a new **vision-assisted data pipeline** that turns an uploaded image into a structured listing draft. The new flow is:

**Frontend uploads image → backend**  
**Backend calls Azure Computer Vision (secret key on server)**  
**Backend returns structured draft `{ item, title, description, confidence }`**  
**Frontend renders draft → user edits → user submits listing**

This is an architectural change because it adds (a) a new cross-service dependency, (b) a new backend responsibility (orchestration + normalization), and (c) a new intermediate artifact (“draft listing”) that sits between raw input and persisted listing data.

**Why it was necessary.** The MVP goal is to reduce friction in creating high-quality listings. Users can start from an auto-generated draft instead of a blank form, while still retaining control (edit-before-post). Keeping the Computer Vision API key server-side also improves security by avoiding secret exposure in the client.

**Alternatives considered (Azure CV vs CLIP).**
- **Azure Computer Vision (chosen):** Tagging output aligns directly with our use case (interpretable labels), performed well on test images, and the free tier (~5,000 requests) is sufficient for demo/prototyping volume. Because it is hosted and managed, it is simpler to scale and deploy without adding GPU/model-serving infrastructure.
- **CLIP (not chosen for Demo 2):** Offers flexible embeddings for semantic similarity and retrieval, but would require additional engineering for model hosting (often GPU), batching/latency control, and threshold calibration. That overhead does not match the current MVP requirement (tags → draft fields) and increases scope/risk.

**Technical debt / limitations.** We introduce vendor dependency (tag schema, confidence behavior, quotas) and may need a fallback if the tagging misses domain-specific items. Future sprints should add a lightweight evaluation set (precision/recall on “must-detect” tags) and a `VisionProvider` abstraction to keep CLIP as a future option if we later need embedding-based search.

## 2 Secondary architectural change: persistent, access-controlled Chat subsystem (UI-enabled)
**What changed.** While the Chat UI is primarily frontend work, enabling chat required new backend architecture: a **ChatThread** data model tied to a `(listing, response)` pair, embedded `messages[]`, and a set of authenticated endpoints for thread creation, listing, retrieval, and message append.

**Why it was necessary.** Negotiation and coordination cannot be supported securely with client-only state. Server persistence provides continuity across sessions and enforces permissions (only the listing owner and response owner can participate).

**Technical debt / limitations.** Embedded messages may grow large; we may need pagination and/or a separate messages collection later. Real-time updates are deferred (HTTP append + refresh is sufficient for Demo 2).