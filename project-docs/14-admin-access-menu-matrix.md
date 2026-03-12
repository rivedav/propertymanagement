# Administration Roles and Menu Access Matrix

## Purpose
Operational menu and dashboard access matrix.

Source-of-truth references:
- Role definitions and contextual rules: `project-docs/02-roles-and-access.md`
- Tenant isolation and access resolution rules: `project-docs/03-multitenancy-rules.md`
- Data entities and dashboard drivers: `project-docs/05-data-model.md`

## Context Dimensions
- Tenant ID: organization boundary.
- User ID: identity reference.
- Dashboard Context: platform admin, tenant admin, owner, resident/renter, leasing agent, staff/property manager.
- Relationship Window: active `unit_relationships` date range.
- Contract Window: active `contract_assignments` date range.
- Permission Scope: role capability + assignment scope.

## Context-Aware Access Matrix
| Module Area | Platform Admin | Tenant Admin | Property Manager / Staff | Owner Context | Resident / Renter Context | Leasing Agent Context |
|---|---|---|---|---|---|---|
| Platform dashboard and tenant lifecycle | Full | None | None | None | None | None |
| Tenant dashboard and tenant settings | Oversight | Full tenant | Limited tenant | None | None | None |
| Properties / buildings / units | Oversight | Full tenant | Scoped manage | View owned scope | View occupied scope | View assigned scope |
| Relationships / leases / assignments | Oversight | Full tenant | Scoped manage | View owned units and related leases | View own active lease/relationship | View own active assignments |
| Financial collections (charges/payments/allocations) | Oversight | Full tenant | Scoped manage | View own charges/payments | View own charges/payments | None unless assignment scope grants |
| Accounts payable (vendor bills/payments) | Oversight | Full tenant | Scoped manage | None | None | None unless assignment scope grants |
| Budgets and reserve planning | Oversight | Full tenant | Limited or view by policy | View if granted | None | None |
| Governance (boards, meetings, minutes, votes) | Oversight | Full tenant | Limited by policy | View if granted | View published outputs only | None |
| Templates (email/notice/reminder) | Oversight | Full tenant | Limited manage | None | None | None |
| Maintenance and work orders | Oversight | Full tenant | Manage | Create/view own scope | Create/view own scope | View/update assigned scope |
| Amenities and reservations | Oversight | Full tenant | Manage schedules/approvals | View availability and own reservations | View availability and own reservations | View only if explicitly assigned |
| Community activities calendar | Oversight | Full tenant | Manage | View | View | View if scope grants |
| Announcements and notices | Oversight | Full tenant | Manage | View audience scope | View audience scope | View audience scope |
| Reports and analytics | Platform-wide | Tenant-wide | Operational scope | Owner scope | Resident scope | Assignment scope |
| Audit logs | Full | Tenant scope | Limited scope | None | None | None |
| Professional services billing (clients/time/expenses/invoices/payments) | Oversight | Full tenant if enabled | Scoped create/manage/approve if granted | None | None | Limited time/expense submission only if explicitly assigned |

## Notes
- This matrix defines expected UI/module exposure by context; detailed permission logic stays in source-of-truth docs.
- Every query must remain tenant-scoped and context-scoped.
- Same user can hold multiple active contexts in one tenant (owner of Unit A, renter of Unit B, leasing agent of Unit C).
- Contract and relationship date validity must be enforced at authorization time.
## Mindmap v4 Role Menu Snapshot (Alignment)
This snapshot mirrors the v4 mindmap and complements the matrix above.

Navigation reference only: permission authority is defined by the matrix and the source-of-truth docs (02/03/05).

### Platform Admin menu
- System Dashboard
- Tenant Management
- User Management
- System Settings
- Audit Logs
- Billing / Platform Fees
- Reports

### Tenant Admin menu
- Tenant Dashboard
- Properties / Buildings / Units
- Residents / Owners / Leases
- Charges & Payments
- Accounts Payable
- Budget & Finance
- Vendors & Contractors
- Maintenance Requests / Work Orders
- Board of Directors
- Meetings & Minutes
- Announcements
- Email & Notice Templates
- Amenities & Reservations
- Community Activities Calendar
- Documents & Media
  - Document Library
  - Media Gallery
  - Forms & Templates
- Reports
- Tenant Settings

### Property Manager menu
- Operations Dashboard
- Properties & Units
- Residents
- Leases
- Maintenance
- Amenities / Reservations
- Announcements
- Documents
- Operational Reports

### Owner dashboard
- Owned Units
- Financial Statements
- Charges & Payments
- Notices
- Amenities / Activities
- Ownership Documents
- Financial Documents

### Resident dashboard
- Lease Summary
- Payments
- Maintenance Requests
- Notices
- Amenities / Activities
- Community Documents
- Community Photos

