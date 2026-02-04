# Developer Execution Guide

> **Status**: Active
> **Role**: Tactical Guide for Code Execution
> **Audience**: Developers / AI Agents

## 1. Start Here
Before writing code, verify you have the correct context:
1.  **What to build**: `project_product_requirements_document.md`
2.  **Where to build**: `project_architecture_document.md`
3.  **How to ship**: `project_delivery_model.md`

## 2. Source of Truth
*   **Business Logic**: MUST reside in `packages/features`. Do NOT put complex logic in React components or API routes.
*   **Database Schema**: `packages/prisma/schema.prisma` is the ONLY source of truth for data models.
*   **API Interface**: `packages/trpc` defines the internal contract.

## 3. Safe vs. Risky Areas

| Area | Risk Level | Guidelines |
| :--- | :--- | :--- |
| **`packages/features`** | **GREEN** | Safe to add new modules. Preferred location for logic. |
| **`apps/web/app`** | **GREEN** | Modern App Router. Use for new UI. |
| **`packages/prisma`** | **AMBER** | Schema changes require migrations. High impact. |
| **`apps/web/pages`** | **RED** | Legacy Pages Router. **Avoid** unless patching bugs. Do not add new features here. |
| **`packages/embeds`** | **RED** | specialized logic. Touch only if explicitly required. |

## 4. Common Pitfalls
*   **Direct DB Access**: Never import `prisma` directly in UI components. Use `trpc`.
*   **Circular Dependencies**: Tightly coupled packages. Always import from `@calcom/*` workspace packages, never relative paths across packages.
*   **Env Variables**: Defined in `.env`. Do not hardcode secrets.

## 5. Development Workflow
1.  **Context**: Read the User Story / Task.
2.  **Branch**: (Start fresh session or check out branch).
3.  **Code**:
    *   Add Schema (if needed) -> `packages/prisma`
    *   Add Logic -> `packages/features`
    *   Add Router -> `packages/trpc`
    *   Add UI -> `apps/web/app`
4.  **Verify**:
    *   `yarn lint` (Must pass)
    *   `yarn build` (Must pass)
    *   `yarn test` (Unit tests for features)
5.  **Document**: Update `project_architecture_document.md` if you added a new package or key service.

## 6. Testing Strategy
*   **Unit Tests**: Required for all logic in `packages/features`. File pattern: `*.test.ts`.
*   **E2E Tests**: Located in `apps/web/playwright`. Run only on major flows.
