# Release Demo 3 – Manifest

This README is the high-level manifest for **Release Demo 3** (Competitive Review & Gap Analysis). It links to the live deployment and indexes the main architectural and strategy artifacts in this monorepo.

## Live deployed application

- **Production web app URL:** https://cultivate-fe.vercel.app/

## Demo 3 artifacts

All release deliverables for Demo 3 are under **`demo3/`**:

| Artifact | Description |
|----------|-------------|
| [demo3/competitive-review-cuj.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/competitive-review-cuj.md) | Competitive Review CUJ across Cultivate and market alternatives, including workflow comparison, friction analysis, gap quantification, and benchmarking conclusions. |
| [demo3/feature-prioritization.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/feature-prioritization.md) | Gap analysis with quantified performance ratios, feature parity assessment, critical vs acceptable gaps, and strategic feature decisions with justification. |
| [demo3/evolved-topology.jpg](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/evolved-topology.jpg) | Updated system topology diagram showing the major architectural evolution for the chat-first farm agent experience. |
| [demo3/architectural-rationale.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/architectural-rationale.md) | Design decision log covering what changed, why it was necessary, alternatives considered, technical debt introduced or resolved, and remaining limitations. |
| [demo3/pivot-contract.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/pivot-contract.md) | Pre-sprint pivot contract defining the competitive hypothesis, kill metric, trigger date, and fallback options. |
| [demo3/build-trap-postmortem.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/build-trap-postmortem.md) | Post-sprint retrospective evaluating whether the implemented features delivered the hypothesized value and whether building them was necessary to test the pivot. |
| [demo3/README.md](https://github.com/3milyfz/EMZ/blob/v1.0.5/demo3/README.md) | Demo 3 release index and manifest. |
---

## Release focus

Demo 3 represents a strategic product pivot from a **traditional marketplace web app** toward a **chat-first farm agent** experience.

Instead of relying primarily on page-based browsing, filtering, and form entry, this release tests whether buyers and sellers can complete core workflows through a conversational interface. The release focuses on validating whether chat can become the main product surface for farm commerce.

### Core workflows in scope
- produce shopping through chat
- listing creation through chat
- voice-to-text input into the agent flow
- quick checkout progression

### Key product capabilities introduced
- chat-first farm agent interface
- Groq-backed agent orchestration
- voice-to-text input
- image-assisted listing creation
- AI-assisted listing enrichment via computer vision and dynamic pricing
- lightweight checkout flow inside the chat experience

---

## Monorepo overview

- **Root (`./`)**  
  Workspace configuration and shared tooling. NPM scripts run the full stack locally; `npm run dev` starts both frontend and backend.

- **Frontend (`pkgs/app`)**  
  React + TypeScript + Vite single-page application. Hosts the main user interface and now supports the evolving chat-first interaction layer alongside the existing marketplace structure.

- **Backend API (`pkgs/server`)**  
  Hono + TypeScript HTTP API server. Persists data in MongoDB via Mongoose. Exposes the application API used for listings, users, chat-related workflows, images, and other marketplace logic.

- **TypeScript SDK (`pkgs/sdk`)**  
  Generated API client used by the frontend. Contains endpoint and model documentation for the backend contract.

---

## Key architectural artifacts

- **Application architecture**  
  Frontend entry points, routing, and providers live in `pkgs/app`. Backend bootstrap and route wiring live in `pkgs/server/src/index.ts` and `pkgs/server/src/app.ts`.

- **Domain & data model**  
  MongoDB/Mongoose models in `pkgs/server/src/models` support users, listings, chats, images, and related domain objects.

- **API surface & contracts**  
  API documentation and generated client artifacts live in `pkgs/sdk`. Backend route implementations live in `pkgs/server/src/routes`.

- **Auth & security**  
  Authentication and authorization are handled through Auth0 integration in the frontend and JWT verification middleware in the backend.

- **Infrastructure & configuration**  
  Shared configuration is managed at the repo root, with package-specific configuration under `pkgs/app` and `pkgs/server`. Media and image-processing support connect to the listing creation workflow.

---

## What changed in Demo 3

The most important evolution in this release is not just feature count, but **interaction model**.

Previously, the product centered on a conventional marketplace flow:
- browse listings
- filter results
- manually fill out forms
- move through separate pages for actions

In Demo 3, the product shifts toward an agent-driven flow:
- express intent in chat or by voice
- let the agent interpret and guide the task
- create listings from uploaded produce images
- confirm quantity and price instead of filling out full forms
- move toward checkout inside the same conversational surface

This release is therefore both a **product pivot** and an **architectural evolution**, not just an incremental feature sprint.

---

## Local development

**Install dependencies** (from repo root):

```bash
npm install