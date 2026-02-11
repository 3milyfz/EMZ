# Release Demo 1 – Manifest

This README is the high-level manifest for the **Release Demo 1** of the Cultivate platform. It links to the live deployment for this release and indexes the main architectural artifacts in this monorepo.

## Live deployed application

- **Production web app URL**: https://cultivate-fe.vercel.app/

## Monorepo overview

- **Root (`./`)**  
  - Workspace configuration and shared tooling.  
  - NPM scripts to run the full stack locally (`npm run dev` starts both app and server).

- **Frontend (`pkgs/app`)**  
  - React + TypeScript + Vite single-page application.  
  - Uses Auth0 for authentication and the generated SDK to call the backend API.

- **Backend API (`pkgs/server`)**  
  - Hono + TypeScript HTTP API server.  
  - Persists data in MongoDB via Mongoose.  
  - Exposes OpenAPI-described endpoints that power both the frontend and the SDK.

- **TypeScript SDK (`pkgs/sdk`)**  
  - Generated API client used by the frontend.  
  - Contains endpoint and model documentation under `pkgs/sdk/docs`.

## Key architectural artifacts

- **Application architecture**
  - Frontend app entry point, routing, and providers in `pkgs/app` (see `pkgs/app/README.md` for framework details).
  - Backend HTTP server bootstrap and route wiring in `pkgs/server/src/index.ts`.

- **Domain & data model**
  - MongoDB/Mongoose models in `pkgs/server/src/models` (e.g., `User`, `Listing`).  
  - Validation schemas in `pkgs/server/src/schemas` for request/response shapes.

- **API surface & contracts**
  - High-level API overview and usage: `pkgs/sdk/README.md`.  
  - Endpoint-level docs and model references: files under `pkgs/sdk/docs/*.md`.  
  - Server routes implementing these contracts in `pkgs/server/src/routes`.

- **Auth & security**
  - Auth0 configuration consumed by the frontend in `pkgs/app/src/config.ts` and `pkgs/app/.env.example`.  
  - JWT verification and authorization middleware in `pkgs/server/src/middleware`.

- **Infrastructure & configuration**
  - Local development scripts and workspace configuration in the root `package.json`.  
  - Server runtime configuration and database connection in `pkgs/server/src/config.ts` and `pkgs/server/src/db.ts`.

## Related Demo 1 artifacts

- **`demo1/baseline-cuj.md` – Journey audit**  
  Structured Customer Journey table capturing actions, context switches, elapsed time, and friction severity, including the target persona and the value-driven goal for this release.  
  **Link placeholder**: _add link or path here_

- **`demo1/system-topology.jpg` – System topology diagram**  
  Visual diagram of foundational system boundaries and logical separation between **UI**, **Logic**, and **Data** tiers.  
  **Link placeholder**: https://github.com/3milyfz/EMZ/blob/9acdda38d414e74077662b81119aec29d9a87481/demo1/system-topology.jpg

- **`demo1/proof-of-concept.md` – Proof of Concept summary**  
  ~500-word technical summary defining the PoC scope for the primary interaction, what “working” means, and any known technical constraints or accepted limitations.  
  **Link placeholder**: _add link or path here_

## Local development (for reference)

- **Install dependencies** (from repo root):

  ```bash
  npm install
  ```

- **Run full stack locally** (frontend + backend):

  ```bash
  npm run dev
  ```

Then open `http://localhost:3000` in your browser to access the local application. For this release, the production URL listed above should point to the live-deployed instance of the same app.
