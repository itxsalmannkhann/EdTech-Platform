# Contributing to EduAI

Thanks for your interest in contributing! This document explains how to get involved.

## Code of Conduct

By participating, you agree to uphold our [Code of Conduct](./CODE_OF_CONDUCT.md).

## Getting Started

1. Fork the repo and clone your fork.
2. Install dependencies: `npm install`.
3. Copy `.env.example` to `.env` and fill in values.
4. Create a branch (see naming below).
5. Make your changes and ensure all quality gates pass.
6. Open a pull request.



## Branch Naming

Use the pattern `<type>/<short-description>`:

- `feat/ai-quiz-generator`
- `fix/login-redirect`
- `docs/update-readme`
- `chore/bump-deps`
- `refactor/course-service`
- `test/enrollment-flow`

## Conventional Commits

All commits **must** follow the [Conventional Commits](https://www.conventionalcommits.org/) spec:

```
<type>(<scope>): <description>

[optional body]
[optional footer]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

**Examples:**

```
feat(auth): implement secure authentication
feat(ai): add AI tutor streaming endpoint
fix(api): resolve registration race condition
docs(readme): update deployment guide
```

## Code Style

- TypeScript everywhere; avoid `any`.
- Run `npm run format` (Prettier) and `npm run lint` (ESLint) before committing.
- Keep components small, reusable, and accessible (WCAG AA).
- Prefer Server Components; use `"use client"` only when needed.
- Co-locate feature code; share primitives in `src/components/ui`.

## Testing Rules

- Add unit tests (Vitest) for utilities and business logic.
- Add e2e tests (Playwright) for critical user flows.
- All tests must pass before a PR is merged.

## Quality Gate (must pass before commit)

```bash
npm run format
npm run lint
npm run typecheck
npm run test
npm run build
```

## Pull Request Process

1. Ensure your branch is up to date with `main`.
2. Fill out the PR template (summary, what changed, how tested).
3. Link any related issues.
4. At least one approving review is required.
5. Squash-merge with a Conventional Commit title.

## Reporting Issues

Use the issue templates. Include reproduction steps, expected vs. actual behavior, and environment details.
