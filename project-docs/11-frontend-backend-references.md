# Frontend and Backend References

## Frontend (Next.js)
- App Router docs: https://nextjs.org/docs/app
- Routing and layouts: https://nextjs.org/docs/app/building-your-application/routing
- Data fetching: https://nextjs.org/docs/app/building-your-application/data-fetching
- Image security and remotePatterns: https://nextjs.org/docs/pages/api-reference/components/image

## Backend and Data (Supabase + Postgres)
- Supabase RLS guide: https://supabase.com/docs/guides/database/postgres/row-level-security
- Supabase JWT deep dive: https://supabase.com/docs/learn/auth-deep-dive/auth-deep-dive-jwts
- Supabase custom claims and RBAC: https://supabase.com/docs/guides/database/postgres/custom-claims-and-role-based-access-control-rbac
- PostgreSQL official RLS docs: https://www.postgresql.org/docs/current/ddl-rowsecurity.html

## Multitenancy Architecture References
- Azure multitenancy checklist: https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/checklist
- Azure multitenant identity approaches: https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/approaches/identity
- AWS SaaS Lens identity and tenant context: https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/identity-and-access-management.html
- AWS PostgreSQL tenancy models (bridge/pool/silo context): https://docs.aws.amazon.com/prescriptive-guidance/latest/saas-multitenant-managed-postgresql/bridge.html

## Claude Code Operational References
- Slash commands (/init, /memory, /permissions): https://docs.anthropic.com/en/docs/claude-code/slash-commands
- CLAUDE.md memory behavior and imports: https://docs.anthropic.com/en/docs/claude-code/memory
- Claude Code settings hierarchy: https://docs.anthropic.com/en/docs/claude-code/settings
- Security and permissions model: https://docs.anthropic.com/en/docs/claude-code/security

## Internal Rule
When external advice conflicts with project-docs, prioritize project-docs and update docs intentionally after review.

## Dashboard Frontend Direction
- Dashboards are implemented with Next.js + React.
- Each role has its own dashboard layout and navigation profile.

Role-specific dashboard layout targets:
- Resident Dashboard
- Owner Dashboard
- Tenant Admin Dashboard
- Property Manager Dashboard
- Leasing Agent Dashboard

Implementation note:
Use shared UI primitives with role/context-specific layout shells to avoid duplicated frontend logic.

## v4 Dashboard Navigation Notes (Documents & Media)
Next.js + React dashboard layouts should include role-specific document/media entry points:

- Tenant Admin Dashboard:
  - Documents & Media
  - Document Library
  - Media Gallery
  - Forms & Templates

- Resident Dashboard:
  - Community Documents
  - Community Photos

- Owner Dashboard:
  - Ownership Documents
  - Financial Documents

## v5 Frontend Requirements
- Dashboard widgets for reserve funds, capital projects, packages, inspections.
- Role-specific module entry points remain mandatory.
- Table filtering and export patterns (CSV/PDF/print) for finance, governance, audit.

## v5 Backend Requirements
- Modular domain services per feature module.
- Reporting query layer for dashboard and export endpoints.
- Centralized audit event generation with field-level diffs.
- Scoped access enforcement by tenant + relationship + lease + contract.


## v6 Frontend Access-Control Requirements
- Administrative Access Dashboard (Tenant Admin)
- Permission matrix table UI (role x module x action)
- Role templates and override management UI
- Per-role menu visibility with action-level gating
- Owner payment status widgets (fees, due dates, overdue, reminders, statements)
- Property manager limited finance widgets (budget snapshot, expense view, collections view if permitted)

## v6 Backend Access-Control Requirements
- Permission evaluation service (tenant + role + module + action + context)
- Module/action authorization middleware
- User-level override resolution
- Invitation access flow service
- Permission-change audit logging with field-level diffs


## Reuse and Workflow-Driven UI References
Frontend should prefer reusable patterns for:
- permission matrix UI
- role template editor
- workflow state controls (publish/schedule/approve)
- reusable table/filter/export components
- reusable editor + attachment/publishing interfaces

Backend should prefer reusable services for:
- centralized permission evaluation
- workflow state transitions
- template rendering
- approval handling
- audit event generation


## Platform Operations UI/Backend References
Frontend references:
- maintenance/interruption banner and scoped notice components
- platform monitoring dashboard (global + tenant health)
- backup/restore admin screens
- feature enablement panel (tenant module toggles)
- support/issue reporting screens

Backend references:
- session invalidation service
- maintenance mode middleware and guards
- feature-flag evaluation service
- backup/restore job orchestration services
- monitoring event capture and health aggregation


## Monetization UX and Services References

Frontend:
- add-on catalog/store UI
- tenant add-on purchase/subscription UI
- module enablement and entitlement status UI
- tenant billing status dashboard
- suspension/reactivation notices
- listings marketplace UI (tenant-local / platform-wide)

Backend:
- entitlement evaluation service
- feature-flag and module registry checks
- subscription activation and cancellation lifecycle
- billing/reminder/late-fee services
- suspension/reactivation automation
- preserved module-data access rules after reactivation


## Additional UI/Backend Reference Areas
Frontend:
- plan selection and entitlement visibility
- add-on purchase and billing status UX
- tenant lifecycle state badges and notices
- impersonation warning bar and exit controls
- export center and offboarding status views
- branding/landing page builder UI
- workflow-driven publishing and approval queue UI

