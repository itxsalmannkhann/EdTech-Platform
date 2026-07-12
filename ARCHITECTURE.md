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

