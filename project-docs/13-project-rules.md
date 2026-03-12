# Project Rules

## Scope Discipline
- Build only in-scope MVP items unless scope is updated.
- Postpone non-essential integrations until foundation is stable.

## Architecture Discipline
- Keep stack simple: Next.js + Supabase + Vercel.
- Use path-based tenancy until core workflows are stable.
- Keep RLS and tenant_id model central to all data work.

## Defect and Validation Discipline
- Do not implement bug fixes without a reproducible case.
- Prove root cause before applying code changes.
- Validate fixes with targeted checks plus broader safety checks.
- For security and tenancy-sensitive areas, verify 2 to 3 times with different contexts.

## Delivery Discipline
- Every schema change must have a migration.
- Every permission-sensitive feature must include access checks.
- Every meaningful session must update AI worklog.

## Reusable Implementation Rules
- Reuse permission services and avoid module-specific auth duplication.
- Reuse workflow engine/state machine for publish/approval scenarios.
- Reuse table/filter/export and notification-template patterns.
- Prefer centralized constants/enums/template keys over repeated literals.
- New features must extend documented shared structures before creating new ones.


## Engineering Rules: Reusable Engine Architecture
- Implement modules as interoperable engines/services, not isolated silos.
- Favor low-code/admin-configurable behavior where feasible.
- Keep new modules isolated by domain but reusable by interface.
- Minimize code duplication by extending existing workflow/permission/template/audit services.


## Drupal-Like Modular Engineering Rules
- maintain a reusable module registry for optional capabilities
- centralize entitlement and permission resolution
- centralize subscription/billing lifecycle logic
- use low-code/UI-driven configuration for offers, bundles, and pricing where feasible
- avoid hardcoded tenant-specific monetization behavior


## Drupal-Like Modular Architecture Enforcement
- implement plan/add-on logic through centralized services
- keep module activation/deactivation policy-driven
- preserve data on module disable unless explicit retention policy says otherwise
- use shared engines: permission, workflow, templates, notifications, entitlements, audit, export, calendar


## Engineering Rules for Platform Capabilities
- implement platform concerns through shared engines (queue, integrations, rate limiting, observability)
- avoid per-module custom implementations of the same platform concern
- schema proposals must map to documented capability layers before SQL work starts
- demo, staging, and production behavior must be explicitly separated by environment policy


## Implementation Rules: Resolution, Dependency, and Consistency
- use one canonical permission-inheritance policy source
- enforce one canonical feature-resolution order across backend/frontend
- block activation of add-ons when required dependencies are unmet
- treat config/version rollback as first-class admin operation
- require idempotency keys for retry-prone operations (webhooks/jobs/imports)


## Governance Precedence Rule (Implementation)
Implementation rules must follow ownership and precedence from `19-doc-governance-and-boundaries.md`.


## Naming and KPI Consistency Rule
Implementation must reuse canonical KPI names and role terms from docs.
Do not create alternate dashboard widget labels for the same metric without updating canonical registry docs first.



## Foundation Implementation Rules
- Do not implement module-level registration behavior without aligning to one reusable module registry model.
- Do not implement module-specific event models when a shared platform/domain event model should be extended.
- Do not implement background jobs without explicit retry/failure/idempotency rules.
- Do not implement soft delete/archive behavior ad hoc; align to one documented retention/removal model first.
- Do not introduce cross-module tight coupling for reporting, notifications, or workflows when event-driven or shared-service boundaries are expected.
