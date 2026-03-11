# Demo 3 Pivot Contract

## Pivot Under Test
We are pivoting from a traditional marketplace web app toward a **chat-first farm agent** experience.  
Instead of asking users to browse, filter, and manually fill out marketplace forms, we are testing whether buyers and sellers will prefer to complete key workflows through a conversational interface.

This sprint does **not** test “adding one more feature” to the existing marketplace. It tests a different product interaction model and a different competitive position.

---

## Hypothesis

### Competitive Positioning Hypothesis
If we reposition our product as a **farm commerce agent** rather than a standard produce marketplace, then users will find it more efficient, intuitive, and differentiated because the agent can unify multiple high-friction workflows into one interface:

- produce discovery through chat
- listing creation through chat
- voice-to-text input for faster entry
- AI-assisted listing enrichment through computer vision and dynamic pricing
- lightweight checkout through the same conversational flow

Our belief is that this creates a stronger competitive position than a conventional marketplace because:

1. **Lower workflow friction**  
   Users do not need to navigate multiple pages, forms, and filters.

2. **Better fit for fragmented, low-digital-maturity users**  
   Small farmers and buyers may be more willing to “describe what they need” than learn a marketplace UI.

3. **Higher perceived product intelligence**  
   The system does more than match listings; it helps users decide, structure, price, and act.

4. **More defensible product direction**  
   Many marketplaces look interchangeable; a farm-specific agent that integrates sourcing, listing, and checkout is a more differentiated wedge.

### Specific Sprint Hypothesis
Users will successfully complete at least one of the following core jobs through the chat interface without needing the old marketplace UI:

- shop for produce
- create a listing
- use voice-to-text to submit intent
- move toward checkout

If users can do this with acceptable clarity and speed, then the agent-first direction is promising enough to continue.

---

## What We Are Testing This Sprint

### Core Experience
The sprint tests whether a conversational interface can become the main surface for our product.

### Demo Scope
The farm agent should support:

1. **Produce shopping through chat**  
   Users express what they want in natural language and receive relevant results.

2. **Groq-backed agent response flow**  
   The agent interprets requests and guides the interaction.

3. **Voice-to-text input**  
   Users can speak instead of typing.

4. **Listing creation through chat**  
   Users can create supply listings conversationally, with support from computer vision and dynamic pricing features.

5. **Quick checkout flow**  
   Users can move from intent to purchase with minimal steps.

---

## Success Logic

We are not claiming that the agent must outperform a mature marketplace on every dimension in one sprint.  
We are testing whether it shows enough evidence to justify continuing the pivot.

Evidence we want to see:

- users understand what the agent can do
- users can complete meaningful tasks with limited confusion
- the chat UX feels simpler, not more complicated, than the marketplace flow
- the agent experience feels meaningfully different from a generic chatbot wrapper

---

## Kill Metric

We will kill or significantly narrow this pivot if, by the Demo 3 decision point, the chat-first agent cannot reliably complete at least **3 of the 4 core workflows** in a real end-to-end demo without manual patching or UI escape hatches.

### Core Workflows
- produce shopping through chat
- listing creation through chat
- voice-to-text input into the agent flow
- quick checkout progression

### Falsifiable Threshold
A workflow only counts as “working” if it:

- starts from a natural user prompt or voice input
- stays entirely inside the chat-first interface
- returns a usable result
- does not require hardcoded intervention during the demo
- does not redirect the user back to the old marketplace flow to complete the task

If fewer than **3 of the 4 workflows** meet this threshold, we will treat the pivot as unsuccessful and either kill it or narrow it to a smaller agent-assisted use case.

### Why This Threshold Matters
This metric is designed to test whether the chat-first farm agent is actually viable as a primary product interaction model, rather than just a thin demo layer on top of the existing marketplace. If the agent cannot support most of the promised workflows in a credible end-to-end experience, then the pivot has not earned continued investment.

---

## Trigger Date

### Firm Decision Point
**End of Demo 3 sprint / before Demo 4 planning**

By this date, we must make a clear decision:

- **continue** investing in the chat-first farm agent as the main product direction
- **limit** the agent to a narrower role
- or **revert** to marketplace-first with AI assistance layered in selectively

This is a firm decision point because continuing the pivot affects future UI, engineering priorities, and product positioning. We should not keep both directions alive indefinitely without evidence.

---

## Decision Rules

### Continue the Pivot if:
- the kill metric is met or exceeded
- users can complete core workflows through chat with manageable friction
- the experience feels more natural than the existing marketplace flow
- the team sees a credible path to improving reliability next sprint

### Narrow the Pivot if:
- one workflow clearly works well, but the full chat-first product does not

Example:
- shopping through chat works
- but listing creation or checkout through chat does not

In that case, we keep the working wedge and stop forcing a full agent-first experience.

### Kill the Pivot if:
- the threshold is missed
- users do not trust or understand the chat flow
- the agent creates more UX friction than it removes
- the product loses clarity compared with the original marketplace

---

## Fallback Options

If the chat-first pivot fails, we do not discard the sprint learnings.  
We fall back to one of the following strategic alternatives.

### Fallback Option 1: Marketplace-First, Agent-Assisted
Keep the traditional marketplace UI as the main interaction model, but embed the agent as a helper for:

- search guidance
- listing drafting
- price suggestions
- onboarding assistance

This keeps the lowest-risk product structure while preserving AI value.

### Fallback Option 2: Agent Only for Listing Creation
Use chat as a specialized workflow for supply-side listing creation only.

Rationale:
Listing creation is form-heavy and may benefit most from:
- voice input
- CV-based produce recognition
- dynamic pricing support

This gives the agent a focused and defensible use case.

### Fallback Option 3: Agent Only for Produce Sourcing / Concierge Search
Position the agent as a sourcing assistant for buyers rather than a full commerce surface.

Rationale:
“Help me find local basil for next week” is a natural conversational task and may be easier than supporting full transactional flows.

### Fallback Option 4: Voice Layer on Existing Product
If chat-first UX is too disruptive, preserve voice-to-text as a productivity layer on top of the current marketplace.

This still supports accessibility and speed without forcing a full interface redesign.

---

## Strategic Rationale for the Pivot Contract

This contract exists to prevent us from drifting into a vague “AI feature expansion” story.  
We are making a concrete strategic bet:

> A farm commerce agent may create a better wedge than a standard marketplace UI.

That bet should be judged by evidence, not enthusiasm.  
If the evidence is weak, we should narrow or reverse the pivot quickly.  
If the evidence is strong, we should commit and build depth around the agent experience.

---

## One-Sentence Commitment
We will continue the farm-agent pivot only if users demonstrate that chat is a clearer and more effective interface for farm commerce than our current marketplace flow; otherwise, we will narrow the agent to a focused assistive role or return to a marketplace-first product.