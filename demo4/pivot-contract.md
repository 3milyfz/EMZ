# Pivot Contract

## Sprint focus

This sprint focused on the **optimization algorithm** that powers buyer-side sourcing. The system uses an LLM to evaluate the restaurant’s submitted demand against produce listings in the database, then computes a matching outcome using two main dimensions:

- **match quality** between user demand and listing attributes
- **pricing weight** to account for cost efficiency

This produces three decision outputs for the user:

1. **Lowest Cost** — prioritizes minimum total purchase cost  
2. **Best Match** — prioritizes the strongest fit to requested produce characteristics  
3. **Balanced Match** — trades off cost and produce-fit quality

The purpose of this sprint was not just to build ranking logic, but to test whether optimization meaningfully improves the buyer journey enough to justify locking it into the alpha release.

---

## Hypothesis

If the optimization workflow is effective, then restaurant users should be able to move from uploaded demand to a reviewable sourcing outcome **with materially less manual search effort than the baseline**, while still receiving recommendations that are meaningfully differentiated across the three output modes.

More specifically, we hypothesize that:

- the optimization system will reduce buyer effort compared with the baseline manual sourcing flow
- users will receive **three distinct recommendation sets** that reflect real tradeoffs between price and fit
- the recommendations will be good enough that the user can proceed to checkout review without needing to manually reconstruct the sourcing process themselves

### Quantified value target
We define success as the system achieving all of the following during internal validation:

- **at least 50% reduction in interaction effort** relative to the baseline manual sourcing flow
- **at least 3 out of 4 test cases** producing recommendation outputs that are clearly differentiated across Lowest Cost, Best Match, and Balanced Match
- **at least 3 out of 4 end-to-end trials** resulting in a reviewable cart without manual search being reintroduced outside the optimization flow

This is the value claim of the sprint: the optimization layer should do enough of the sourcing work that the product is meaningfully better than a manual marketplace experience.

---

## Kill metric

We will pivot away from the current optimization approach if, by the trigger date, **either** of the following is true:

1. **Fewer than 3 out of 4 internal test runs** can complete the flow from demand submission to reviewable recommendation output without manual intervention or fallback to manual listing search

**or**

2. The three output modes (**Lowest Cost**, **Best Match**, **Balanced Match**) are not meaningfully distinguishable in practice in **more than half of evaluated cases**, meaning the optimization does not create decision value beyond a generic ranking list

### Why this is falsifiable
This kill metric is intentionally concrete. The sprint fails if:
- the workflow cannot reliably produce usable recommendations
- the output categories collapse into near-identical results
- the user still has to do the core sourcing work manually

If any of those happen, the current optimization strategy is not strong enough to justify continued investment in this form.

---

## Trigger date

**Trigger date: End of Demo 4 sprint / before alpha feature lock decision**

This is the firm decision point for whether the current optimization system remains part of the locked alpha product.

By this date, the team must decide one of two things:

- **Proceed**: the optimization logic is sufficiently useful, distinguishable, and reliable to remain in the alpha release
- **Pivot**: the current approach does not create enough value or reliability, and the team should simplify or replace it before further expansion

This decision must be made before the team moves from feature building into alpha validation and stabilization, because optimization is central to the restaurant-side value proposition.

---

## Fallback options

If the current optimization approach does not meet the kill metric, the team will not continue expanding it in its present form. Instead, we will choose one of the following fallback options.

### Fallback A: Simplify to rule-based ranking
Replace the LLM-led evaluation layer with a narrower deterministic ranking method based on:
- produce type match
- quantity availability
- price
- simple keyword alignment from demand notes

This reduces model complexity and improves consistency. It is the preferred fallback if the main problem is unreliable or unstable scoring.

### Fallback B: Keep only one recommendation mode
If three recommendation styles do not create clear user value, collapse the interface into a single default recommendation view, likely:
- **Balanced Match** as the default mode

This keeps the optimization concept while removing artificial complexity that the user may not benefit from.

### Fallback C: Reposition optimization as assisted filtering
If end-to-end recommendation generation is not reliable enough, reposition the system as a guided shortlist generator rather than a decision engine. In that version, the algorithm produces a filtered candidate set for the user to browse, instead of claiming to optimize the full sourcing choice.

### Fallback D: Narrow the supported use case
If the optimization works only for structured, common produce requests, then alpha scope should be reduced to those cases only. Edge-case sourcing and nuanced produce preference matching can be deferred until beta.

---

## Strategic rationale

This contract exists because optimization is easy to overbuild. A sophisticated ranking pipeline is only worth keeping if it creates clear decision value for the buyer. The team should not keep investing in this feature merely because it is technically interesting.

The question for this sprint is simple:  
**Does the optimization algorithm materially improve the sourcing journey enough to deserve a place in the alpha release?**

If yes, it becomes part of the locked product surface.  
If no, the team should pivot quickly to a simpler and more reliable alternative.

---

## Decision rule

At the trigger date:

- **Go forward** if the system consistently produces usable, differentiated, reviewable recommendations and clearly reduces manual sourcing effort
- **Pivot** if the system is unreliable, indistinguishable across output modes, or still depends on the user doing too much manual sourcing work

This keeps the sprint aligned with product value rather than model sophistication.