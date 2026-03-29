---
name: thynkr-codebase
description: Knowledge of the Thynkr codebase structure, conventions, and development workflow
---

# Thynkr Codebase Guide

## Repository Structure

```
thynkr/
├── apps/
│   ├── web/          → Next.js 15 frontend (port 3000)
│   │   ├── src/app/  → App Router pages (34+ pages)
│   │   ├── src/components/ → React components (20+ folders)
│   │   ├── src/hooks/ → Custom React hooks
│   │   ├── src/lib/   → Client utilities, API client
│   │   └── content/blog/ → MDX blog articles
│   └── api/          → Fastify 5 backend (port 3001)
│       ├── src/routes/ → 33 API endpoint files
│       ├── src/services/ → 25+ business logic modules
│       ├── src/models/schema.ts → Drizzle ORM schema (40+ tables)
│       ├── src/ai/ → Claude API integration
│       ├── src/seed/ → 107 seed files (subjects, concepts, questions)
│       └── src/plugins/ → Fastify plugins (redis, scheduler, auth)
├── packages/
│   └── shared/ → Shared TypeScript types, enums, Zod schemas
├── docker-compose.yaml → Local dev (4 services)
├── railway.toml → Railway deployment config
└── CLAUDE.md → Development rules and coding standards
```

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15 (App Router), React 19, Tailwind CSS 4 |
| Backend | Fastify 5, TypeScript (ESM, strict mode) |
| Database | PostgreSQL 16 + pgvector, Drizzle ORM |
| Cache | Redis 7 |
| AI | Claude API (Anthropic) for tutoring |
| Embeddings | Voyage AI for RAG pipeline |
| Auth | NextAuth.js v5 (Google OAuth + magic link via Resend) |
| Deploy | Vercel (frontend) + Railway (backend) |

## Key Conventions

- TypeScript strict mode, ESM only (`import`, never `require`)
- Server components by default in Next.js
- Drizzle ORM for all database access — never raw SQL
- Zod validation at API boundaries
- API routes registered in `apps/api/src/server.ts`
- All 33 route files follow the pattern: `export default async function routes(app: FastifyInstance)`

## Database Schema Essentials

The schema at `apps/api/src/models/schema.ts` (1683 lines) includes:
- `students` — user profiles, onboarding status, learning preferences
- `enrollments` — student-subject relationships
- `subjects` — QCE subjects (14 live)
- `concepts` — learning concepts within subjects
- `questions` — practice questions with difficulty calibration
- `practice_sessions` — session history
- `responses` — individual question responses
- `knowledge_state` — per-concept mastery levels
- `spaced_rep_schedule` — next review dates
- `misconceptions` — detected misconceptions

## Design System (Non-Negotiable)

- Dark shell + light content: navy sidebar (#1a1d2e), white cards on #f8f9fb
- Accent: violet (#7c5cfc) for primary actions
- Correct answers: soft green (#34c77b)
- Wrong answers: warm amber (#f5a623) — NEVER red, NEVER say "Wrong"
- Practice mode: immersive (no sidebar/header), max-width 720px
- Typography: 16px min body, 18px for questions
- Desktop-first responsive design

## Development Commands

```bash
# Local dev with Docker
docker compose up --build -d

# Monorepo commands
pnpm install
turbo dev         # Dev all apps
turbo build       # Build all
turbo typecheck   # Type check
turbo lint        # Lint

# Database
pnpm db:push      # Push schema to DB
pnpm db:studio    # Open Drizzle Studio
```

## Deployment

- Frontend auto-deploys to Vercel on git push
- Backend deploys to Railway (watches `apps/api/**` and `packages/shared/**`)
- Health check: `GET /health` (checks DB + Redis)
