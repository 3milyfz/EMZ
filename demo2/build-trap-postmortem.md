# Build Trap Post-Mortem — Demo2 (Cultivate)

## What we set out to validate (Proof of Value)
This sprint’s hypothesis focused on reducing the two biggest frictions in our Product Definition CUJ and Competitive Review CUJ (vs Facebook Marketplace):  
1. **Listing creation friction** (manual item/title/description causes incomplete/unstructured listings), and  
2. **Coordination friction** (repetitive, “business language” messaging around produce name, quantity, and pickup window).

We prioritized an “AI Assist Pack” concept (Auto-Lister + Copilot), but we validated it with **Human-in-the-Loop** control: generated content is reviewed/edited before publishing or sending.

## What we built and whether it delivered expected value
### Auto-Lister (Photo → Draft, HITL) + Price hints
This feature directly targeted the Facebook Marketplace gap: farmers currently must manually fill in key listing fields, often resulting in missing/unstructured posts. Our Auto-Lister reduced the burden of writing item/title/description and shifted the farmer’s role to **evaluation** rather than authoring. Adding a **StatCan-based price reference** further reduced decision friction for a high-impact field (price), improving posting confidence and consistency.

**Value delivered:** Faster time-to-post and better listing completeness (our intended outcome).  
**Did we need to build it to learn this?** Largely yes. The core value is experiential: the reduction in typing and the quality of the draft are only credible when users interact with it end-to-end. A paper prototype could validate “people dislike typing,” but not whether photo→draft actually removes enough friction without creating edit overhead.

### Chat interface + Copilot auto-completion (HITL)
Our UI decision was that **chat is created upon offer submission**, and restaurants can open the thread via a **message icon** on responses. This anchored coordination to a plausible transaction and reduced low-signal messaging. For “copilot,” we intentionally shipped a **hard-coded, rule-based auto-completion** (PR #16) instead of LLM APIs because early produce negotiation is highly structured (produce name, quantity, pickup window). This provided instant, free, deterministic suggestions and avoided integration instability during a time-boxed sprint.

**Value delivered:** Reduced messaging friction for structured first-contact and confirmations, with reliable demo behavior.  
**Did we need to build it to learn this?** Partially. We could have validated message structure needs with scripted templates (no UI), but building the chat UX showed whether suggestions fit naturally into the flow (timing, affordances, acceptance/edit behavior).

## Demand validation vs capability validation
- **Demand validation:** Interviews and competitive benchmarking support that farmers want faster listing creation and that buyers benefit from more structured listings. Users also value faster, clearer negotiation.
- **Capability validation:** We demonstrated we can operationalize these outcomes with HITL automation (photo→draft) and deterministic messaging assistance. We did **not** validate whether an LLM-based copilot is necessary yet; our rule-based approach suggests many early messages may not require generative AI.

## What we did not implement and why (and what we learned)
We deprioritized **fit score** and the **Ag Guide agent** this sprint. Building them would have expanded scope into (a) constraint modeling and comparability logic (fit score), and (b) grounding and hallucination risk (agent). Our learning this sprint is that the **largest immediate bottlenecks** to adoption are creation/coordination friction; fit score + Ag Guide are likely the next bottlenecks once transactions scale and farms need feasibility support.

## Build-trap reflection (what we built too early)
The main potential build-trap risk was initially framing “copilot” as LLM-dependent. We avoided premature build by shipping a rule-based copilot first. Next sprint, if we explore LLM APIs, we will design an A/B test against templates/rules to isolate whether LLMs add incremental value beyond structured prompts.

## What changes in the next pivot contract
Next sprint’s pivot contract should:
1. Treat LLM integration as a **secondary hypothesis** with explicit incremental-value metrics (beyond rule templates).
2. Prioritize **fit score** and **Ag Guide (tiered, grounded)** as core workflow enablers if our CUJ stalls at feasibility/decision steps.
3. Add clearer pre-commitment about which learnings require building vs can be tested with prototypes (e.g., validating fit score logic with mocked offers before implementing full UI).