### Leasing Agent dashboard
- Assigned Units
- Listings / Showings
- Lease Applications
- Prospective Tenants
- Contract Period Details
- Documents


## Mindmap v5 Role Menu Snapshot (Alignment)
This snapshot mirrors the v5 mindmap and complements the matrix above.
Navigation reference only: permission authority is defined by the matrix and source-of-truth docs (02/03/05).

### Tenant Admin menu additions (v5)
- Vendor Management
  - Vendor Directory
  - Service Contracts
  - Insurance / License Expiry
  - Vendor Ratings
  - Contract Expiry Alerts
  - Assigned Work
- Projects
  - Project Registry
  - Quotes / Bids
  - Approvals
  - Milestones
  - Budgets
  - Progress Photos
- Reserve Fund Planning
  - Asset Life Forecast
  - Replacement Cost Forecast
  - Funding Targets
  - Reserve Analysis
- Governance Voting
  - Create Vote
  - Eligible Voters
  - Voting Deadline
  - Results
  - Resolutions
- Inspection Tracking
  - Fire Inspection
  - Elevator Inspection
  - Safety Inspection
  - Insurance Inspection
  - Inspection Reports
- Digital Forms
  - Renovation Request
  - Move-In / Move-Out
  - Parking Request
  - Pet Registration
  - Guest Registration
- Packages / Deliveries
  - Package Log
  - Resident Notification
  - Pickup Status
  - Locker / Desk Tracking
- Notification Center
  - Email Notifications
  - In-App Notifications
  - Reminders
  - Event Triggers


## v6 Permission Behavior Matrix (Core Reference)
This section defines menu + actions + approval + financial visibility behavior.

| Role | Menu Scope | Action Rights | Approval Rights | Financial Visibility |
|---|---|---|---|---|
| Tenant Admin | Full tenant admin menus + Administrative Access Dashboard | view/create/edit/approve/export/delete/manage | Full tenant approval workflows | Full tenant financial scope |
| Property Manager / Staff | Operational + limited admin modules | view/create/edit (approve/export/delete/manage only if granted) | Limited, grant-based | Limited-by-default; optional budget/expense/collections views |
| Owner | Owner dashboard modules | view/export + limited create for owner workflows | None by default | Maintenance fee status, due dates, overdue alerts, payment history, statements, reminders |
| Resident / Renter | Resident dashboard modules | view/create for resident workflows | None by default | Due alerts, payment history, maintenance status, reservation visibility |
| Leasing Agent / Realtor | Assignment-scoped modules | view/create/edit within assignment scope | Optional if granted | Usually none unless explicitly granted |


## v6+ Action Coverage Addendum
The access matrix must explicitly account for:
- publish
- approve
- schedule
- template management
- content workflow access

Example extension fields for role evaluation:
- `can_publish`
- `can_approve`
- `can_schedule`
- `can_manage_templates`
- `can_manage_workflows`


## Website Admin / Platform Operations Menu (Addendum)
Operational areas to include:
- Monitoring Dashboard
- Maintenance Mode Control
- Forced Logout Control
- Backup / Restore
- Tenant Feature Enablement
- Issue Reporting Control
- Future Messaging Control (planned)

Each area should map to actions such as:
- view
- configure
- execute
- approve
- export


## Website Admin Monetization Areas (Addendum)
- Add-On Catalog
- Bundles / Offers
- Tenant Entitlements
- Tenant Billing and Collections
- Suspension / Reactivation Control
- Listings Publication Controls

Action examples:
- view
- create
- edit
- enable/disable
- approve
- export
- suspend/reactivate


## Website Admin Add-On Classification Controls
Website Admin must classify each module as:
- core
- optional add-on
- design-now/implement-later
- future

Control actions:
- include-by-default
- optional-for-purchase
- hidden/not-offered
- tenant-enabled / tenant-disabled
- suspend/restore tenant access by billing/account state


### Optional Add-On Enforcement Rule
Website Admin controls tenant activation for optional modules.
If a module is optional and not enabled for a tenant:
- menu entries are hidden/disabled,
- actions are blocked by authorization/entitlement checks,
- historical data remains preserved for possible reactivation.


## Website Admin Plan/Lifecycle/Support Controls (Addendum)
Add menu/control areas:
- Plan and Package Management
- Plan Feature Matrix
- Tenant Plan Assignment
- Tenant Lifecycle Management
- Export and Offboarding Center
- Support Impersonation Control
- Branding and Landing Builder Governance


## Admin Terminology Normalization
Canonical term:
- Platform Admin (Website Admin)

Use this combined term in this file and references to avoid naming drift.


## Analytics and Notification Menu Areas (Addendum)
Add areas for:
- Platform Analytics
- Tenant Analytics
- Add-On / Feature Status
- Notifications Center

