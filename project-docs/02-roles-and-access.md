# Roles and Access

## Identity and Access Principles
- User identity is global (`users`), but permissions are contextual.
- A single user may belong to multiple tenants.
- The same user may hold multiple concurrent roles in one tenant.
- Effective access is never resolved from one role alone.

## 1) Platform-Level Roles
- Platform Admin

Scope:
- Platform-wide oversight, tenant lifecycle, support, system settings, global audit access.
- Does not bypass tenant data boundaries unless explicitly authorized by platform policy.

## 2) Tenant Organization Roles
- Tenant Admin
- Property Manager
- Staff
- Accounting Staff
- Maintenance Staff
- Secretary
- Board Member
- Security

Scope:
- Tenant operations, staff coordination, governance workflows, finance operations, announcements, documents, settings.
- Permissions are tenant-scoped and policy-driven.

## 3) Unit / Property Relationship Roles
- Owner
- Resident
- Rent Tenant / Renter
- Landlord
- Co-owner
- Emergency Contact

Scope:
- Unit/property-specific visibility and actions.
- May coexist with tenant org role(s), or exist without administrative membership.

## 4) Contract-Based Roles
- Leasing Agent / Realtor
- Vendor Contact
- Contractor Assignment

Scope:
- Time-bound, scope-bound operational access via contracts/assignments.
- Can grant limited access without standard tenant admin role.

## Access Resolution Model
Effective permissions are resolved using:
1. selected tenant
2. selected dashboard context
3. active unit relationship(s)
4. active contract assignment(s)
5. role type(s) and permission scope
6. date validity windows

## Dashboard Context Rules
A user can switch contexts based on active records:
- Platform Admin Dashboard
- Tenant Admin Dashboard
- Owner Dashboard
- Resident / Renter Dashboard
- Leasing Agent Dashboard
- Staff / Property Manager Dashboard

Menus and modules must adapt per active context.

### Contextual Menu Considerations
- Financial management: charges, payments, payment allocations, vendor bills/payables, budgets, reserve tracking.
- Governance: board members, meeting agendas/minutes, votes/resolutions.
- Templates: email/notice/reminder templates by tenant policy.
- Amenities and reservation calendar: amenity setup, reservation rules, approvals, schedule control.
- Community activities calendar: recurring events, duration, publish visibility, resident/owner access.

## Multi-Tenant Participation Example
A single global user can hold different contexts simultaneously, for example:
- owner in Condo A
- resident in Condo B
- realtor contracted by Condo Z
- renter in Building D

## Optional External Roles
- Vendor / Contractor (optional)
These roles are typically contract-based and time-bounded, and should be resolved from active assignment scope.

## Access Dependency Checklist
This checklist is normative and must be enforced by authorization rules and active date windows (leases, ownership records, and contract periods).


## Tenant Administrator as Access Controller (v6)
Tenant Admin is the tenant-level authority for user administration and access governance.

Administrative Access Dashboard capabilities:
- users by role
- permission matrix
- role templates
- custom role overrides
- invite / disable / reactivate users
- approval workflow rules
- node/screen/window/utility access controls
- rights by action: view, create, edit, approve, export, delete, manage

## Permission Matrix Model (v6)
Permissions are managed by:
- role
- module
- action

Suggested module set:
- charges, payments, budgets, meetings, documents, reservations,
  community activities, maintenance requests, vendor bills, reports,
  templates, audit logs

## Contextual Financial Visibility Rules (v6)
- Owner dashboards must include maintenance fee status, due dates, overdue alerts, payment history, downloadable statements, reminder/alarm status.
- Property Manager / Staff has no full financial control by default.
- Limited financial visibility is optional and grant-based:
  - budget snapshot
  - expense view
  - collections view
  - operational reports
- Resident/Renter dashboards may include due alerts, payment history, maintenance request status, reservation visibility.


## Drupal-Style Privileges Model
The platform follows a Drupal-inspired role/permission approach:
- permissions are defined by role + module + action
- privileges are contextual by tenant and relationship state
- approval hierarchy can restrict sensitive actions by authority level

Standard action examples:
- view
- create
- edit
- modify
- delete
- approve
- publish
- schedule
- export
- manage

Module examples:
- announcements
- documents
- media
- meetings
- votes
- charges
- payments
- budgets
- reservations
- maintenance
- templates
- audit logs

## Publishing and Workflow Permissions
Publishing-related permissions are explicit and separable:
- publish
- schedule
- unpublish/archive
- approve-before-publish

Higher-authority approval may be required by:
- Tenant Admin
- Board of Directors
- configured high-authority roles


