# MVP Scope

## In Scope
1. Platform admin controls
2. Tenant creation and management
3. User membership across multiple tenants
4. Contextual roles and permission enforcement
5. Properties, buildings, units
6. Unit relationships (owner, resident, renter, landlord, co-owner, contacts)
7. Lease records (time-based)
8. Contract assignments (time-based, scoped access)
9. Announcements
10. Maintenance requests
11. Manual charges, payments, and payment allocations
12. Audit logs for key actions

## Out of Scope (For Now)
- Stripe Connect and online payment orchestration
- Full accounting features
- Workflow automation platform integrations
- Subdomain-based tenant routing
- Background worker infrastructure
- In-app AI product features

## MVP Success Criteria
- Tenant data isolation is enforced in DB and app.
- Access checks combine membership + relationships + contracts + active dates.
- Core operational modules are usable end-to-end.
- Dashboard context switching is reliable for multi-role users.
- Audit logging captures security and business critical events.

## Scope Classification (Platform Operations Update)

### Current Scope
- Core tenant operations, contextual dashboards, access control, financial basics, governance basics, amenities/calendar, audit trail.
- Tenant-level configuration for permissions and contextual module visibility.

### Design Now / Implement Later
- Platform monitoring dashboard (global + tenant health views).
- Tenant feature enablement flags (for modular rollout and packaging).
- Shared-database backup/restore workflow design and restore auditability.
- Client issue reporting module design and lifecycle.

### Future Scope
- Internal messaging module (feature-flagged, optional/billable).
- Heavy automation/orchestration workflows.
- Advanced tenant-scoped restore tooling and operational playbooks.


## Add-On Scope Classification

### MVP Core
- core property operations, finance baseline, governance baseline, amenities baseline, audit baseline

### Design Now / Implement Later
- add-on catalog and bundles
- tenant entitlements and subscription lifecycle
- tenant billing states, reminders, grace periods, suspension rules
- module activation/deactivation with data preservation
- tenant-local and platform-wide listing publication controls

### Future Premium Modules
- advanced automation
- internal messaging
- advanced marketplace promotion and premium listing placement


## Module Classification Matrix

### 1) Core Platform Modules
- identity and memberships
- tenant/user administration and RBAC
- properties/buildings/units
- unit relationships and leases
- core maintenance requests
- core receivables/payables/budgets
- governance baseline (meetings, minutes, announcements)
- amenities and calendar baseline
- audit trail and reporting baseline
- platform operations baseline (maintenance, monitoring, recovery controls)

### 2) Optional Add-On Modules (Website Admin enabled per tenant)
- contractor management
- issue reporting / support tickets
- advanced maintenance workflows
- package tracking
- inspection tracking
- SMS notifications
- advanced email automation
- news / publishing module
- internal messaging
- emergency notifications
- advanced document library
- additional storage tiers
- media gallery pro
- document versioning / retention
- advanced reports
- accountant tools
- billing management pro
- collections / late fee automation
- reserve planning
- board voting
- governance pack
- agenda / resolution management
- tenant-local property sale/rental listings
- platform-wide publication upgrade
- featured listing promotion
- premium support assistant
- priority support
- onboarding package
- migration service

### 3) Design-Now / Implement-Later Modules
- add-on catalog and bundles
- entitlement/subscription lifecycle
- tenant billing state automation and suspension/reactivation policy
- module activation/deactivation with preserved data
- tenant add-on self-purchase flow
- listing publication scope and monetization rules

### 4) Future Modules
- deeper workflow builder
- internal messaging advanced phases (base module remains Optional Add-On when released)
- advanced marketplace/commercial capabilities
- broader automation/orchestration


### Classification Rule Clarification
The modules listed under "Optional Add-On Modules" are not core by default.
They are only active when enabled by Website Admin for the tenant and backed by entitlement.

If an optional module is not yet implemented, it remains:
- classified as Optional Add-On (commercial/entitlement model), and
- in Design-now / Implement-later or Future delivery state until built.


## Platform Plan and Module Classification Clarification
Base plans/packages may define:
- included modules
- included add-ons
- user limits
- storage limits
- support tier
- reporting level

