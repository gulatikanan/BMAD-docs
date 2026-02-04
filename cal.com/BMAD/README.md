# BMAD Documentation Layer

This directory contains context files specifically optimized for **BMAD (Building Multi-Agent Development)** agents.

## ⚠️ Source of Truth
Crucially, this `cal.com` repository already contains extensive documentation in:
*   **`AGENTS.md`** (Root): **AUTHORITATIVE** instructions for all agents.
*   **`agents/`** (Root): Detailed rules and commands.
*   **`docs/`**: Official project documentation.

**BMAD Agents must defer to `AGENTS.md` for coding standards, pattern constraints, and specific command usage.**

---

## Getting Started

### Setup / Installation

1. **Clone and install** (from repository root):
   ```sh
   git clone https://github.com/calcom/cal.com.git
   cd cal.com
   yarn
   ```

2. **Environment**: Copy `.env.example` to `.env`, set `NEXTAUTH_SECRET` and `CALENDSO_ENCRYPTION_KEY` (see root [README.md](/README.md) for generation commands).

3. **Database**: Set `DATABASE_URL` in `.env`, then run:
   ```sh
   yarn workspace @calcom/prisma db-migrate
   ```

4. **Quick start (with Docker)**: From root, run `yarn dx` to start Postgres and the app; see root README for default credentials.

5. **Run the app**: `yarn dev` — web app at [http://localhost:3000](http://localhost:3000).

*Full setup details, prerequisites (Node ≥18, PostgreSQL ≥13, Yarn), and deployment options are in the root [README.md](/README.md) under "Getting Started" and "Development".*

---

## Documentation Map

All files listed below **exist** in this folder. The pointer strategy is valid only when the pointed-to targets exist; both pointer files and their targets are maintained in this repo.

| File | Audience | Purpose |
| :--- | :--- | :--- |
| **`active_context.md`** | All Agents | Tracks the *current* session state, phase, and immediate goals. |
| **`project_product_requirements_document.md`** | Product Manager | Baseline PRD representing the existing system capabilities (Scheduling, Teams, App-Store). Gives context to technical decisions. |
| **`project_architecture_document.md`** | Architect | Maps the Monorepo structure (`apps/*` vs `packages/*`) and Data Flow. |
| **`project_system_patterns.md`** | Developer / Architect | **POINTER** file. Directs agents to `AGENTS.md` and `agents/rules/`. |
| **`project_tech_context.md`** | Developer | **POINTER** file. Summarizes the stack (Next.js, tRPC, Prisma) and points to `package.json`. |
| **`api_reference.md`** | Developer | Concrete API v2 endpoints with request/response examples (bookings, event types). |

---

## API Documentation

For **concrete endpoints and request/response examples** (not just high-level descriptions), use:

- **BMAD API reference**: [api_reference.md](./api_reference.md) — selected v2 endpoints (e.g. `POST /v2/bookings`, `GET /v2/event-types`) with example request and response bodies.
- **Full OpenAPI spec**: [docs/api-reference/v2/openapi.json](/docs/api-reference/v2/openapi.json).
- **Published docs**: [Cal.com API v2](https://cal.com/docs/api-reference/v2/introduction) (authentication, rate limits, all endpoints).

---

## How to use
1.  **Product Manager**: specific tasks will require creating *new* PRDs or updating `active_context.md`. Use `project_product_requirements_document.md` as context for what currently exists.
2.  **Architect**: Use `project_architecture_document.md` to understand boundaries. When designing new features, ensuring they fit into `packages/features` boundaries is critical.
3.  **Developer**: **MUST READ `AGENTS.md` FIRST.** The `project_tech_context.md` is a quick summary, but `AGENTS.md` contains the specific `Do` and `Don't` lists. Use `api_reference.md` for API usage examples.
