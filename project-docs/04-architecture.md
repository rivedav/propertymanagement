# Architecture

## Stack
- Next.js (App Router)
- Supabase Auth
- Supabase Postgres
- Supabase Storage
- Vercel hosting

## Platform Decisions
- Manual payments first.
- Stripe later.
- Path-based tenant routing first.
- Subdomains later.

## 1) Identity Layer
- One global user identity per person.
- Auth handled by Supabase Auth.
- Identity is independent from tenant membership.

## 2) Tenant Context Layer
- User selects active tenant context.
- All tenant business operations resolve within selected tenant.
- Tenant context is mandatory for tenant-scoped endpoints/queries.

## 3) Relationship / Assignment Layer
- `unit_relationships`: ownership/residency/renter/landlord/co-owner/contact roles.
- `leases`: formal time-bound occupancy agreements.
- `contract_assignments`: time-bound scoped assignments (leasing/realtor/vendor/contractor).

## 4) Dashboard Context Layer
Contextual dashboards:
- Platform Admin
- Tenant Admin
- Owner
- Resident / Renter
- Leasing Agent
- Staff / Property Manager

## 5) Authorization Layer
Effective authorization combines:
- membership role
- active relationship records
- active lease state
- active contract assignment scope
- date validity windows
- permissions matrix

## 6) Financial Operations Layer
- Receivables: charges, payments, payment allocations.
- Payables: vendor bills and vendor payments.
- Planning: budgets, budget lines, reserve controls.

## 7) Governance / Board Layer
- boards, board_members
- meetings, meeting_minutes
- votes/resolutions
- templates and official notices/announcements

## 8) Amenities / Reservation Calendar Layer
- amenities catalog
- amenity reservations with rules/approvals
- community activities
- unified tenant calendar entries

## 9) Audit and Logging Layer
Business audit trail must capture business-critical state changes and actor context.
Technical/system logging handles runtime/infrastructure telemetry.

Minimum business audit trail fields:
- tenant_id
- actor_user_id
- action_type
- entity_type
- entity_id
- field_name (if field-level change)
- old_value
- new_value
- performed_at
- ip_address (optional)
- user_agent (optional)
- reason_or_note (optional)

Financial and governance modules require stricter auditability and reviewability.

## Why Tenant Membership Alone Is Not Enough
Membership alone cannot model real operations where one user can simultaneously be:
- owner of one unit,
- renter/resident of another,
- leasing agent for another scope.
The system must resolve access from membership + relationships + assignments + active periods.

## 10) Vendor Management Layer
- Vendor directory, service contracts, insurance/license expiry tracking, vendor ratings.
- Contract expiry alerts and assigned work tracking.
- Integrates with maintenance, work orders, and payables.

## 11) Capital Projects Layer
- Project registry, quotes/bids, approvals, milestones, project budgets, progress photos.
- Links governance approvals with financial planning and vendor assignments.

## 12) Reserve Fund Planning Layer
- Asset life forecast, replacement cost forecast, funding targets, reserve analysis.
- Feeds budget planning, financial statements, and board reporting.

## 13) Inspection Tracking Layer
- Fire, elevator, safety, and insurance inspections.
- Inspection schedule + reports + compliance status visibility.

## 14) Digital Forms Layer
- Renovation request, move-in/move-out, parking request, pet registration, guest registration.
- Form workflow integrates with approvals, notifications, and documents.

## 15) Packages and Deliveries Layer
- Package log, resident notifications, pickup status, locker/desk tracking.
- Event-driven updates and searchable package history.

## 16) Notification Center Layer
- Email notifications, in-app notifications, reminders, event triggers.
- Used by reservations, packages, inspections, forms, governance, and finance reminders.


## 17) Access Control Matrix Layer (v6)
- Tenant-admin-controlled role-based access matrix.
- Permission model is module/action based.
- Supports role templates and custom role overrides.
- Supports action-level rights: view, create, edit, approve, export, delete, manage.

