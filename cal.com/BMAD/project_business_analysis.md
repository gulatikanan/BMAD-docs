# Business Analysis (Current State)

> **Status**: Verified As-Is
> **Date**: 2026-02-03
> **Scope**: Brownfield Codebase Analysis (`apps/web`, `packages/*`, `schema.prisma`)

## 1. System Overview
Cal.com is a mature, infrastructure-grade scheduling platform. It operates as a monorepo offering "Scheduling as a Service." The system is functionally comparable to Calendly but differentiates via:
*   **Open Source Core**: Self-hostable via Docker/Node.
*   **Extensibility**: A modular Event Type system and App Store.
*   **Enterprise Features**: Native support for Organizations, SSO, and SCIM.

## 2. Business Model & Monetization
The codebase supports a hybrid business model (Open Core + SaaS):

*   **PLG (Product-Led Growth)**:
    *   **Free/Pro Tier**: Individual users (User model has `plan` attributes).
    *   **Teams**: Small groups using shared event types and round-robin scheduling.
*   **Enterprise (B2B)**:
    *   **Organizations**: Hierarchical structure (`Team` model with `isOrganization=true`) supporting managed users and verified domains.
    *   **Licensing**: Code in `packages/features/ee` indicates guarded functionality potentially requiring a license key.
*   **Platform/Marketplace**:
    *   Value generation via the App Store ecosystem (`packages/app-store`).
    *   Payment processing capabilities (Stripe/PayPal apps) suggest revenue share or facilitating paid bookings for users.

## 3. Core Functional Domains
Based on `packages/features` and `schema.prisma`:

### 3.1 Booking Engine (The Core)
*   **Availability Logic**: Handles complex constraints (User Schedules, Overrides, Travel Time, Buffers).
*   **Assignment Logic**: Supports Round-Robin (Weighted), Collective availability, and Manual assignment.
*   **Workflow**: Post-booking automation (Emails, Webhooks, SMS reminders).

### 3.2 Event Configuration
*   **EventType**: The central configuration object. Supports:
    *   Multiple locations (In-person, Link, Phone, Conference).
    *   Custom intake forms (`EventTypeCustomInput`).
    *   Payment requirements (Price/Currency).
    *   Recurring rules.

### 3.3 App Store Ecosystem
*   **Integration Framework**: A standardized interface in `packages/app-store` allowing varied apps (Calendar, Video, CRM) to plug into the booking flow.
*   **Categories**:
    *   **Calendars**: Two-way sync (Google, Outlook, Apple).
    *   **Conferencing**: Auto-generation of video links.
    *   **CRM**: Contact creation on booking.
    *   **Analytics**: Tracking pixels (GA4, Fathom).

### 3.4 Governance & Administration
*   **RBAC**: Role-Based Access Control (User, Admin, Team Admin).
*   **Impersonation**: Admin capability to act as users for support/setup.
*   **Audit**: Logging of booking changes/cancellations.

## 4. User Segments
1.  **Host (The Customer)**: The user creating schedulable links. Prioritizes control, calendar sync reliability, and workflow automation.
2.  **Attendee (The Guest)**: The end-user booking time. Prioritizes ease of use, friction-less booking, and timezone clarity.
3.  **Team Admin**: Manages group availability and routing rules.
4.  **Developer**: Consumes the Platform API (`apps/api/v2`) or builds App Store extensions.

## 5. Known Risks & Constraints
*   **Complexity Risk**: The `EventType` model is massive (approx. 150 fields/relations). Changes to widespread scheduling logic carry high regression risk.
*   **Integration Dependency**: Core value relies on 3rd party APIs (Google, Zoom, Stripe). API breakages in these providers can degrade system stability.
*   **Migration Risk**: The split between `API_V1` and `API_V2` (visible in enums) suggests an ongoing or incomplete migration path.
*   **Data Volume**: Tables like `Booking` and `Webhook` will grow rapidly; long-term scalability of the single Postgres instance is a constraint.

## 6. Explicit Unknowns
*   **Current License Status**: While `packages/features/ee` exists, the specific active license enforcement mechanism is opaque in a static scan.
*   **Legacy Code**: The ratio of Pages Router vs. App Router usage in `apps/web` is mixed, making frontend refactoring estimates uncertain.
*   **External Services**: Reliance on specific external microservices (e.g., for PDF generation or heavy processing) is not fully visible in the repo scan.
