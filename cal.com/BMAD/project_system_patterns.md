# Project System Patterns (Pointer)

> **Purpose**: Directs BMAD agents to the authoritative sources for coding standards and system patterns. This file does not duplicate content; it points to the single source of truth.

## Where to Find System Patterns

| Topic | Location | Notes |
| :--- | :--- | :--- |
| **Do / Don't rules** | [AGENTS.md](/AGENTS.md) (repository root) | Authoritative list of coding standards, PR size limits, and boundaries. |
| **Modular engineering rules** | [agents/rules/](/agents/rules/) | Quality, error handling, architecture, and feature-specific rules. |
| **Command reference** | [agents/commands.md](/agents/commands.md) | Type check, lint, test, and Prisma commands. |
| **Domain knowledge** | [agents/knowledge-base.md](/agents/knowledge-base.md) | Business rules and domain context. |

## Developer Workflow

1. **Before coding**: Read [AGENTS.md](/AGENTS.md) first. Follow its Do/Don't lists and PR guidelines.
2. **When implementing**: Use `select` (not `include`) in Prisma; put business logic in Services, not repositories.
3. **Before pushing**: Run `yarn type-check:ci --force` and `yarn biome check --write .`.

*This pointer file is valid only because the above paths exist in the repository. If you move or rename those files, update this document accordingly.*
