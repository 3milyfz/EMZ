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

---

## 2. Control Plane vs. Data Plane

### Control Plane: Orchestration, Deployment, Secrets
- **Source of truth:** GitHub repositories
- **Delivery automation:**
  - Frontend deploys to Vercel (CI/CD via Git-based deployment)
  - Backend deploys to Railway (Git-based deploy)
- **Secret management:**
  - GitHub Secrets for CI/CD tokens (e.g., Vercel deploy token)
  - Vercel Environment Variables (e.g., `VITE_API_BASE_URL`)
  - Railway Environment Variables (e.g., `DATABASE_URL`, `JWT_SECRET`, `NODE_ENV=production`)
- **Why this structure:** To prioritize a repeatable, low-touch pipeline with no manual server access and no secrets in source code.

> **Important deployment note (SQLite + Volume):** Railway volumes mount at **runtime**, so schema initialization must run at **runtime** as well. We run `prisma migrate deploy` during service start to ensure migrations apply against the mounted `/data` volume, not the ephemeral build container filesystem.

### Data Plane (User Request Flow)
1. Instructor opens the app in a browser → Vercel serves the SPA over HTTPS.
2. SPA calls backend endpoints via HTTPS + `Authorization: Bearer <JWT>`.
3. Backend validates JWT (stateless auth), executes business logic, and reads/writes via Prisma.
4. Prisma persists state to `/data/app.db` on the Railway volume.

Data plane characteristics:
- **Stateless API:** No server-side sessions required; JWT authenticates each request.
- **Persistence:** Session state survives redeploys/restarts via volume-backed DB file.
- **Transport security:** HTTPS between tiers.

---

## 3. Immediate MVP Primitives (for A3 specifically)

These choices meet sprint constraints and classroom-scale usage while remaining cloud-ready.

### Frontend: Vercel Edge Hosting
- **Primitive:** Static SPA hosting with CDN distribution
- **Why now:** Fast, reliable delivery with minimal ops; handles traffic spikes automatically.
- **Constraint fit:** No backend coupling, clean FE/BE separation, easy environment variable injection.

### Backend: Railway Single Web Service
- **Primitive:** One containerized Node/Express instance
- **Why now:** Expected scale is small (classroom settings), so a single instance is sufficient and debuggable.
- **Constraint fit:** Clear service boundary; easy to automate deploys.

### Database: SQLite on Railway Volume
- **Primitive:** File-based persistence at `file:/data/app.db`
- **Why now:** Lowest operational overhead; fast local reads/writes; perfect for light data volume.
- **Constraint fit:** Still qualifies as persistent storage in a cloud environment; simple to explain and validate.

### Authentication: JWT Bearer Tokens
- **Primitive:** Stateless auth with signed tokens and middleware verification
- **Why now:** Avoids cookie/session infrastructure; easy to deploy and scale horizontally later.
- **Constraint fit:** Enables multi-user separation (queries scoped by `userId`) with minimal moving parts.


