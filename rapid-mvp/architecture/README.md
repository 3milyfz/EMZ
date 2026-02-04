# Architecture Manifest

High-level index of architectural artifacts for the Classroom Presentation Randomizer MVP.

## Live Application

**Production URL:** [https://classroom-presentation-randomizer.vercel.app/](https://classroom-presentation-randomizer.vercel.app/)

## Architectural Artifacts

- **[Cloud Architecture Diagram](diagram.jpg)** — Visual representation of the deployment architecture
- **[Architecture Rationale](rationale.md)** — Design decisions and trade-offs
- **[Deployment Workflow Summary](workflow.md)** — CI/CD pipeline overview
- **[Reflections](reflections.md)** — Post-deployment learnings and insights

## Code Repository

- **GitHub Repository:** [https://github.com/3milyfz/classroom-presentation-generator](https://github.com/3milyfz/classroom-presentation-generator)
- **Deployment Workflow:** [`.github/workflows/deploy.yml`](https://github.com/3milyfz/classroom-presentation-generator/blob/1517b69fb2aec9dc912c75c8e149eb91fd7f4274/.github/workflows/deploy.yml)

## How to Validate Deployment

### Frontend (Vercel)
- [ ] Visit the production URL and verify the app loads
- [ ] Click "Register" or "Login" to test authentication flow
- [ ] Verify API calls succeed (check browser Network tab for successful requests to backend)

### Backend (Railway)
- [ ] Check Railway dashboard: service is running and healthy
- [ ] Test API endpoint: `https://backend-production-4a2e.up.railway.app`
- [ ] Verify database connection: register/login endpoints work without errors

### CI/CD Pipeline
- [ ] Check GitHub Actions: latest workflow run completed successfully (green checkmark)
- [ ] Verify deployments: both frontend (Vercel) and backend (Railway) jobs completed
- [ ] Test trigger: push a tag (e.g. `git tag v1.0.1 && git push origin v1.0.1`) and confirm new deployment starts

## Architecture Overview

- **Frontend:** React + Vite, deployed to Vercel
- **Backend:** Node.js + Express + Prisma, deployed to Railway
- **Database:** PostgreSQL (production), SQLite (local dev)
- **Authentication:** JWT-based
- **CI/CD:** GitHub Actions (triggers on tag push `v*` or release publish)
