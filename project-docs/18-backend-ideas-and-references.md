# Backend Ideas and References

## Backend Direction for MVP
- Use Postgres with strict RLS for tenant isolation.
- Keep authorization in both app logic and SQL policies.
- Support contextual access using membership + relationships + contract assignments.
- Add audit logging for critical writes and admin actions.

## References
- PostgreSQL RLS (official): https://www.postgresql.org/docs/current/ddl-rowsecurity.html
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security
- Supabase JWT and claims: https://supabase.com/docs/learn/auth-deep-dive/auth-deep-dive-jwts
- Azure multitenancy checklist: https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/checklist
- AWS SaaS Lens identity and context: https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/identity-and-access-management.html

## Backend Checklist
- [ ] Global users + tenants + tenant_memberships schema
- [ ] Properties/buildings/units schema
- [ ] unit_relationships schema with active date windows
- [ ] leases schema with active date windows
- [ ] contract_assignments schema with permission_scope_json
- [ ] RLS policies combining membership, relationships, and assignments
- [ ] Audit log write path from server actions/api handlers
- [ ] Migration and rollback notes per change

## v5 Backend Implementation Ideas
- Modular domain services: vendors, projects, reserve, inspections, forms, packages, notifications.
- Unified audit event generation pipeline with field-level change capture.
- Reminder/notification scheduling for deadlines and expiries.
- Reporting endpoints for dashboard widgets and export jobs.
- Reservation conflict/approval engine for amenities.
- Inspection scheduling and compliance status service.
- Package event logging and resident notification workflow.


## v6 Backend Access-Control Ideas
- Module/action access checking service with tenant-context resolver.
- Context-based dashboard selector (role + permissions + active relationship/lease/assignment).
- Permission inheritance model (role templates -> role permissions -> user overrides).
- Override precedence and conflict resolution rules.
- Access-change audit event generation with field-level diffs.


## Drupal-Inspired Backend Services
- Centralized permission service (role/module/action + context).
- Workflow engine for content lifecycle states.
- Template rendering service with variable registry.
- Reusable notification engine for workflow/time triggers.
- Reusable audit logging service with field-level diffs.
- Approval service for hierarchical decisions.


## Platform Operations Backend Ideas
- shared-database tenant-safe recovery service design
- maintenance middleware with tenant/platform scope evaluation
- centralized session invalidation and token revocation flow
- health event logging and aggregation pipeline
- job-based backup/restore orchestration
- support ticket domain services
- future messenger integration hooks (planned)


## Monetization Backend Services
- centralized entitlement service
- feature-flag/module registry service
- platform billing and collections service
- subscription lifecycle service
- suspension/reactivation service
- listings publication service


## New Backend Service Ideas
- plan/package resolution engine
- entitlement/subscription pricing engine
- lifecycle transition and suspension service
- impersonation service with strict audit hooks
- export/offboarding orchestration jobs
- reusable workflow/approval engine
- notification preference and escalation engine


## Platform Capability Backend Ideas
- unified rate-limit and abuse-detection middleware
- API + webhook contract layer with retry/idempotency support
- connector SDK pattern for external integrations
- queue worker framework with dead-letter recovery
- retention and archival scheduler
- search indexing pipeline
- usage analytics aggregation service
- DR drill runner and backup validation service
- tenant settings and localization resolver service


## Backend Control Service Ideas
- permission resolution service (hierarchy + override + deny precedence)
- feature resolution service (plan -> tenant override -> add-on -> role permission)
- module dependency validator
- configuration version/rollback service
- upgrade orchestration service (migrations + staged rollout + rollback)
- consistency guard service (idempotency/retry/locking patterns)


## Backend Services: Document and Import Engines
Document services:
- metadata and version service
- publish scheduler service
- visibility and permission enforcement service
- bulk action processor with audit trail
- tenant storage usage/quota service

Import services:
- file parser abstraction (CSV/Excel)
- mapping and normalization service
- validation pipeline
- queued import execution service
- result reporting and error artifact service


## Tenant-Safe Import Engine Controls
- tenant-context resolver (authoritative import tenant scope)
- import ownership guard (reject file-provided tenant_id/internal IDs)
- legacy reference mapper (`external_legacy_id`, `source_system_id`)
- tenant-scoped matcher with explicit global-entity allowlist
- dedup risk classifier (possible-duplicate/review-needed)
- batch audit emitter for import actor + tenant + file + result counts


## Analytics and Notification Backend Ideas
- analytics rollup job pipeline (daily/monthly)
- dashboard query services with tenant-safe guards
- widget data provider registry
- entitlement resolution service for widget/module gating
- notification queue processing workers
- channel adapters and delivery log service


## v7 Backend Service Additions
- preventive maintenance scheduler and task generator
- asset lifecycle service with maintenance linkage
- leasing pipeline service (applications/showings/marketing files)
- owner ledger aggregation service
- inspection due metrics provider
- canonical KPI registry service for dashboard consistency


## Workforce Backend Ideas
- workforce scope-aware access service
- attendance entry and monthly aggregation service
- leave request workflow and approval service
- legal notice acknowledgment tracking service
- pay statement visibility and export orchestration service
- workforce analytics rollup and widget provider services


## Workforce Payroll Backend Ideas
- withholding rules engine (configurable, versioned, effective-date aware)
- rule override resolver (tenant-specific when allowed)
- paystub breakdown calculation provider
- payroll audit logging pipeline for rule changes
- classification-aware withholding extension points (employee/contractor)



## Professional Services Billing Backend Ideas
- client billing-profile service
- effective-dated pricing engine for hourly and fixed-fee resolution
- immutable invoice-line snapshot capture for rate and withholding details
- time-entry approval to invoice pipeline
- billable-expense approval and invoice-linking service
- invoice numbering service with tenant-safe sequence control
- invoice PDF rendering/export service
- payment receipt and partial allocation service
- status-history audit emitter for invoice lifecycle changes


## Dashboard Configuration Backend Ideas
- dashboard definition registry service
- scope resolver for platform, tenant, module, role, and future user dashboards
- widget configuration service
- widget filter-definition registry
- widget series-definition resolver for multi-line/multi-series charts
- layout persistence service
- permission-aware widget visibility service
- widget export/fullscreen/chart-switch capability resolver
- dashboard configuration audit event pipeline


## Historical Continuity Backend Ideas
- stable-entity identity resolver for tenant, property, building, unit, and user master records
- effective-dated relationship service for ownership, occupancy, membership, administration, and agent assignments
- historical name/path alias service for tenant/property/building naming continuity
- current-state projection service derived from active historical records
- history-aware query endpoints for tenant users, ownership, administration, and naming-review tables
- audit/event generation for assignment changes, name changes, and path/domain changes


## Consolidation Note
This file is for backend implementation ideas only.
Canonical definitions for access architecture, entity naming, module boundaries, and dashboard architecture belong in:
- `project-docs/04-architecture.md`
- `project-docs/05-data-model.md`
- `project-docs/19-doc-governance-and-boundaries.md`
