# CampusSphere - Project Memory & State Tracker

> Update this file after every work session. It is the single source of truth for current progress.

---

## 1. Project Overview

- **Project Name**: CampusSphere
- **Repository Branch**: `dev` (merge to `main` only after CI passes)
- **Current Phase**: Day 1 — Setup · Database · Docker (starting Mon 19 Oct)
- **Last Updated**: 2026-10-14

---

## 2. Completed Tasks

- [x] GitHub repository initialized with `main` and `dev` branches.
- [x] Base `.gitignore` (Node) and `README.md` committed.
- [x] Documentation suite created in `docs/`:
  - [PRD.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/PRD.md) — What to build, users, features, out-of-scope.
  - [Architecture.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/Architecture.md) — Monorepo structure, tech stack, data models, API reference, security.
  - [Rules.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/Rules.md) — What to use/avoid, error patterns, AI guardrails.
  - [Phases.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/Phases.md) — 10-day plan with all 70 tasks as trackable checkboxes.
  - [Design.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/Design.md) — Color tokens (light/dark), typography scale, component specs.
  - [memory.md](file:///c:/D/CLG/Hsin/CampusSphere/Campus-Sphere/docs/memory.md) — This file.

---

## 3. Currently Active Files

> Update this section whenever you start or finish working on a file.

| File | Status | Notes |
| :--- | :--- | :--- |
| `docs/memory.md` | Active | Updated after every session |
| — | — | No implementation files yet |

---

## 4. Next Actions (Day 1 — Mon 19 Oct)

Follow `Phases.md` Day 1 checklist in order:

1. **#2** Scaffold `client/` with Vite + React; install deps; set up Tailwind; create folder structure; verify dev server.
2. **#3** Scaffold `server/` with Express; install all packages; create folder structure; add `GET /health`; verify in browser.
3. **#4** Add ESLint + Prettier to both; create `.env.example` files with all variable placeholders.
4. **#5** Create Neon free DB; add `DATABASE_URL` to `server/.env`; test with `npx prisma db pull`.
5. **#6** Write full Prisma schema with all 11 models and enums.
6. **#7** Run migration; write and run seed; verify in Prisma Studio.
7. **#8** Write Dockerfiles for `client/` and `server/`.
8. **#9** Write `docker-compose.yml`; run `docker compose up --build`; confirm all services.

---

## 5. Architecture Decision Records (ADR)

| ID | Decision | Rationale |
| :--- | :--- | :--- |
| ADR-001 | **Vite + React SPA** (not Next.js) | Simpler mental model for a standalone client. Vite's HMR and build speed suit a fast 10-day build. No SSR complexity needed for this app. |
| ADR-002 | **Express REST API** (not Next.js API routes) | Separate server is independently deployable on Render with Docker. Cleaner separation of concerns. Socket.io attaches naturally to an Express HTTP server. |
| ADR-003 | **Neon PostgreSQL + Prisma** | Serverless Postgres free tier; Prisma gives type-safe queries and declarative migrations without raw SQL. |
| ADR-004 | **Render (Docker) + Vercel** | Both have generous free tiers and zero-config GitHub integration. Docker on Render ensures consistent environment from local to production. |
| ADR-005 | **Socket.io for real-time chat** | Handles WebSocket reconnection, rooms, and namespaces out of the box. JWT auth in `io.use()` integrates cleanly with existing middleware pattern. |
| ADR-006 | **Cloudinary for images** | Unsigned upload presets allow direct browser → Cloudinary uploads without routing files through the server, reducing latency and server load. API secret stays server-side via a signing endpoint. |
| ADR-007 | **No personal/sensitive info in repo** | No machine names, local paths, or personal identifiers in any committed file. Credentials only in local `.env` (gitignored). |

---

## 6. Known Issues / Blockers

> Add any issues encountered here during development.

| # | Description | Status |
| :--- | :--- | :--- |
| — | None yet | — |
