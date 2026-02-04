# Project Tech Context (Pointer)

> **Purpose**: Quick technical summary for BMAD developers. For full standards and commands, **MUST READ [AGENTS.md](/AGENTS.md) first.**

## Stack Summary

| Layer | Technology |
| :--- | :--- |
| **Framework** | Next.js 13+ (App Router in parts), NestJS for API v2 |
| **Language** | TypeScript (strict) |
| **Database** | PostgreSQL with Prisma ORM |
| **API** | tRPC (internal), REST (public API v2 in `apps/api/v2`) |
| **Auth** | NextAuth.js |
| **Styling** | Tailwind CSS |
| **Testing** | Vitest (unit), Playwright (E2E) |

## Key File References

| Resource | Path |
| :--- | :--- |
| **Root package.json** | [package.json](/package.json) — scripts, workspaces |
| **Database schema** | [packages/prisma/schema.prisma](/packages/prisma/schema.prisma) |
| **tRPC routers** | [packages/trpc/server/routers/](/packages/trpc/server/routers/) |
| **Web routes** | [apps/web/app/](/apps/web/app/) (App Router) |
| **API v2** | [apps/api/v2/](/apps/api/v2/) |

## Pointer Rule

This file summarizes the stack only. **All coding standards, Do/Don't rules, and command references live in [AGENTS.md](/AGENTS.md) and [agents/](/agents/).** Do not rely on this file alone for implementation decisions.

*This pointer file is valid only because the above paths exist. If the repo structure changes, update this document.*
