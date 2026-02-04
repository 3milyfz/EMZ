# System Topology Rationale

This MVP is deployed as a **decoupled three-tier system** optimized for real-world reliability without over-engineering considering the use case. We intentionally separated **presentation**, **application**, and **data** concerns so each tier can evolve independently as load and product requirements grow.

---

## 1. System Topology & Service Boundaries

### Presentation Tier — Vercel (React SPA)
- **What it does:** Serves the compiled React app (Vite build) as static assets via Vercel’s edge/CDN.
- **Boundary:** The frontend is **stateless** and communicates with the backend only through HTTPS REST calls.
- **Why this boundary:** Static hosting + CDN provides fast global delivery with near-zero operational overhead.

### Application Tier — Railway Web Service (Node/Express API)
- **What it does:** Handles business logic (randomizer rules, timer orchestration, CRUD for teams/session state) and access control.
- **Boundary:** Exposes REST endpoints (e.g., `/api/teams`, `/api/randomize`, `/api/reset`, `/api/status`) and enforces **JWT authentication**.
- **Why this boundary:** The backend is the “policy + logic” layer. Keeping it separate allows independent deployment and scaling without coupling UI changes to server logic.

### Data Tier — Railway Persistent Volume (SQLite + Prisma)
- **What it does:** Stores user accounts, team metadata, and session/randomizer state in a SQLite file located at:
  - **`/data/app.db`** (Railway volume mount)
- **Boundary:** Backend accesses the DB through **Prisma ORM** using `DATABASE_URL=file:/data/app.db`.
- **Why this boundary:** We keep persistence co-located with compute for low latency and simplicity, while still maintaining a clear “data tier” boundary for future migration.


