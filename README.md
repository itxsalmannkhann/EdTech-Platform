<div align="center">

# 🎓 EduAI — AI-Powered EdTech Platform

**Learn anything, powered by AI.** A modern, full-stack online courses platform with an AI tutor, smart quizzes, and personalized learning — built to deploy entirely on free-tier infrastructure.

[![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Prisma](https://img.shields.io/badge/Prisma-6-2D3748?logo=prisma)](https://www.prisma.io/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-4-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)

</div>

---

## ✨ Features

### 🤖 AI-Powered Learning
- **AI Tutor** — streaming chat assistant for explanations, study plans, and practice
- **AI Quiz Generator** — turn any lesson into a practice quiz
- **Smart Recommendations** — personalized course suggestions
- **Learning Analytics** — progress tracking and completion prediction
- Works with **free** OpenAI-compatible models (OpenRouter, Gemini)

### 📚 Courses
- Categories, levels, reviews & ratings
- Video / article / PDF / quiz / assignment / coding lessons
- Sections & curriculum builder, progress tracking, certificates
- Wishlist, cart, and Stripe (test mode) checkout
- Announcements, discussions, and notes

### 👥 Roles
- **Student** — learn, track progress, earn certificates
- **Instructor** — create & manage courses, view analytics
- **Admin / Super Admin** — manage users, approve courses, audit logs

### 🔐 Authentication & Security
- NextAuth (JWT) with credentials + Google/GitHub OAuth ready
- Role-based access control (RBAC) and protected routes
- Rate limiting, security headers, input validation with Zod

---

## 🏗️ Architecture

```
Next.js 15 (App Router, RSC) ──> Route Handlers (API) ──> Prisma ──> PostgreSQL (Neon)
        │                              │
        │                              ├──> Upstash Redis (rate limit / cache)
        │                              ├──> OpenAI-compatible AI (OpenRouter / Gemini)
        │                              ├──> Stripe (payments, test mode)
        │                              ├──> Cloudinary (media)
        │                              └──> Resend (email)
        └──> NextAuth (JWT, RBAC)
```

See [ARCHITECTURE.md](./ARCHITECTURE.md) for details.

---

## 📁 Folder Structure

```
src/
├── app/                    # App Router pages & route handlers
│   ├── (marketing)/        # Public pages (landing, courses, pricing, ...)
│   ├── (auth)/             # Login & register
│   ├── admin/              # Admin dashboard
│   ├── instructor/         # Instructor dashboard
│   ├── dashboard/          # Student dashboard
│   └── api/                # Route handlers (auth, register, ai, ...)
├── components/             # UI primitives & feature components
│   ├── ui/                 # Button, Card, Input, ... (shadcn-style)
│   ├── layout/             # Navbar, Footer, DashboardShell
│   ├── auth/ ai/ courses/  # Feature components
│   └── marketing/
├── lib/                    # Core libs (auth, prisma, ai, rbac, env, utils)
├── server/                 # Server-only data access
├── config/                 # Site config
└── generated/              # Prisma client (generated)
prisma/                     # Schema & seed
```

---

## 🚀 Installation

> **Prerequisites:** Node.js 20+, npm, and a [Neon](https://neon.tech) Postgres database (free).

```bash
# 1. Clone
git clone https://github.com/itxsalmannkhann/EdTech-Platform.git
cd EdTech-Platform

# 2. Install
npm install

# 3. Configure environment
cp .env.example .env   # then fill in the values

# 4. Set up the database
npm run db:push        # push schema to your Neon database
npm run db:seed        # load sample courses & demo users

# 5. Run
npm run dev            # http://localhost:3000
```

### Demo accounts (after seeding)

| Role       | Email                   | Password      |
| ---------- | ----------------------- | ------------- |
| Admin      | `admin@eduai.com`       | `Password123` |
| Instructor | `instructor@eduai.com`  | `Password123` |
| Student    | `student@eduai.com`     | `Password123` |

---

## 🧑‍💻 Development

```bash
npm run dev          # start dev server
npm run typecheck    # TypeScript check
npm run lint         # ESLint
npm run format       # Prettier
npm run test         # Vitest unit tests
npm run test:e2e     # Playwright e2e tests
npm run db:studio    # Prisma Studio
```

---

## ☁️ Deployment

Deploy free on **Vercel** + **Neon** + **Upstash**. See [DEPLOYMENT.md](./DEPLOYMENT.md) for the full guide.

1. Push to GitHub
2. Import the repo into Vercel
3. Add environment variables (see [ENVIRONMENT.md](./ENVIRONMENT.md))
4. Vercel runs `npm run build` (which runs `prisma generate`) automatically

---

## 🔑 Environment Variables

All variables are documented in [`.env.example`](./.env.example) and [ENVIRONMENT.md](./ENVIRONMENT.md). Every required service has a free tier.

---

## 🧰 Tech Stack

| Layer       | Technology                                              |
| ----------- | ------------------------------------------------------- |
| Framework   | Next.js 15 (App Router), React 19, TypeScript           |
| Styling     | Tailwind CSS v4, shadcn-style UI, Framer Motion         |
| Data        | Prisma ORM, PostgreSQL (Neon)                           |
| Auth        | NextAuth (JWT, RBAC)                                    |
| AI          | OpenAI-compatible SDK (OpenRouter / Gemini free models) |
| Infra       | Upstash Redis, Cloudinary, Resend, Stripe (test)        |
| Forms       | React Hook Form + Zod                                   |
| State/Query | TanStack Query                                          |
| Testing     | Vitest, Playwright                                      |
| Tooling     | ESLint, Prettier, Husky, lint-staged                    |

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) and our [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md).

## 🗺️ Roadmap

See [ROADMAP.md](./ROADMAP.md) for what's planned.

## ❓ FAQ

See the in-app [FAQ](http://localhost:3000/faq) or [ROADMAP.md](./ROADMAP.md).

## 📄 License

[MIT](./LICENSE) © EduAI

## 🙏 Acknowledgements

Built with Next.js, Prisma, NextAuth, Tailwind CSS, and the open-source community. Course images from [Unsplash](https://unsplash.com).