## 18) Administrative Access Dashboard Layer (v6)
- Tenant-level dashboard for user administration and permission governance.
- Supports users-by-role, permission matrix editing, invite/disable/reactivate, and approval workflow rules.
- Provides controlled access to nodes/screens/windows/utilities.

## 19) Contextual Financial Visibility Layer (v6)
- Financial widgets are resolved by role + permission grants + active context.
- Owner financial visibility is explicit.
- Property manager financial visibility is limited-by-default and grant-based.

## Access Resolution Clarification (v6)
Access may come from:
- tenant membership
- unit relationship
- lease
- contract assignment
- granted permission matrix entries


## 20) Content Workflow Layer (Drupal-Inspired)
- Rich content editor with attachments (photos/documents/files).
- Editorial states: draft, submitted, approved, published, unpublished, archived.
- Supports publish-now and scheduled publish/unpublish flows.

## 21) Template System Layer
- Reusable email/notice/reminder/approval/invitation templates.
- Variable-based rendering at send/publish time.
- Configuration-driven template selection by module and workflow state.

## 22) Site Automation Layer (Planned for Later)
Planned (not immediate implementation):
- scheduled publish/unpublish
- reminder emails
- overdue notices
- reservation reminders
- meeting reminders
- maintenance follow-up notices

## 23) Reusable and Configuration-Driven Design Principles
- Prefer reusable components/services over duplicated logic.
- Prefer configuration and workflow definitions over hardcoded branching.
- Prefer centralized constants/enums/templates/state machines.
- Reuse existing engines before introducing new isolated implementations.


## Platform Operations Layer
Operational controls for SaaS reliability and security:
- maintenance mode controls (global + tenant)
- interruption notices and maintenance windows
- forced logout and session policy enforcement
- tenant-targeted operational communications

## Platform Monitoring Layer
Observability for platform and tenant health:
- tenant health events
- incident visibility
- upgrade impact monitoring
- downtime/error trend visibility

## Platform Recovery Layer
Shared-database recovery strategy:
- full platform backup jobs
- tenant-scoped export/restore where feasible and safe
- media/files backup strategy
- restore job lifecycle with strict audit trail

## Feature Enablement Layer
Tenant-scoped module enablement:
- feature flags for optional modules
- packaging/billable readiness controls
- safe rollout by tenant or tenant cohort

## Shared-Database Operational Constraint
The platform is shared-database multi-tenant. Operational controls and recovery flows must remain tenant-safe and audit-heavy. Tenant membership alone is not sufficient for access resolution; relationship, lease, contract, and granted permissions remain part of effective authorization.


## Add-On Catalog and Entitlement Layer
- central add-on catalog and bundle definitions
- tenant entitlements drive module visibility and action availability
- activation occurs automatically after payment confirmation
- deactivation removes access but preserves module data for reactivation

## Platform Billing and Collections Layer
- tenant invoice generation
- reminders, late fees, grace periods
- suspension/reactivation automation by billing rules
- tenant account states: active, past_due, grace_period, suspended, canceled

## Dynamic Module Activation Rules
- menus and module access are entitlement-driven
- forced logout is not default for add-on activation
- forced logout is reserved for security/maintenance events


## Explicit Module Classification Architecture

### Core Platform Module Boundary
Core modules are always available unless tenant/account is suspended by platform billing policy.

### Optional Add-On Module Boundary
Optional modules are controlled by entitlement + feature flag and enabled per tenant by Website Admin policy.

### Design-Now / Implement-Later Boundary
These are architecture-approved now but implemented in phased delivery:
- add-on marketplace/store
- entitlement lifecycle automation
- billing/suspension automation
- listing monetization tiers

### Future Boundary
Modules planned for later (not in current implementation commitment) remain extension points only.

## Add-On Activation / Deactivation Policy
- On confirmed payment, entitlement activates and module access becomes available automatically.
- Forced logout is not default for entitlement activation.
- On cancellation/disable, module access can be blocked but data remains preserved.
- On reactivation, historical module data becomes accessible again under entitlement rules.


## Base Plan / Package Layer
Defines tenant plan assignment and plan-included capabilities.
Plans may include limits and support/reporting tiers.

