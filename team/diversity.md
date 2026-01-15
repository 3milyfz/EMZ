# Diversity
---

## Diversity is ...

Diversity is not just who is on the team, it’s what perspectives, assumptions, and lived experiences shape our default decisions. In practice, it changes *what we notice*, *what we prioritize*, and *who our product works well for*. For this project, we treat diversity as an engineering requirement: it helps us reduce blind spots in UX, accessibility, privacy, and trust, and it improves our ability to validate product-market fit beyond “people like us.”

---

# Team Background
---

Our team has a varied background across culture, training, and professional context. We bring experience spanning software engineering, startup-style product development, and data/LLM research. Collectively, we speak multiple languages and have lived and worked across different regions, which gives us stronger sensitivity to communication norms, user expectations, and how “good product design” can look different depending on context.

At the same time, we recognize that we share meaningful overlap (similar academic environment and tech workplace norms). This overlap increases team cohesion and speed—but can also narrow our default assumptions about users, accessibility needs, and privacy expectations.

---

## Strengths & Weaknesses

Our team’s strengths come from having a tight end-to-end build loop: we can move from product concept → prototype → implementation → evaluation quickly. Emily’s full-stack and UI/UX execution strengths support fast iteration and a polished MVP, while Zoey’s data + LLM experience supports grounded architecture decisions, evaluation, and responsible AI constraints (e.g., how to handle failure modes, calibration, and user trust).

Our weaknesses are mostly “coverage” gaps rather than capability gaps:
- As a small team, we have limited bandwidth and can over-index on what we personally find intuitive.
- We may default to high-tech literacy user assumptions (e.g., comfort with AI workflows, interpretability expectations, willingness to troubleshoot).
- We have limited lived experience with accessibility needs (vision/motor/cognitive), low-bandwidth contexts, and non-English-first usage patterns—areas that strongly affect whether a product is inclusive.

---

### Subject Matter Experts

Subject matter experts (SMEs) who can help us address our expertise gaps are:
- **LLM/ML Engineer (deployment-focused):** Helps us choose the right architecture (prompt-only vs RAG vs fine-tuning) under cost/latency constraints, and design monitoring + iteration loops in production.
- **Privacy / Responsible AI SME:** Advises on data minimization, storage/retention policies, consent + disclosure language, and safe handling of sensitive text.
- **Accessibility / Inclusive Design SME:** Helps us test for assistive tech compatibility, cognitive load, readability, and “works for real humans” UX.
- **Domain SMEs (THEME industry):** Helps validate user pain points, workflows, and what “value” means in this domain—so our MVP solves a real problem rather than a technically interesting one.

---

## Diversity

Our team has a varied background, but we acknowledge that we do not have people experienced in / who are:
- **People with disabilities** (vision, motor, cognitive), or those who rely on assistive technologies
- **Users with low tech literacy** or limited AI familiarity
- **People from lower-resource contexts** (limited time, device constraints, low bandwidth, or strong cost sensitivity)
- **Non-English-first users** or communities where translation/localization is essential
- **Communities with different norms around trust, privacy, and AI**, especially in sensitive or high-stakes contexts
- **Users outside university/tech ecosystems**, who may use different mental models and workflows

---

### How this can impact us

A potential impact of this is that we may unintentionally design for “default users” who resemble our own context: high literacy, high device access, and familiarity with product conventions and AI tools. This could show up as:
- **Accessibility gaps:** UI choices that are visually polished but not navigable via keyboard/screen readers, or that create unnecessary cognitive load.
- **Trust + privacy missteps:** Over-collection of user data by default, unclear consent flows, or assumptions that users want personalization/memory.
- **Workflow mismatch:** Features that are technically strong but don’t map to how real users in the THEME industry actually make decisions.
- **Over-trust risk:** Users may treat LLM outputs as authoritative unless we design clear uncertainty signals, guardrails, and explanations.

People with backgrounds that differ from our own can help us in the THEME industry because they widen our “test surface area.” They help us catch failure modes early—where the product breaks, confuses, excludes, or violates expectations—before those issues become expensive to fix. Concretely, they make our product stronger by:
- Challenging our assumptions about what users *want* vs what we *think they should want*
- Improving accessibility and usability across different contexts
- Stress-testing our privacy and trust design choices
- Helping us build something that’s robust across cultures, abilities, and real-world constraints