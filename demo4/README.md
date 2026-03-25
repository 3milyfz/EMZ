# Release Demo 4 – Manifest

This README is the high-level manifest for **Release Demo 4** (Alpha Release Validation & Feature Lock). It provides a direct link to the live-deployed application and indexes the required artifacts included in this release submission repo.

## Live deployed application

- **Production web app URL:** https://cultivate-fe.vercel.app/

---

## Demo 4 artifacts

All release deliverables for Demo 4 are under **`demo4/`**:

| Artifact | Description |
|----------|-------------|
| [demo4/feature-lock-justification.md](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/feature-lock-justification.md) | Documents why the current feature set is sufficient for the alpha release, with justification tied to core workflow readiness, reduced friction, and feature freeze reasoning. |
| [demo4/alpha-validation-evidence.md](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/alpha-validation-evidence.md) | Provides evidence that the system meets alpha release criteria, including feature completeness, internal stability, repeatable testing, and functional security, operations, and reliability controls. |
| [demo4/evolved-topology.jpg](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/evolved-topology.jpg) | Updated system topology diagram showing the most important architectural evolution in the product and its supporting infrastructure. |
| [demo4/pivot-contract.md](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/pivot-contract.md) | Pre-sprint pivot contract defining the hypothesis, kill metric, trigger date, and fallback options for this release cycle. |
| [demo4/build-trap-postmortem.md](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/build-trap-postmortem.md) | Retrospective evaluating whether the implemented changes delivered the expected value and whether building them was necessary to validate the underlying assumptions. |
| [demo4/README.md](https://github.com/3milyfz/EMZ/blob/v1.0.6/demo4/README.md) | Demo 4 release index and manifest. |

---

## Release focus

Demo 4 marks the transition from feature expansion into **alpha release validation**. The goal of this release is to determine whether the product is sufficiently complete, internally stable, and operationally reliable to justify a **feature lock** before beta development.

This release focuses on validating that the current system can support the core user experience end-to-end without placeholder flows, mock logic, or fatal execution failures. It also evaluates whether the team has the minimum security, logging, error handling, and recovery controls expected of an alpha-stage product.

### Core goals in scope
- confirm that primary workflows are feature-complete
- validate that the system is stable during internal testing
- justify feature freeze for the alpha milestone
- confirm readiness to move from build mode into controlled refinement

### Key validation areas
- feature completeness
- internal stability
- test repeatability
- basic error handling
- security controls
- operations visibility
- reliability and recovery behaviour

---

## Alpha release position

This release is centered on the question: **Is the product ready for feature lock?**

Rather than introducing a new interaction model, Demo 4 is about validating that the current product state is strong enough to freeze the alpha scope. The emphasis is on confirming that the implemented workflows can be executed consistently, that the system behaves predictably under internal use, and that major technical and product risks are visible and manageable.

The output of this release is therefore not only a set of updated artifacts, but also a release decision framework: whether the team should proceed with the current feature set into the next phase or continue alpha iteration.

---

## Monorepo overview

- **Root (`./`)**  
  Workspace configuration and shared tooling. NPM scripts run the full stack locally.

- **Frontend (`pkgs/app`)**  
  React + TypeScript + Vite single-page application containing the user-facing product experience.

- **Backend API (`pkgs/server`)**  
  Hono + TypeScript HTTP API server backed by MongoDB/Mongoose. Supports listings, users, chat-related flows, uploads, and other application logic.

- **TypeScript SDK (`pkgs/sdk`)**  
  Generated API client used by the frontend to interact with backend endpoints and contracts.

---

## What changed in Demo 4

The major change in this release is a shift in emphasis from **building features** to **validating release readiness**.

In earlier demos, the focus was on proving product value, reducing competitive friction, and evolving the interaction model. In Demo 4, the focus becomes:
- validating whether the implemented features are sufficient
- confirming that primary workflows work reliably end-to-end
- evaluating whether the system is ready for feature freeze
- documenting whether alpha criteria are met or whether further iteration is still required

This makes Demo 4 a **release-readiness checkpoint**, not just another build sprint.

---

## Local development

**Install dependencies** (from repo root):

```bash
npm install