## Tenant Lifecycle Layer
Tenant lifecycle states:
- onboarding
- active
- past_due
- grace_period
- suspended
- canceled
- archived
- offboarded

## Export and Offboarding Layer
Supports:
- tenant data export
- documents/media export
- financial export
- tenant archive
- offboarding orchestration

## Impersonation and Support Access Layer
Restricted support mode for Platform Admin with mandatory audit logging.

## Reusable Workflow and Approval Engine Layer
Shared workflow engine for:
- content publishing
- payment approvals
- project approvals
- reservation approvals
- board approvals
- vendor approvals

## Notification Preference Layer
Configurable preferences by:
- user
- tenant
- event type
- escalation policy (later)

## Public Website and Tenant Subsite Model
- main platform marketing site
- tenant landing pages/subsites
- login/forgot-password entry
- no public self-signup

## Tenant Branding and Landing Page Builder Layer
UI-configurable tenant branding:
- logo, colors, background
- menu/page composition
- publication blocks
- landing-page layout controls


## Data Retention and Archival Policy Layer
Retention policies must be configurable per domain:
- audit logs
- financial records
- documents/media
- backups
- tenant archives
Policy controls should include retention duration, archive tier, and purge authorization.

## Rate Limiting and Abuse Protection Layer
Baseline controls:
- login attempt throttling
- API request rate limits
- upload abuse limits
- messaging abuse limits
- tenant and IP-based guardrails

## API Architecture Layer
High-level API model:
- internal service APIs
- external integration APIs
- webhook endpoints
- token-based authentication strategy
- rate-limited and auditable request handling

## Integration Framework Layer
Reusable connector model for:
- payment gateways
- banking APIs
- email providers
- SMS providers
- identity providers
- external accounting systems

## Background Job and Task Queue Layer
Asynchronous workloads:
- email and notification delivery
- exports and report generation
- backup/restore orchestration
- automation tasks and scheduled jobs

## File Storage and Security Layer
Storage architecture includes:
- document/media segregation
- tenant quota controls
- version-aware storage
- optional malware/security scanning pipeline

## Search Layer
System-wide search for:
- documents
- residents
- units
- issues
- vendors

## Data Import Layer
Onboarding import capabilities for:
- owners, residents, units
- opening balances
- vendors
- contracts

## Activity Feed Layer
Tenant and user activity timeline for:
- payments
- announcements
- maintenance events
- document/media uploads

## Feature Usage Analytics Layer
Platform Admin analytics:
- module adoption
- feature usage
- tenant activity patterns
- plan/add-on usage trends

## Disaster Recovery Layer (Targets)
Define and govern:
- backup validation cadence
- RTO targets
- RPO targets
- failover strategy

## Environment Strategy Layer
Environment model:
- development
- staging
- production
- demo

## Demo Environment Engine Layer
Demo capabilities:
- demo tenant generator
- sample data generator
- role simulation
- demo reset controls

## Tenant Configuration Profile Layer
Tenant-level settings:
- timezone
- currency
- payment rules
- late fee rules
- localization preferences

## Localization and Internationalization Layer
Design for:
- multi-language support
- multi-currency formatting
- locale-specific date/time formats


## Data Classification and Visibility Policy Layer
Classification levels:
- public
- tenant-public
- resident
- owner
- board
- staff
- admin
- system

Policy use:
- document/media visibility
- messaging/notification visibility
- reporting access scope
- audit/event visibility

## Permission Inheritance and Resolution Layer
Default hierarchy (tenant context):
- Platform Admin
- Tenant Admin
- Property Manager
- Staff
- Resident

Rules:
- higher roles may inherit lower-role permissions by policy
- explicit deny overrides inherited allow
- user overrides are evaluated after role inheritance
- all inheritance/override changes must be auditable

## Feature Flag Resolution Order
Recommended precedence:
1. platform plan
2. tenant overrides
3. tenant add-ons/entitlements
4. role permissions

If layers conflict, the highest-priority restrictive rule wins unless explicitly overridden by approved policy.

