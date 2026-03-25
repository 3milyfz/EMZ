# Demo 4 – Build-trap postmortem

Demo 4 was our feature lock decision checkpoint, so the right question wasn’t “can we ship more?” but “did what we shipped actually test the hypotheses we needed for alpha?”

## Hypotheses we were trying to validate

1) **Conversational sourcing reduces restaurant friction**: if restaurants can describe an order in natural language (or upload a simple CSV) and get structured strategies back, they’ll complete sourcing faster than a manual browse-and-add flow.

2) **Chat persistence prevents drop-off**: if chats survive navigation and switching contexts, users won’t lose drafts or “where they were,” and the experience feels reliable enough to trust.

3) **Multimodal listing creation reduces farmer effort**: if a farmer can upload a photo and get a draft, the “time-to-first-listing” drops and we remove a major onboarding choke point.

4) **Location + pricing signals increase practical usefulness**: if we capture postal code and keep basic price hints fresh, search/matching and price-setting become less guessy and closer to the reality of local sourcing.

## What we built, and whether it was necessary

### What was necessary (direct hypothesis tests)
- **CSV demand upload + automatic extraction** were necessary to test Hypothesis #1 because they created a “single upload → structured line items” contract the UI (and future evaluation) can drive repeatedly. The validation and error codes ensure we can measure success/failure without hand-waving.
- **Optimization-assisted shopping with three options** was necessary to test Hypothesis #1 because it’s the mechanism that compresses sourcing decisions into a small, reviewable set. In product terms, these are **Lowest Cost**, **Best Match**, and **Balanced Match**.
- **Multi-chat CRUD + frontend session persistence** were necessary to test Hypothesis #2 because the failure mode we cared about was context loss. Persisting `activeChatId` per role and retaining drafts makes the “reliability feel” testable in real usage.
- **Authenticated image upload pipeline** was necessary groundwork for Hypothesis #3. Without a real upload/serve path (mime validation, processing, storage), “multimodal drafting” would have remained a demo stub.
- **Postal capture + geocoding at registration/settings** was necessary to test Hypothesis #4 because location-dependent flows can’t be evaluated without stable, normalized location data.

### What might have been overbuilt (risk of build trap)
- The optimizer ecosystem (strategy schemas, structured payloads, UI affordances) can expand quickly. The risk is building a “perfect planner” before proving it changes outcomes. For alpha, the minimum proof is: users can input an order, receive actionable allocations, and proceed to checkout without the system breaking.
- Premium dynamic pricing with automated daily price updates is valuable, but there’s build-trap risk if we treat “dynamic pricing” as a flagship without validating that farmers actually want frequent auto-adjustment versus simple guidance. The alpha goal is credibility and trust, not financial optimization.

## Signals we did (and didn’t yet) capture

We can already observe qualitative signals from internal testing:
- The system now fails **predictably** (400/401/415 with messages) instead of failing mysteriously—this directly reduces debugging time during alpha validation.
- Navigation and chat state feel more like a “real product” because sessions persist; we can now test longer, multi-step journeys without resets.

What we still don’t have (and should avoid building around blindly):
- Hard metrics tying optimizer usage to completion time, order success rate, or conversion. Without even lightweight instrumentation, we risk continuing to build planner features that feel impressive but don’t move the adoption needle.

## Decision

Demo 4’s builds largely **did validate the core hypotheses enough to justify feature lock for alpha**, with one caveat: the optimizer and pricing automation must be treated as “validated enough to freeze,” not “proven for scale.” The correct next step is controlled refinement: tighten edge cases, add minimal instrumentation, and prioritize reliability and clarity over expanding capability.