## Platform Operations Permissions (Website Admin / Platform Admin)
Platform Admin operational powers include:
- configure session timeout policy
- force logout (global or tenant-scoped)
- put platform or tenant in maintenance/interruption mode
- publish maintenance/interruption notices (global or tenant-targeted)
- monitor platform and tenant health
- run/approve backup and restore operations
- enable/disable tenant feature modules (for packaging/billing strategy)
- enable/disable client issue reporting per tenant

These controls are infrastructure-level and must be audited.


## Support Impersonation and Auditable Access
Platform Admin support mode may allow controlled "view-as-tenant" / impersonation for troubleshooting.

Rules:
- support mode is restricted, time-bounded, and permission-scoped
- all impersonation events are auditable
- audit fields include actor, target tenant/user, reason, and timestamps
- support mode cannot bypass audit requirements


## Workforce Role Set (Distinct Context)
Workforce roles:
- Platform Employee
- Platform HR/Admin
- Tenant Employee
- Tenant Supervisor
- Tenant HR/Admin

Role boundary rule:
These workforce roles are distinct from:
- Owner
- Resident / Renter
- Board Member
- Leasing Agent

Access resolution remains contextual by tenant/workforce scope and granted permissions.


## Payroll Rule and Paystub Access Controls
Payroll withholding rule changes must be restricted to authorized workforce admin roles:
- Platform HR/Admin (platform scope)
- Tenant HR/Admin (tenant scope)

Control requirements:
- permission-controlled create/edit/activate/deactivate/version actions
- effective-date-aware rule governance
- full audit trail for rule changes and approvals

Paystub visibility rules:
- employees can view their own pay statements only
- supervisors may view scoped team payroll views only if granted
- tenant HR/Admin access is tenant-scoped
- platform HR/Admin access is platform-workforce scoped



## Professional Services Billing Access Context
This module family introduces business-billing roles and permissions that remain tenant-scoped and permission-driven.

Suggested tenant-scoped access patterns:
- Tenant Admin: full configuration and oversight
- Accounting Staff: client billing, invoice generation, payment application, reporting
- Staff / Property Manager: time and expense entry within granted scope
- Board Member: oversight/reporting only if explicitly granted
- Contractor Assignment or external service user: limited time/expense submission only if assignment policy allows

Suggested module/action coverage:
- clients
- service types
- projects
- rates
- time entries
- expenses
- invoices
- payments received
- withholding rules
- numbering profiles
- invoice templates

Suggested actions:
- view
- create
- edit
- approve
- generate_invoice
- send
- export
- apply_payment
- void
- manage

Historical-safety rule:
Users who can manage rates, withholding rules, or invoice numbering preferences must not be able to retroactively alter historical invoice meaning. Historical invoices must preserve their own rate and withholding snapshots.


## Historical Identity and Assignment Continuity
Access and relationship resolution must preserve historical continuity rather than replacing prior records.

This historical tracking applies to:
- tenant administrators
- board members
- property owners
- residents / renters
- realtors / leasing agents
- website/platform administrators where applicable

Rules:
- a tenant administrator change must create a new assignment period rather than overwriting the prior administrator record
- board member changes must preserve prior terms and effective dates
- owner/resident/renter changes must preserve prior relationship periods
- realtor/leasing-agent changes must preserve prior assignment scope and dates
- access evaluation for current rights uses active/current records, while audit/history views must preserve expired and superseded records
- stable master entities such as user identity, tenant organization, property, building, and unit must not be replaced unnecessarily when relationships or personnel change


## Canonical Terminology
Use these terms consistently in access documentation:
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

Normalization rules:
- `Website Admin` should be written as `Platform Admin (Website Admin)`.
- `Resident`, `Renter`, and `Resident/Renter` should be normalized to `Resident / Renter` unless quoting a legacy field or role.
- `Leasing Agent / Realtor` should be normalized to `Leasing Agent` in canonical prose, with `Realtor` treated as an alias when needed.
- `Property Manager / Staff` should be normalized by using `Property Manager / Staff` consistently in matrix-style docs and `Property Manager` or `Staff` only when individually intended.


## Canonical Access Resolution Model
Users may hold multiple roles and contexts at the same time.

Effective access is resolved by combining:
1. role template
2. access profile
3. scope
4. relationship
5. add-on entitlements
6. overrides
7. active/effective dates

Interpretation rules:
- role template defines baseline capabilities
- access profile refines usable permissions/menu exposure for a context
- scope limits where access applies
- relationship records grant contextual rights tied to tenancy, ownership, occupancy, leasing, or workforce assignment
- add-on entitlements gate module availability
- overrides refine or restrict default access
- current access uses active/effective records only, while historical records remain queryable