## Module Dependency Layer
Optional modules can declare dependencies.
Examples:
- Advanced Reporting -> requires core Financial modules
- SMS Notifications -> requires Notification engine
- Governance Pack -> requires Governance baseline
- Listing Promotion -> requires Listings module

## Configuration Versioning Layer
UI-driven settings require:
- configuration history
- version identifiers
- rollback capability
- config-change audit logging

## Platform Upgrade Strategy Layer
Upgrade model includes:
- scheduled maintenance windows
- tenant interruption windows
- migration scripts with verification
- staged rollouts (cohort/canary)
- feature-flag rollout safety and rollback path

## UI Capability Registry Layer
Registry maps:
- module
- routes
- menus
- UI components
- required permissions
- feature flag / entitlement key

## Data Consistency Strategy Layer
Shared-tenant consistency rules:
- explicit transaction boundaries for critical writes
- idempotent command handling for retry-prone flows
- safe retry patterns for async jobs/webhooks
- compensating actions for partial failures


## Document Management Workflow Layer (Detailed)
Document lifecycle supports:
- upload
- metadata edit
- publish
- unpublish
- schedule publish
- schedule unpublish
- version replacement
- delete (policy-controlled)

Required metadata:
- title
- description
- category
- tags
- created_date
- created_by
- file_size
- file_path
- document_version
- visibility_level

Visibility levels:
- public
- tenant-public
- resident
- owner
- board
- staff
- admin

Scheduling:
- publish_date
- unpublish_date
Scheduled actions must execute through the background job/task queue architecture.

Bulk operations:
- bulk publish
- bulk unpublish
- bulk delete
- bulk visibility change

Storage integration:
- tenant storage quotas
- file size tracking
- total tenant storage usage tracking
- optional file scanning/security controls

## CSV / Excel Import Engine Layer (Detailed)
Supported onboarding/migration targets:
- units
- owners
- residents
- leases
- vendors
- financial balances
- charges
- payments
- bank transactions
- contracts

Standard import workflow:
1. upload CSV/Excel
2. detect file format
3. column mapping
4. data validation
5. preview results
6. user confirmation
7. background processing
8. import result report

Validation baseline:
- required fields
- duplicate detection
- invalid date formats
- invalid currency values

Import outputs:
- records created
- records updated
- records skipped
- records failed
- downloadable error report


## Import Safety and Tenant Isolation Layer
Authoritative tenant context rules:
- every import runs under an explicitly selected target tenant in UI
- selected tenant context is the authoritative source for record ownership
- import files must not set or override `tenant_id`
- platform assigns `tenant_id` internally to every tenant-scoped imported record
- platform generates internal primary IDs; file-provided IDs are never trusted as internal IDs

Legacy references:
- external identifiers may be stored only as reference fields, such as:
  - `external_legacy_id`
  - `source_system_id`

Isolation rules:
- matching and updates are tenant-scoped by default
- global matching is allowed only for explicitly global entities by design
- import flow must block cross-tenant leakage

Preview and confirmation safety:
- preview must display selected target tenant clearly before confirmation
- confirmation must include import type, source file, and tenant scope summary

## Import Matching and Deduplication Policy
Recommended matching priority:
1. `external_legacy_id`
2. business key
3. normalized identity fields (email/phone/name) with caution

Dedup safety rules:
- do not silently auto-merge risky matches
- uncertain matches must be flagged as:
  - possible duplicate
  - review needed
- merge/apply decisions must be auditable


## Analytics and Dashboard Architecture
Layer 1: Raw transactional data
- charges, payments, leases, work orders, reservations, documents, votes, audit logs

Layer 2: Analytics/Aggregation
- summaries, rollups, metrics views
- tenant financial summaries
- maintenance metrics
- occupancy metrics
- add-on usage metrics

Layer 3: Dashboard Widgets
- KPI cards
- charts
- timelines
- tables
- calendars
- trend indicators

Dashboard families:
- Platform Admin (Website Admin)
- Tenant Admin
- Property Manager / Staff
- Owner
- Resident / Renter
- Leasing Agent / Realtor

