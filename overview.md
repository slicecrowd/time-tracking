# App High‑Level Overview & Implementation Plan (v1.0)

This document describes the full product vision, features, architecture, and implementation strategy for the Mac + Web time tracking app.

---

## 1) Product Scope & Roles
[... full Section 1 content ...]

## 2) Core Feature Set
[... full Section 2 content ...]

## 3) Non‑Functional Requirements
[... full Section 3 content ...]

## 4) Architecture (Modular Monolith now; integrate microservices later)
[... full Section 4 content ...]

## 5) Tech Stack Choices
[... full Section 5 content ...]

## 6) Environments & DevOps
[... full Section 6 content ...]

## 7) Data Model (initial draft)
[... full Section 7 content ...]

## 8) External API (Partner‑facing, plan for v1)
[... full Section 8 content ...]

## 9) Email & Notifications
[... full Section 9 content ...]

## 10) Payments & Plans
[... full Section 10 content ...]

## 11) AI Implementation Plan
[... full Section 11 content ...]

## 12) Desktop Packaging (.dmg)
[... full Section 12 content ...]

## 13) Security Checklist
[... full Section 13 content ...]

## 14) Testing & QA
[... full Section 14 content ...]

## 15) Roadmap (lean, shippable milestones)
[... full Section 15 content ...]

## 16) Cursor — Indexing & Docs
This section provides a **summary version** of the whole plan. 
- You only need this if you *don’t* want to index the entire document. 
- Since you’re making this repo public, **just point Cursor’s Indexing & Docs to this file (`overview.md`)** and Cursor will have the whole plan.

**Project Overview**
- This repo contains: web app (Next.js), desktop app (Tauri), shared UI, DB schema, and a REST API under `/api/v1`.
- Architectural choices: modular monolith; Postgres via Prisma; Supabase Auth; Stripe Billing; Resend/Postmark for email.

**Repository Map**
- `apps/web`: User‑facing web UI, admin/manager panels, internal route handlers.
- `apps/desktop`: Tauri app with Rust commands for macOS capture and React UI that shares components from `packages/ui`.
- `packages/db`: Prisma schema/migrations; generated client.
- `packages/ui`: shadcn/ui components, Tailwind styles.
- `packages/core`: domain services, validation, RBAC guards.
- `packages/types`: shared TypeScript types.
- `packages/sdk-js`: generated API client; keep version‑locked to OpenAPI.

**Conventions**
- TypeScript strict mode; Zod validation at API boundaries.
- Conventional Commits; Changesets for versioning.
- All server writes require `teamId` and membership checks.

**Runbook**
- Dev web: `pnpm -w dev` (Vercel env vars needed).
- Dev desktop: `pnpm -w tauri dev`.
- Migrate DB: `pnpm -w prisma migrate dev`.
- Seed data: `pnpm -w seed` (creates default statuses and demo project/tasks).

---

## 17) Cursor — Rules & Memories
Paste this section manually into Cursor → Rules & Memories for each agent/tab.

**Rules**
1. Favor simplicity and reuse across web + desktop (shared `packages/ui`).
2. Never bypass RBAC: every mutation checks membership & role.
3. All DB writes include `teamId` (or `null` for Individual scope) and `ownerId`.
4. Do not store raw window titles or file paths that contain emails/URLs with tokens—run `redactSensitive()` first.
5. Keep API idempotent: client can safely retry writes with `x-idempotency-key`.
6. Exports must stream; avoid loading large reports fully in memory.
7. Stick to Prisma for data access; no raw SQL unless indexed migration requires it.
8. When adding statuses, update seed, enum, validations, and test fixtures.
9. All external calls (Stripe, email, AI) go through `packages/core/services/*`.
10. Log token usage for every AI call; attach to `AIUsageEvent` for billing.

**Memories**
- Platform: Mac desktop + web.
- Time tracking modes: Day Overview, Task.
- Providers: Supabase (DB/Auth/Storage), Vercel (web/API), Stripe (billing), Resend/Postmark (email).
- Distribution: signed & notarized `.dmg` via Tauri.
- Partner API: `/api/v1` REST with API keys and webhooks.

---

## 18) Answers to Key Questions
[... Section 18 content ...]

## 19) Risks & Assumptions
[... Section 19 content ...]

## 20) Next Steps
[... Section 20 content ...]

---
