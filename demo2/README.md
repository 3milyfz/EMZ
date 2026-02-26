# Release Demo 2 – Manifest

This README is the high-level manifest for **Release Demo 2** (Core Features & Proof of Value). It links to the live deployment and indexes the main architectural artifacts in this monorepo.

## Live deployed application

- **Production web app URL:** https://cultivate-fe.vercel.app/  

## Demo 2 artifacts (Assignment 5)

All release deliverables for Demo 2 are under **`demo2/`**:

| Artifact | Description |
|----------|-------------|
| [demo2/product-definition-cuj.md](https://github.com/3milyfz/EMZ/blob/42c25ac289903b76a7c6585473ddc280d47451b3/demo2/product-definition-cuj.md) | End-to-end Product Definition CUJ: persona, journey steps, feature–value mapping. |
| [demo2/feature-prioritization.md](https://github.com/3milyfz/EMZ/blob/42c25ac289903b76a7c6585473ddc280d47451b3/demo2/feature-prioritization.md) | Strategic prioritization, implementation timeline (this sprint vs deferred). |
| [demo2/evolved-topology.png](https://github.com/3milyfz/EMZ/blob/4dfd7521e7eb8025c4dc9741325d64f81585f1f2/demo2/evolved-topology.png) | Evolved system topology diagram (industry-standard layered view). |
| [demo2/architectural-rationale.md](https://github.com/3milyfz/EMZ/blob/42c25ac289903b76a7c6585473ddc280d47451b3/demo2/architectural-rationale.md) | Design decisions: what changed, why, alternatives, technical debt, limitations. |
| [demo2/pivot-contract.md](https://github.com/3milyfz/EMZ/blob/42c25ac289903b76a7c6585473ddc280d47451b3/demo2/pivot-contract.md) | Pre-sprint pivot contract (hypothesis, kill metric, trigger date, fallbacks). |
| [demo2/build-trap-postmortem.md](https://github.com/3milyfz/EMZ/blob/42c25ac289903b76a7c6585473ddc280d47451b3/demo2/build-trap-postmortem.md) | Post-sprint retrospective and validation. |
| [demo2/README.md](https://github.com/3milyfz/EMZ/blob/4dfd7521e7eb8025c4dc9741325d64f81585f1f2/demo2/README.md) | Demo 2 release index and checklist. |

---

## Monorepo overview

- **Root (`./`)**  
  Workspace configuration and shared tooling. NPM scripts to run the full stack locally: `npm run dev` starts both frontend and backend.

- **Frontend (`pkgs/app`)**  
  React + TypeScript + Vite single-page application. Auth0 for authentication; generated SDK for backend API calls. Routes: Home, Register, Listings, Listing Detail, New/Edit Listing, **Messages**, **Chat Thread** (per-response).

- **Backend API (`pkgs/server`)**  
  Hono + TypeScript HTTP API server. Persists data in MongoDB via Mongoose. Exposes REST endpoints described by OpenAPI; JWT verification via Auth0. Key route groups: `/api/listings`, `/api/users`, `/api/chat`, `/api/images`, produce-items.

- **TypeScript SDK (`pkgs/sdk`)**  
  Generated API client used by the frontend. Endpoint and model documentation under `pkgs/sdk/docs`.

---

## Key architectural artifacts

- **Application architecture**  
  Frontend entry point, routing, and providers in `pkgs/app`. Backend bootstrap and route wiring in `pkgs/server/src/index.ts` and `pkgs/server/src/app.ts`.

- **Domain & data model**  
  MongoDB/Mongoose models in `pkgs/server/src/models`: `User`, `Listing`, `ChatThread`, `ImageAsset`, `DraftSuggestion`, `ProduceItem`. Validation schemas in `pkgs/server/src/schemas`.

- **API surface & contracts**  
  High-level API overview: `pkgs/sdk/README.md`. Endpoint-level docs: `pkgs/sdk/docs/*.md`. Implementations: `pkgs/server/src/routes` (listings, users, chats, images, produce-items).

- **Auth & security**  
  Auth0 configuration in `pkgs/app/src/config.ts` and env. JWT verification and user resolution in `pkgs/server/src/middleware/auth.ts`. CORS and error handling in `pkgs/server/src/app.ts`.

- **Infrastructure & configuration**  
  Root `package.json` for workspace and dev scripts. Server config and DB connection: `pkgs/server/src/config.ts`, `pkgs/server/src/db.ts`. Image storage via GridFS; optional Azure Vision for draft-from-image.

---

## Local development

**Install dependencies** (from repo root):

```bash
npm install
```

**Run full stack** (frontend + backend):

```bash
npm run dev
```

Then open the frontend (e.g. `http://localhost:5173` for Vite) and ensure the app is configured to talk to the local API (e.g. `http://localhost:3000`). See `pkgs/app` and `pkgs/server` READMEs for package-specific setup and env vars.