Widget/filter requirements:
- visualization support: metric cards, bar/line/pie/donut/stacked charts, timelines, calendars, filterable tables, exportable widgets
- filters: date range, tenant, property, building, unit, category, vendor, status, role/context

Future configurability:
- dashboard layouts should be metadata-driven and reusable, not hardcoded per page.

## Entitlements / Feature Enablement Architecture (Expanded)
- feature catalog
- plan features
- tenant entitlements
- add-on activation/deactivation
- module enablement checks
- preserved data after deactivation

## Notification Engine Architecture (Expanded)
- notification templates
- notification channels
- notification queue
- delivery logs
- reminder rules
- event-driven notifications


## Operations Expansion: Preventive Maintenance and Asset Register
Preventive Maintenance:
- recurring schedules
- recurrence rules
- task generation and tracking
- completion and escalation workflow

Asset Register:
- building/infrastructure asset inventory
- lifecycle state tracking
- linkage to preventive and corrective maintenance workflows

## Leasing Agent Pipeline Architecture
- applications pipeline
- showing schedule
- unit marketing workflow
- marketing file management
- contract-period scoped access

## Owner Ledger and Operational Documents Architecture
Owner Ledger:
- owner-level statement of charges, payments, and running balance

Property Manager Operational Documents:
- operational reports
- inspection forms
- contractor documentation

## Canonical Platform Admin Dashboard KPI Set
- revenue overview
- tenant health
- add-on adoption
- storage usage
- platform usage
- system activity
- security logs
- export center

## Inspection Due / Overdue Analytics Widget
A standard analytics widget must expose:
- inspections due
- inspections overdue
- due date windows and trend deltas


## Employee Portal / Workforce Architecture Layer
Scope model:
- Platform Workforce scope
- Tenant Workforce scope

Functional coverage:
- Employee Dashboard (leave balance, pending requests, hours, announcements)
- Time & Attendance (time entry, duplicate prior entry, monthly hours, filters/export)
- Employee Self-Service (profile, security, password, preferences, account info, browser notifications)
- Leave / Requests (vacation/sick/personal, history, approval status)
- Holidays / Legal Notices (calendar, notices, acknowledgement tracking)
- Pay Statements / Payroll Visibility (viewer, print/download, YTD)
- Workforce Admin (directory, time/leave approvals, reports, payroll integrations)
- Workforce Analytics (hours, leave, pending requests, attendance trends)

Reuse rule:
This module family must reuse existing engines:
- invitation/auth engine
- MFA/security engine
- role/permission engine
- workflow/approval engine
- document engine
- notification engine
- analytics/dashboard engine
- export engine

It is not a separate isolated application.


## Workforce Payroll Withholding Rules Engine Architecture
Withholding/tax retention must be configuration-driven, not hardcoded UI logic.

Rule model requirements:
- withholding percentage
- exempt threshold amount
- effective date
- active/inactive status
- rule version history
- optional tenant-specific override
- optional employee/contractor classification support (later)

Example rule behavior:
- first $500 earned is exempt
- after threshold, 10% withholding applies

Security and governance:
- permission-controlled rule administration
- audited rule changes
- versioned rule lifecycle
- effective-date-aware calculation resolution

## Pay Statement / Paystub Breakdown Architecture
Pay statements should support:
- gross pay
- taxable/eligible amount
- exempt threshold
- withholding amount
- other deductions
- total deductions
- net pay
- YTD gross
- YTD net
- YTD withholding

Calculation and rendering should reuse shared rule/configuration services and export/reporting services.



## Professional Services Billing / Time / Invoice Architecture Layer
Optional add-on module family for reusable client billing operations.

Functional coverage:
- client management and billing profiles
- approved time-entry billing workflow
- service-type and project billing classification
- effective-dated hourly and fixed-fee pricing
- billable/reimbursable expense capture
- withholding/tax retention calculation and snapshot preservation
- invoice generation, numbering, formatting, and PDF output
- payment receipt, partial allocation, and receivables visibility

