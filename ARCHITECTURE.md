# Architecture

This document describes the high-level architecture of EduAI.

## Overview

EduAI is a full-stack application built on **Next.js 15 (App Router)**. It uses
React Server Components for data-heavy pages, Route Handlers for the API layer,
and Prisma for type-safe database access against PostgreSQL.

```
┌──────────────────────────────────────────────────────────────┐
│                        Browser (React 19)                       │
│   Server Components · Client Components · TanStack Query         │
└───────────────┬────────────────────────────────────────────────┘
                │ HTTP / streaming
┌───────────────▼────────────────────────────────────────────────┐
│                    Next.js 15 (App Router)                       │
│  ┌───────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Server Compo- │  │ Route Handlers│  │ Middleware (RBAC      │  │
│  │ nents (RSC)   │  │ (/api/*)      │  │ route protection)     │  │
│  └──────┬────────┘  └──────┬───────┘  └──────────────────────┘  │
│         │                  │                                     │
│  ┌──────▼──────────────────▼───────────────────────────────┐    │
│  │                    lib/ (core services)                  │    │
│  │  auth · prisma · ai · rbac · rate-limit · env · utils    │    │
│  └──────┬───────────────────────────────────────────────────┘    │
└─────────┼─────────────────────────────────────────────────────┘
          │
   ┌──────┼───────────────┬───────────────┬──────────────┬─────────┐
   ▼      ▼               ▼               ▼              ▼         ▼
 Neon   Upstash       OpenRouter/      Stripe        Cloudinary  Resend
(Postgres) Redis        Gemini        (payments)      (media)    (email)
```

## Layers

### 1. Presentation (`src/app`, `src/components`)
- **Route groups**: `(marketing)`, `(auth)` for layout isolation.
- **Server Components** fetch data directly via `src/server/*`.
- **Client Components** (`"use client"`) handle interactivity, forms, and
  streaming UIs.
- **Design system** lives in `src/components/ui` (shadcn-style) plus
  `globals.css` design tokens.

### 2. Application / API (`src/app/api`, Route Handlers)
- RESTful Route Handlers for registration, AI, and (future) course/commerce
  operations.
- Each handler validates input with **Zod**, applies **rate limiting**, and
  enforces **RBAC** where needed.

### 3. Domain & Services (`src/lib`, `src/server`)
- `auth.ts` — NextAuth config (JWT strategy, providers, callbacks).
- `rbac.ts` — role hierarchy and guards (`requireUser`, `requireRole`).
- `prisma.ts` — singleton Prisma client.
- `ai.ts` — OpenAI-compatible client with streaming support.
- `rate-limit.ts` — Upstash sliding-window limiters (no-op without Redis).
- `env.ts` — type-safe env access + feature flags.
- `server/*.ts` — server-only data access functions.

### 4. Data (`prisma/`)
- PostgreSQL via Prisma. See `prisma/schema.prisma`.
- Generated client output to `src/generated/prisma`.

## Key Decisions

- **JWT sessions** keep auth edge-friendly and stateless; middleware checks for
  the session cookie, while fine-grained role checks happen server-side.
- **Graceful degradation**: optional integrations (AI, Redis, Stripe, email) are
  feature-flagged so the app boots and renders even when keys are absent.
- **Server-first data fetching** reduces client JS and improves performance/SEO.
- **Free-tier first**: every external dependency has a generous free plan.

## Rendering Strategy

- Marketing pages: static where possible, dynamic for data-backed listings.
- Dashboards: dynamic, authenticated Server Components.
- AI endpoints: Node runtime with streaming responses.

## Security

See [SECURITY.md](./SECURITY.md). Highlights: RBAC, Zod validation, rate
limiting, security headers, bcrypt password hashing, secrets via env only.