Design rule:
- every feature must be classified as Core Platform, Optional Add-On, Design-Now / Implement-Later, or Future.
- optional modules are disabled by default unless enabled by Website Admin for the tenant.


## Architecture Capabilities Classification (Pre-Schema)
### Core Platform Architecture Components
- retention/archival policy framework
- logging/observability baseline
- export framework
- tenant lifecycle and offboarding
- API auth and internal service boundaries

### Design-Now / Implement-Later Architecture Components
- background job/task queue
- integration framework
- rate limiting framework
- search service
- import tooling framework
- feature usage analytics
- reusable dashboard/widget configuration engine

### Future Architecture Components
- advanced demo environment automation
- advanced AI-assisted operations analytics
- expanded integration marketplace


## Baseline vs Premium Clarification
To avoid classification conflicts:
- Governance baseline records (meetings/minutes/basic votes) are core.
- Advanced governance automation/pack is optional add-on.
- Basic reserve tracking is core; advanced reserve planning analytics is optional add-on.
- Issue reporting/package tracking/inspection tracking are optional add-ons and may remain design-now/implement-later until enabled and implemented.


## Canonical Classification Definitions
- Core: baseline capability expected in default platform operation.
- Optional Add-On: capability enabled only by entitlement/feature flag per tenant.
- Design-Now / Implement-Later: architecture agreed and documented now, implementation deferred.
- Future: not committed for current implementation phase.


## Dashboard and Analytics Classification

### Core
- role-context dashboard families
- baseline KPI cards/tables/calendars
- tenant-safe dashboard filtering
- baseline notification center behavior

### Design-Now / Implement-Later
- analytics aggregation layer (daily/monthly rollups)
- reusable widget query/provider layer
- add-on usage analytics and entitlement dashboards
- metadata-driven dashboard layout definitions
- reusable dashboard/widget configuration engine baseline

### Future
- tenant UI-driven dashboard builder
- advanced chart composition and reusable dashboard templates
- self-service dashboard personalization by role/profile


## Scope Clarification for v7 Operational Additions
Core:
- preventive maintenance baseline
- asset register baseline
- owner ledger baseline visibility from existing financial records
- inspection due/overdue visibility
- property manager operational documents baseline
- basic leasing relationship support

Optional Add-On or Design-Now / Implement-Later:
- leasing applications pipeline
- showing schedule
- advanced unit marketing workflows

Future:
- expanded marketing automation for leasing
- advanced pipeline scoring and forecasting


## Workforce Module Classification

Core architecture support:
- employee identity and invitation model
- workforce role model compatibility
- approval workflow compatibility
- employee document access via existing document engine
- employee notifications via existing notification engine

Optional Add-On:
- Employee Portal / Workforce module family

Design-Now / Implement-Later:
- pay statements visibility
- payroll export/import integration
- leave approvals
- workforce analytics
- holiday/legal notice management

Future:
- full payroll engine
- benefits management
- attendance geofencing
- recruiting
- performance reviews


## Workforce Payroll Withholding Scope Classification
Core architecture support:
- pay statement visibility and secure access controls
- paystub data presentation model (gross/net/YTD fields)
- auditability and permission boundaries for payroll-visible artifacts

Design-Now / Implement-Later:
- configurable withholding/tax retention rules engine
- effective-date-aware payroll rule evaluation
- tenant-specific withholding overrides (if policy allows)
- employee/contractor classification-aware withholding extensions

Future (unless promoted later):
- full payroll execution engine
- advanced tax/regulatory packs by jurisdiction



## Professional Services Billing Scope Classification

### Optional Add-On Module
- Professional Services Billing / Time / Invoice

### Design Now / Implement Later
Advanced behaviors for this module family are architecture-approved now but deferred unless promoted later:
- dynamic effective-dated rate schedules
- fixed-fee and hourly mixed invoices
- withholding/tax retention versioning and snapshots
- automatic invoice numbering profiles
- invoice PDF template variants (detailed, summary, grouped)
- partial payment allocation workflows
- invoice/payment analytics and export views

### Classification Note
This module family is optional add-on / monetizable and is not part of the MVP core platform by default.