Core architecture rules:
- this is not a one-off billing screen set
- this must be a reusable modular module family
- historical invoices must remain unchanged after future rate or withholding-rule changes
- invoice lines must preserve rate snapshots
- invoice lines must preserve withholding snapshots
- payments must support partial allocation

Required engine reuse:
- invitation/auth engine
- MFA/security engine
- permission engine
- workflow/approval engine
- document/receipt engine
- notification engine
- analytics/dashboard engine
- export/PDF engine
- audit engine

Boundary clarification:
- Workforce module handles employee time logging, leave, and paystub visibility
- Professional Services Billing module handles client billing, pricing, expenses, withholding, invoicing, and receivables


## Dashboard / Widget Configuration Engine Layer
Dashboards must be metadata-driven and configurable rather than hardcoded page-by-page.

Supported dashboard scopes:
- platform dashboards
- tenant dashboards
- module dashboards
- role dashboards
- future user-custom dashboards

Reusable widget configuration must support:
- widget code
- title
- module/data source
- chart type
- enabled/disabled
- default visibility
- layout position
- size
- refresh interval
- export capability
- fullscreen capability
- chart-switch capability
- filtering capability
- series toggle capability

Widget series configuration must support:
- multiple lines/series in one chart
- enable/disable specific series
- legend visibility
- axis grouping
- aggregation type
- default visible series

Reusable filter definitions should support:
- tenant
- property
- building
- unit
- date range
- status
- vendor
- category
- resident
- client
- service type
- employee
- project

Visualization support:
- KPI cards
- tables
- line charts
- bar charts
- stacked bar charts
- pie charts
- donut charts
- timeline views
- area charts
- trend indicators
- future heatmaps / advanced analytics

Permission rule:
- dashboards and widgets must support role-based visibility and permission-aware rendering.

Architecture rule:
- charts/widgets must use reusable dashboard/analytics engines rather than one-off hardcoded implementations in each module.


## Historical Identity, Naming, and Relationship Continuity Layer
The platform must preserve historical relationships and administrative continuity over time.

Architecture rules:
- stable master entities must not be replaced unnecessarily
- relationship changes must be tracked historically rather than overwriting past records
- the same tenant organization may change administrator personnel, board members, display/legal name, and public path/subdomain while remaining the same `tenant_id`
- the same property/building/unit may change owner, resident, lease, or assignment while preserving historical records
- historical review must be available for both operational and governance-sensitive records

Stable entity examples:
- tenant organization
- property
- building
- unit
- user identity

Historical tracking examples:
- ownership changes from person A to person B
- tenant administrator personnel changes
- board member term changes
- realtor/agent assignment changes
- tenant/complex/building display name changes
- tenant path/subdomain changes without changing the underlying tenant identity

Design rule:
Current-state resolution should rely on active/effective records, while historical tables/views/auditability must preserve prior states without destructive replacement.


## Canonical Architecture Terminology
Canonical terms for architecture docs:
- Platform Admin (Website Admin)
- Tenant
- Property
- Building
- Unit
- Owner
- Resident / Renter
- Leasing Agent
- Workforce Employee
- Contractor
- Board Member
- Administrator
- Module
- Add-On
- Role
- Permission
- Access Profile
- Scope
- Entity
- Relationship
- Historical Record


## Canonical Access Architecture
The platform access model must be described consistently across documentation.

Effective access combines:
- role template
- access profile
- scope
- relationship
- add-on entitlements
- overrides
- active/effective dates

Documentation rule:
- `02-roles-and-access.md` owns role semantics and access examples
- this file owns the architecture model
- `05-data-model.md` owns the supporting entities
- `14-admin-access-menu-matrix.md` owns menu/module exposure by context


## Canonical Module Architecture Names
Use these canonical module families across documentation:
- Identity & Access
- Tenant Management
- Property Management
- Leasing
- Maintenance
- Documents
- Reservations
- Finance & Billing
- Workforce
- Reporting & Analytics
- Add-On Marketplace
- Integrations
- API Layer
- Security

