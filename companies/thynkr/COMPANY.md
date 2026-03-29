---
name: Thynkr
description: AI-powered adaptive learning platform for Australian QCE secondary students
slug: thynkr
schema: agentcompanies/v1
version: 0.1.0
license: MIT
authors:
  - name: Suren
goals:
  - Launch Thynkr beta and onboard first 10 active students by end of April 2026
  - Complete school pilot preparation and run first B2B pilot with a QLD school
  - Establish automated content and growth engine producing 2-3 blog posts per week and daily social content
  - Ship all P1 engineering tasks (parental consent, ATAR improvements, teacher dashboard) before pilot
  - Reach 50 active students by end of May 2026
requirements:
  secrets:
    - ANTHROPIC_API_KEY
    - GH_TOKEN
---

Thynkr is an adaptive learning platform for Australian secondary students preparing for QCE (Queensland Certificate of Education) and ATAR. It uses AI-powered tutoring, spaced repetition, and ATAR prediction to help students study smarter.

## Business Context

- **Stage**: Pre-beta, approaching first school pilot (April 2026)
- **Team**: Solo founder (Suren) + AI agent team
- **Tech stack**: Next.js 15 + Fastify 5 + PostgreSQL + Redis, deployed on Vercel (frontend) + Railway (backend)
- **Revenue model**: Freemium B2C ($19-49/mo) + B2B school licensing ($249-499/student/year)
- **Target market**: QLD Year 11-12 students (115,000 addressable), starting with Chemistry/Physics/Maths

## Current State

- 381+ commits, 14 QCE subjects live with seeded content
- Adaptive practice, AI tutoring (Claude), ATAR prediction, career chatbot all functional
- Teacher dashboard infrastructure exists but needs polish
- Blog pipeline partially automated (migrating from external system)
- Deployed on Railway (API) + Vercel (frontend)

## How This Company Operates

Suren is the board operator. He creates high-level tasks and the CEO agent decomposes them into work for the team. The primary codebase lives at the Thynkr GitHub repository. Engineering agents work directly on the codebase via git. Growth agents produce content artifacts (blog drafts, social copy, research reports) as issue comments and attachments.

All agents use `claude_local` adapter (Claude Code CLI). Engineering agents get git access to the Thynkr repository. Growth agents get web search capabilities for community research.
