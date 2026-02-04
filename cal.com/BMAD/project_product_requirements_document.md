# Product Requirements Document (PRD) — Cal.com Baseline

> **Status**: Verified As-Is (Brownfield)
> **Date**: 2026-02-03
> **Purpose**: Baseline PRD representing existing system capabilities. Provides context for technical decisions and feature work. For new features, create or extend PRDs as needed.

## 1. Product Vision

Cal.com delivers **scheduling infrastructure for everyone**: self-hosted or hosted, white-label by design, API-driven, with full control of events and data. It positions as the open-source Calendly successor.

## 2. Goals & Success Criteria

| Goal | Success Indicator |
| :--- | :--- |
| **Scheduling as a Service** | Hosts can create event types; attendees can book without friction; availability and timezone handling are correct. |
| **Extensibility** | App Store integrations (calendars, video, CRM, payments) plug into a standard interface. |
| **Enterprise readiness** | Organizations, SSO, SCIM, and admin/impersonation support B2B use cases. |
| **Developer platform** | Public API (v2) and tRPC enable integrations and custom booking flows. |

## 3. User Personas & Needs

| Persona | Primary Need | Key Capabilities |
| :--- | :--- | :--- |
| **Host** | Reliable scheduling and control | Event types, availability, calendar sync, workflows (email/SMS/webhooks). |
| **Attendee** | Easy, low-friction booking | Public booking page, timezone clarity, confirmations. |
| **Team Admin** | Group scheduling and routing | Teams, round-robin, collective availability. |
| **Developer** | Integrate or extend Cal.com | API v2 (REST), OAuth, event types, bookings, availability. |

## 4. Core Product Capabilities (Current)

### 4.1 Scheduling & Booking
- **Event types**: Configurable (duration, locations, custom fields, payment, recurrence).
- **Availability**: User schedules, overrides, buffers, travel time.
- **Assignment**: Round-robin (weighted), collective availability, manual.
- **Post-booking**: Emails, webhooks, SMS reminders, workflow automation.

### 4.2 Event Configuration
- **EventType**: Central entity; multiple locations (in-person, link, phone, conference); custom intake; payment; recurring rules.

### 4.3 App Store Ecosystem
- **Categories**: Calendars (two-way sync), conferencing (video links), CRM, analytics.
- **Integration model**: Standardized app interface in `packages/app-store`.

### 4.4 Governance & Administration
- **RBAC**: User, Admin, Team Admin.
- **Impersonation**: Admin can act as user for support.
- **Audit**: Booking changes and cancellations logged.

## 5. Out of Scope (Baseline)

- Custom business logic not represented in the codebase.
- Features guarded under `packages/features/ee` without an active license (behavior may vary).

## 6. Technical Context for Decisions

- **Monorepo**: Turborepo; `apps/web` (Next.js), `apps/api/v2` (NestJS), `packages/features` (domain), `packages/prisma` (data).
- **Data model**: `EventType` is central and large (~150 fields/relations); changes carry high regression risk.
- **API**: Internal tRPC; public REST API v2; v1/v2 migration in progress.
- **Integrations**: Heavy reliance on third-party APIs (Google, Zoom, Stripe); failures affect stability.

*This PRD is the baseline for the existing product. When proposing new features or refactors, reference this document and extend or add PRDs as needed so technical decisions are traceable to product context.*
