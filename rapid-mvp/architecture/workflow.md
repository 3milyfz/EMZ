# Deployment Workflow Summary

This document summarizes the automated deployment pipeline and how it achieves **Deployment Zen** (hands-off deployment from repository to production).

---

## Workflow file (permalink)

**Direct link to the workflow in the code repository (main branch):**

[https://github.com/3milyfz/classroom-presentation-generator/blob/master/.github/workflows/deploy.yml](https://github.com/3milyfz/classroom-presentation-generator/blob/master/.github/workflows/deploy.yml)

---

## Trigger event

The pipeline runs **only** on:

1. **Tag push** — Pushing a tag that matches `v*` (e.g. `v1.0.0`, `v1.0.0-A3`).
2. **GitHub Release published** — Creating and publishing a new Release from the repo.

There is no manual “Run workflow” dependency for production deploys: a specific, intentional event (tag or release) is the single trigger.

---

## Build steps

| Step | What it does |
|------|-------------------------------|
| Checkout | Clone the repository. |
| Setup Node | Install Node 20 and enable npm cache. |
| Install backend deps | `npm ci` in `backend/`. |
| Prisma generate | `npm run db:generate` in `backend/` (with a placeholder `DATABASE_URL` for the build environment only). |
| Install frontend deps | `npm ci` in `frontend/`. |
| Build frontend | `npm run build` in `frontend/` (Vite production build). |

The **deploy** job runs only if the **build** job succeeds.

---

## Deploy target

- **Target:** **Vercel** (production).
- **What is deployed:** The **frontend** application (static build from `frontend/`).
- **How:** The deploy job installs the Vercel CLI and runs `vercel deploy --prod` from the `frontend/` directory, using a token from GitHub Secrets.

The backend is not deployed by this workflow; it is run elsewhere (e.g. local or a separate host).

---

## How secrets are used (names only, no values)

| Purpose | Type | Name | Used for |
|--------|------|------|----------|
| Deploy to Vercel | GitHub Secret | `RANDOMIZER_TOKEN` | Vercel API token for `vercel deploy --prod`. The job fails fast if this is missing. |
| Frontend API URL (optional) | GitHub Variable or Secret | `VITE_API_BASE` | Injected at frontend build/deploy time so the app can call the correct backend URL. |

No credentials or API keys are stored in the workflow file or the repository; all sensitive values are supplied via GitHub Secrets (and optional Variables).

---

## How Deployment Zen is achieved (hands-off)

**Deployment Zen** here means: **no manual cloud push**. The path from code to production is fully automated once a tag or release is created.

1. **Single trigger** — A tag push or a published Release is the only trigger. The team does not click “Deploy” in a cloud console or run deploy commands by hand for normal releases.
2. **Automated build** — The workflow runs the full build (backend deps, Prisma generate, frontend build) in a consistent environment. If the build fails, nothing is deployed.
3. **Automated deploy** — If the build succeeds, the deploy job runs and pushes the frontend to Vercel production using the stored token. No manual step is required.
4. **Repeatable and auditable** — Every production deploy is tied to a specific tag or release, so you can see exactly what was deployed and when from the repo and Actions history.

So: **tag or release → build → deploy**. No manual cloud push is required; that’s how Deployment Zen is achieved.