Clarification:
Detailed submodules may exist, but they should map back to one of the canonical module families above.


## Canonical Dashboard Widget Names
Canonical widget names should use title case:
- Revenue Overview
- Tenant Health
- Add-On Adoption
- Storage Usage
- Platform Usage
- System Activity
- Security Logs
- Export Center
- Inspection Due
- Maintenance Status
- Occupancy Rate
- Vacancy Rate

Documentation rule:
Where lowercase variants exist, normalize future references to the title-case canonical names above.


## Module Registry and Lifecycle Architecture
Every module should be registered through one reusable module registry model rather than ad hoc per-feature wiring.

A module registration definition should describe:
- module key
- canonical module name
- routes
- menus
- permissions
- owned entities
- dashboard widgets
- notifications
- background jobs
- feature flags
- dependencies

Lifecycle direction:
- planned
- enabled
- disabled
- suspended
- archived

Rule:
Module lifecycle and registration should be configuration-driven and tenant-safe.

## Canonical Shared Service Boundaries
The platform should define and reuse these shared backend service boundaries:
- auth service
- permission resolution service
- tenant context service
- workflow/approval service
- notification service
- billing service
- dashboard query service
- document service
- import/export service
- audit service
- monitoring/health service

Rule:
Domain modules may extend these services, but should not replace them with isolated parallel implementations unless architecture docs are updated first.

## Platform and Domain Event Model
The platform should define a reusable event model for domain and platform actions.

Minimum event categories:
- user invitation
- membership changes
- payment posted
- invoice generated
- document published
- maintenance request created
- ownership transferred
- add-on enabled

Event rules:
- events must be tenant-safe
- events must be auditable where business-critical
- events should support notifications, analytics, monitoring, and automation without tight coupling between modules

## Monitoring and Health Signal Model
Monitoring should define canonical health signals for:
- application health
- tenant health
- failed jobs
- failed notifications
- failed integrations
- quota alerts
- security alerts

These signals should feed:
- monitoring dashboards
- alerting workflows
- operational review
- audit-friendly incident handling

## Background Job Operating Model
Background jobs should follow explicit operating rules for:
- retry policies
- failed job handling
- dead-letter strategy or equivalent
- idempotency
- admin/UI visibility for job status

Operational rule:
Async job execution must be observable, retry-safe, and reviewable by authorized administrative roles.

## Search and Indexing Strategy Layer
Search architecture should define:
- searchable entities
- tenant-scoped search boundaries
- permission-aware result filtering
- indexing/update direction

Search results must remain:
- tenant-safe
- permission-aware
- consistent with active visibility rules

## Archive, Soft Delete, and Historical Record Rules
The platform should distinguish clearly between:
- archive
- soft delete
- immutable historical record

Rules:
- immutable historical records must not be destructively replaced
- archive behavior must preserve audit and retention obligations
- restore behavior must be policy-controlled and auditable
- removal behavior must remain retention-safe

## Master Data vs Transactional Data Distinction
The platform should distinguish:
- master/reference data
- transactional data

Examples:
- master/reference data: tenant organizations, properties, buildings, units, service types, categories, module definitions
- transactional data: payments, invoices, expenses, reservations, maintenance events, approvals, notifications, audit events

This distinction should guide:
- schema planning
- indexing
- archival/retention behavior
- reporting design

## Dangerous Administrative Action Safeguards
The platform should define extra safeguards for:
- restore
- tenant suspend/reactivate
- impersonation
- role/permission changes
- numbering scheme changes
- withholding rule changes

Safeguard direction:
- stronger audit logging
- explicit reason capture
- confirmation/approval gates where policy requires
- scoped execution rights
- historical reviewability

## Performance Intent and Scalability Direction
Performance direction should explicitly include:
- tenant-scoped indexing expectations
- rollups/reporting models for dashboards
- snapshot strategy for historical correctness
- avoidance of cross-module tight coupling

Rule:
Performance-sensitive read models, rollups, and snapshots should be designed intentionally before schema implementation, not left to ad hoc optimization later.