Backend:
- plan resolution service
- entitlement and pricing rule evaluation
- lifecycle transition service
- support impersonation service with audit hooks
- export orchestration service
- reusable workflow engine APIs
- notification preference evaluation service


## Additional Platform Capability References
Frontend:
- activity feed timeline widgets
- import wizard and error resolution UI
- export center and job status views
- tenant settings (timezone/currency/localization)
- plan/add-on usage analytics dashboards
- demo mode indicator and reset controls (admin-only)

Backend:
- API gateway/auth/rate-limit middleware
- webhook dispatcher + retry strategy
- integration connector framework
- async queue workers and dead-letter handling
- search indexing workers
- retention/archival scheduler
- observability pipeline and alert routing


## Document Management UI/Backend Contract
Frontend expectations:
- document library grid/table
- upload form with metadata fields
- metadata edit panel
- bulk action toolbar
- scheduled publish/unpublish controls

Backend expectations:
- document permission action enforcement
- scheduled publish/unpublish job handlers
- tenant quota enforcement and usage aggregation
- bulk action execution + audit logging

## CSV/Excel Import UI/Backend Contract
Frontend expectations:
- format detection feedback
- column mapping screen
- validation summary + row-level issues
- preview and confirmation step
- result report with downloadable errors
- template download section

Backend expectations:
- parser adapters (CSV/Excel)
- mapping resolver
- validation engine
- async import executor
- idempotent retry-safe processing
- report artifact generation


## Import Safety UX and Service Contract
Frontend requirements:
- always show selected target tenant in import header and preview
- block confirmation if tenant context is not explicitly selected
- show mapping + validation + dedup risk indicators before run
- show possible duplicate/review-needed states

Backend requirements:
- ignore/reject tenant_id from import files
- assign tenant_id from authenticated selected tenant context
- generate internal IDs server-side only
- enforce tenant-scoped matching by default
- allow global matching only for explicitly global entity types
- audit import actor, tenant, file, import type, and result counts/failures


## Analytics and Dashboard Contract References
Frontend:
- reusable KPI/chart/timeline/table/calendar widgets
- standardized dynamic filter bar
- role-context dashboard families
- exportable report widgets

Backend:
- dashboard widget data providers
- tenant-safe query service for widgets
- analytics rollup jobs
- entitlement resolution for widget/module visibility
- notification queue processing + delivery logging


## v7 Naming and Capability Normalization
Canonical terms:
- Platform Admin (Website Admin)
- Tenant
- Owner
- Resident / Renter
- Leasing Agent
- Module
- Feature
- Add-On

Canonical dashboard widget names:
- revenue overview
- tenant health
- add-on adoption
- storage usage
- platform usage
- system activity
- security logs
- export center


## Workforce Module UI/Backend Contract
Frontend:
- employee dashboard family
- time entry and monthly summary screens
- leave request and approval status views
- pay statement viewer/download screens
- workforce analytics widgets

Backend:
- workforce scope resolver (platform vs tenant)
- attendance and leave workflow services
- payroll visibility/export integration service
- workforce analytics rollup jobs
- workforce notification/reminder processing


## Payroll Withholding and Paystub Contract
Frontend:
- withholding rule admin form (authorized roles only)
- effective-date/version timeline UI
- paystub breakdown viewer with YTD sections
- clear scope indicators (platform workforce vs tenant workforce)

Backend:
- withholding rule resolution service (effective-date aware)
- rule versioning service
- optional tenant override resolver
- payroll calculation adapter using reusable rules engine
- payroll rule audit event emitter



## Professional Services Billing UI/Backend Contract

Frontend expectations:
- client list/detail screens with billing settings
- time-entry approval queue and invoice generation actions
- invoice builder supporting hourly, fixed-fee, and expense lines
- invoice tables with filters for client, date, status, project/service
- payment tables with filters for method, date, client, invoice, status
- detailed, summary, and grouped invoice previews
- PDF print/export action
- rate/withholding history visibility on invoice detail

Backend expectations:
- effective-dated rate resolver
- withholding rule resolver with version history
- immutable invoice-line snapshot capture for rates and withholding
- auto-numbering service with uniqueness validation
- invoice PDF/export generation service
- payment allocation and remaining-balance recalculation service
- invoice status transition service with audit logging


## Dashboard / Widget Configuration Engine Contract

Frontend expectations:
- metadata-driven dashboard renderer
- reusable widget container with title/actions/state handling
- configurable layout grid by scope and breakpoint
- widget toolbar support for export, fullscreen, chart switch, and filters
- multi-series chart controls with legend and series toggles
- reusable filter bar for tenant/property/building/unit/date/status/vendor/category/resident/client/service type/employee/project
- permission-aware widget hiding/disabled states

Backend expectations:
- dashboard definition resolver by platform/tenant/module/role scope
- widget data-provider registry
- reusable filter-definition registry
- series-definition resolver
- permission-aware dashboard/widget visibility resolver
- widget refresh/query orchestration
- audit logging for dashboard configuration changes


## Consolidation Note
This file provides implementation references and UI/backend contract guidance only.

Canonical owners:
- access model architecture: `project-docs/04-architecture.md`
- role and permission semantics: `project-docs/02-roles-and-access.md`
- conceptual entities: `project-docs/05-data-model.md`
- module visibility and menus: `project-docs/14-admin-access-menu-matrix.md`

Do not treat this file as the owner of architecture definitions.
