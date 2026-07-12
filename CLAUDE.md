# CLAUDE.md

Guidance for AI coding assistants (Claude, etc.) working in this repository.

## Project

EduAI — an AI-powered EdTech platform. Next.js 15 (App Router) + React 19 +
TypeScript + Prisma (PostgreSQL/Neon) + NextAuth + Tailwind v4. Deployable on
free-tier services.

## Commands

```bash
npm run dev         # dev server
npm run build       # prisma generate + next build
npm run typecheck   # tsc --noEmit
npm run lint        # eslint
npm run format      # prettier
npm run test        # vitest
npm run db:push     # sync schema to DB
npm run db:seed     # seed sample data
```

## Architecture (read first)

- `src/app` — App Router. Route groups: `(marketing)`, `(auth)`. Dashboards at
  `dashboard/`, `instructor/`, `admin/`. API in `app/api`.
- `src/lib` — core services: `auth`, `prisma`, `ai`, `rbac`, `rate-limit`,
  `env`, `utils`. Import via the `@/` alias.
- `src/server` — server-only data access (`import "server-only"`).
- `src/components/ui` — shadcn-style primitives. Reuse these.
- `prisma/schema.prisma` — data model. Generated client → `src/generated/prisma`.

See [ARCHITECTURE.md](./ARCHITECTURE.md) for the full picture.

## Conventions

- TypeScript strict; avoid `any`. Generate types from Prisma where possible.
- Prefer **Server Components**; add `"use client"` only when needed.
- Validate all external input with **Zod** at API boundaries.
- Enforce auth with `requireUser` / `requireRole` from `src/lib/rbac.ts`.
- Optional integrations are **feature-flagged** (`src/lib/env.ts`) — never assume
  a key exists; degrade gracefully.
- Styling via Tailwind utilities and design tokens in `globals.css`. Keep
  components accessible (WCAG AA): labels, focus states, ARIA.
- **Never hardcode secrets.** Use env vars only.

## Workflow

1. Make focused changes; reuse existing components/services.
2. Run the quality gate: `format → lint → typecheck → test → build`.
3. Use **Conventional Commits** (see [CONTRIBUTING.md](./CONTRIBUTING.md)).
4. Update `CHANGELOG.md` and docs when behavior changes.
5. Do not commit broken code; never force-push or commit secrets.

## Gotchas

- Prisma client output is `src/generated/prisma` (not the default). Run
  `npm run db:generate` after schema edits.
- Middleware is edge-safe and only checks the session cookie; do real role checks
  server-side.
- The AI client is OpenAI-compatible; it targets OpenRouter/Gemini free models.