Access behavior:
- visibility/action scope follows role + entitlement + context
- analytics and notification views must remain tenant-safe


## v7 Dashboard and Operations Addendum
Platform Admin (Website Admin) dashboard KPI names are canonical:
- revenue overview
- tenant health
- add-on adoption
- storage usage
- platform usage
- system activity
- security logs
- export center

Tenant/Admin Operations additions:
- preventive maintenance
- asset register
- owner ledger
- inspection due widget
- operational documents
- leasing pipeline controls (scoped by role)

Owner Ledger visibility rules:
- owner-facing ledger is scoped to the owner's own properties only
- tenant admin may see owner financial records according to role
- property manager/staff access only if explicitly permitted


## Workforce Menu Areas (Addendum)
Platform Admin (Website Admin):
- Platform Workforce
- Platform HR/Admin
- Workforce Analytics (platform scope)

Tenant Admin / Tenant HR/Admin:
- Tenant Workforce Directory
- Time Approvals
- Leave Approvals
- Workforce Reports
- Payroll Export/Import Integration (if enabled)

Tenant Employee / Supervisor:
- Employee Dashboard
- Time & Attendance
- Leave / Requests
- Holidays / Legal Notices
- Pay Statements (if enabled)


## Workforce Payroll Rule Governance (Addendum)
Authorized administration:
- Platform HR/Admin: platform-workforce withholding rules
- Tenant HR/Admin: tenant-workforce withholding rules

Access and action boundaries:
- can_view_paystatements
- can_manage_withholding_rules
- can_activate_rule_versions
- can_manage_rule_overrides (if permitted)
- can_export_payroll_statements

Restrictions:
- no cross-tenant payroll rule administration
- paystub access must follow workforce scope and explicit permissions




## Professional Services Billing Menu Areas (Addendum)
Tenant Admin menu additions when module is enabled:
- Professional Services Billing
  - Clients
  - Service Types
  - Projects
  - Rates
  - Time Entries
  - Billable Expenses
  - Invoices
  - Payments Received
  - Withholding Rules
  - Invoice Templates
  - Numbering Profiles
  - Billing Reports

Property Manager / Staff menu additions if enabled by tenant policy:
- Time Entries
- Billable Expenses
- Invoice Draft Queue
- Billing Reports (limited)

Permission/action examples:
- `can_manage_clients`
- `can_manage_rate_schedules`
- `can_approve_time_entries`
- `can_approve_billable_expenses`
- `can_generate_invoices`
- `can_manage_invoice_numbering`
- `can_apply_payments`
- `can_void_invoices`
- `can_export_invoice_pdf`


## Dashboard Configuration Governance (Addendum)
Platform Admin (Website Admin):
- Platform Dashboard Profiles
- Global Widget Registry
- Analytics Layout Governance

Tenant Admin:
- Tenant Dashboard Profiles
- Module Dashboard Profiles
- Widget Visibility Rules

Permission/action examples:
- `can_view_dashboards`
- `can_manage_dashboard_profiles`
- `can_manage_dashboard_widgets`
- `can_manage_widget_filters`
- `can_manage_widget_series`
- `can_export_widgets`

Visibility rule:
- dashboard and widget rendering must respect role, permission, entitlement, and active context.


## Historical Review and Continuity Views (Addendum)
Tenant Admin and authorized oversight roles should have historical review access for administrative continuity and relationship changes.

Recommended historical admin views:
- Tenant Users History
- Ownership History
- Tenant Administration History
- Tenant Name / Alias History
- Board Member Term History
- Agent / Assignment History

Table expectations:

Tenant Users table:
- name
- email
- role
- status
- tenant
- start date
- end date
- active/current flag

Ownership History table:
- property/unit
- old owner
- new owner
- effective dates
- status

Tenant Administration History table:
- tenant
- administrator
- role
- start date
- end date
- active/current flag

Tenant Name / Alias History table:
- tenant
- previous name
- current name
- effective date
- current flag

Visibility rule:
These views must preserve historical records and avoid destructive replacement. Current operational access may use active records, but historical review remains available to authorized roles.


## Canonical Terminology Note
This file should use:
- `Platform Admin (Website Admin)`
- `Resident / Renter`
- `Leasing Agent`
- `Property Manager / Staff`

Legacy aliases such as `Website Admin` or `Realtor` may appear in older notes, but canonical references should use the terms above.


## Access Model Consistency Note
Menu and dashboard visibility in this file assumes the canonical access model defined in:
- `project-docs/02-roles-and-access.md`
- `project-docs/04-architecture.md`

Effective exposure should be understood as the result of:
- role template
- access profile
- scope
- relationship
- add-on entitlements
- overrides
- active/effective dates
