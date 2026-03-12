# Data Model

## Overview
This model is organized into six delivery modules:

1. Identity and memberships  
2. Unit relationships and leases  
3. Finance receivables  
4. Finance payables and budgets  
5. Governance and meetings  
6. Amenities and calendar  

Design principles:
- one global user across many tenants
- tenant-scoped business records
- context-aware authorization
- time-based relationship and assignment validity
- auditable business state transitions

---

## 1) Identity and Memberships

### Business problem it solves
Define who the user is globally and what organizational access they have per tenant.

### Main entities / tables
- `users`
- `tenants`
- `tenant_memberships`
- `audit_logs`
- `audit_log_field_changes` (recommended for field-level diffs)

### Minimum suggested columns

#### users
- id
- email
- full_name
- phone
- created_at
- updated_at

#### tenants
- id
- slug
- name
- status
- created_at
- updated_at

#### tenant_memberships
- id
- tenant_id
- user_id
- org_role
- status
- created_at
- updated_at

#### audit_logs
- id
- tenant_id (nullable for platform events)
- actor_user_id
- action_type
- entity_type
- entity_id
- performed_at
- ip_address (optional)
- user_agent (optional)
- reason_or_note (optional)
- created_at

#### audit_log_field_changes
- id
- audit_log_id
- field_name
- old_value
- new_value
- created_at

### How it connects to the rest of the system
- All modules reference `users` and `tenant_id`.
- Membership is baseline access for tenant admin/staff contexts.
- Audit tables are cross-cutting and capture business changes from all modules.

---

## 2) Unit Relationships and Leases

### Business problem it solves
Represent who is connected to units/properties and when those relationships are valid.

### Main entities / tables
- `properties`
- `buildings`
- `units`
- `unit_relationships`
- `leases`
- `contract_assignments`

### Minimum suggested columns

#### properties
- id
- tenant_id
- name
- property_type
- address_line_1
- city
- state
- postal_code
- country
- status
- created_at
- updated_at

#### buildings
- id
- tenant_id
- property_id
- name
- code
- status
- created_at
- updated_at

#### units
- id
- tenant_id
- property_id
- building_id (nullable)
- unit_number
- floor
- unit_type
- status
- created_at
- updated_at

#### unit_relationships
- id
- tenant_id
- unit_id
- user_id
- relationship_type
- start_date
- end_date
- is_primary
- status
- notes
- created_at
- updated_at

#### leases
- id
- tenant_id
- unit_id
- landlord_user_id (nullable)
- tenant_user_id
- lease_start
- lease_end
- rent_amount
- payment_frequency
- deposit_amount (nullable)
- status
- created_at
- updated_at

#### contract_assignments
- id
- tenant_id
- unit_id (nullable)
- principal_user_id (nullable)
- assigned_user_id
- contract_type
- start_date
- end_date
- permission_scope_json
- status
- notes
- created_at
- updated_at

### How it connects to the rest of the system
- Drives owner/resident/renter/leasing-agent dashboard contexts.
- Controls receivables target users/units.
- Provides assignment scope for operations and payables interactions.

---

## 3) Finance Receivables

### Business problem it solves
Track money owed by owners/residents/renters and how payments are applied.

### Main entities / tables
- `charges`
- `payments`
- `payment_allocations`

### Minimum suggested columns

#### charges
- id
- tenant_id
- unit_id (nullable)
- billed_user_id
- charge_type
- amount
- due_date
- status
- created_by
- created_at
- updated_at

#### payments
- id
- tenant_id
- payer_user_id
- amount
- payment_method
- payment_date
- reference_number (nullable)
- status
- created_by
- created_at
- updated_at

#### payment_allocations
- id
- tenant_id
- payment_id
- charge_id
- allocated_amount
- created_by
- created_at
- updated_at

### How it connects to the rest of the system
- Uses relationships/leases to determine who should be charged.
- Feeds owner and resident/renter dashboards.
- Feeds reporting and budget-vs-actual analysis.

---

## 4) Finance Payables and Budgets

### Business problem it solves
Manage vendor obligations, outgoing payments, budget control, and reserve planning.

### Main entities / tables
- `vendors`
- `vendor_bills`
- `vendor_payments`
- `budgets`
- `budget_lines`
- `maintenance_requests` (payable source context)
- `work_orders` (payable source context)

### Minimum suggested columns

#### vendors
- id
- tenant_id
- name
- vendor_type
- contact_name
- contact_email
- contact_phone
- status
- created_at
- updated_at

#### vendor_bills
- id
- tenant_id
- vendor_id
- property_id (nullable)
- work_order_id (nullable)
- bill_number
- bill_date
- due_date
- amount
- status
- approved_by (nullable)
- created_by
- created_at
- updated_at

#### vendor_payments
- id
- tenant_id
- vendor_bill_id
- amount
- payment_date
- payment_method
- reference_number (nullable)
- status
- created_by
- created_at
- updated_at

#### budgets
- id
- tenant_id
- fiscal_year
- period_type
- total_amount (nullable)
- reserve_target (nullable)
- status
- approved_by (nullable)
- created_by
- created_at
- updated_at

#### budget_lines
- id
- tenant_id
- budget_id
- category_code
- category_name
- planned_amount
- actual_amount
- period_month (nullable)
- created_at
- updated_at

### How it connects to the rest of the system
- `work_orders` and vendor assignments influence AP.
- Budget approval links to governance roles.
- Financial state changes must be strongly auditable.

---

## 5) Governance and Meetings

### Business problem it solves
Support board governance, formal meeting records, decisions, and official communications.

### Main entities / tables
- `boards`
- `board_members`
- `meetings`
- `meeting_minutes`
- `votes`
- `email_templates`
- `announcements`

### Minimum suggested columns

#### boards
- id
- tenant_id
- name
- term_start (nullable)
- term_end (nullable)
- status
- created_at
- updated_at

#### board_members
- id
- tenant_id
- board_id
- user_id
- position
- start_date
- end_date (nullable)
- status
- created_at
- updated_at

#### meetings
- id
- tenant_id
- board_id (nullable)
- meeting_type
- title
- scheduled_start
- scheduled_end
- location (nullable)
- status
- created_by
- created_at
- updated_at

#### meeting_minutes
- id
- tenant_id
- meeting_id
- minutes_text
- status
- created_by
- approved_by (nullable)
- created_at
- updated_at

#### votes
- id
- tenant_id
- meeting_id
- motion_title
- result
- yes_count
- no_count
- abstain_count
- created_by
- created_at
- updated_at

#### email_templates
- id
- tenant_id
- template_type
- name
- subject (nullable)
- body
- status
- created_by
- created_at
- updated_at

#### announcements
- id
- tenant_id
- title
- body
- audience_scope_json
- status
- published_at (nullable)
- created_by
- created_at
- updated_at

### How it connects to the rest of the system
- Governance actions rely on membership/role permissions.
- Announcements and templates feed tenant/resident/owner communications.
- Meeting/calendar artifacts integrate with amenities/calendar timeline views.

---

## 6) Amenities and Calendar

### Business problem it solves
Manage shared amenity usage, reservation lifecycle, and tenant community scheduling.

### Main entities / tables
- `amenities`
- `amenity_reservations`
- `community_events`
- `calendar_entries`

### Minimum suggested columns

#### amenities
- id
- tenant_id
- property_id (nullable)
- name
- amenity_type
- requires_approval
- reservation_rules_json
- status
- created_by
- created_at
- updated_at

#### amenity_reservations
- id
- tenant_id
- amenity_id
- requested_by_user_id
- start_at
- end_at
- duration_minutes
- approval_status
- approved_by (nullable)
- status
- created_by
- created_at
- updated_at

#### community_events
- id
- tenant_id
- title
- description (nullable)
- start_at
- end_at
- duration_minutes
- is_recurring
- recurrence_rule (nullable)
- visibility_scope
- approval_status
- created_by
- created_at
- updated_at

#### calendar_entries
- id
- tenant_id
- source_type
- source_id
- title
- start_at
- end_at
- duration_minutes
- visibility_scope
- status
- created_by
- created_at
- updated_at

### How it connects to the rest of the system
- Resident/owner dashboards use this for availability and participation.
- Staff/property manager workflows handle approvals/scheduling conflicts.
- Governance meetings and operational events can appear in unified calendar views.

---

## Audit Trail Requirements (Platform-Wide)

Business audit trail must capture:
- tenant_id
- actor_user_id
- action_type
- entity_type
- entity_id
- field_name changes where applicable
- old_value
- new_value
- performed_at
- optional ip_address
- optional user_agent
- optional reason_or_note

Distinction:
- Business audit trail: business-state and authorization-relevant changes.
- Technical/system logs: runtime/infrastructure telemetry.

High-priority auditable domains:
- role/permission changes
- leases and ownership changes
- reservations and approvals
- budgets and budget lines
- board records, meetings, minutes, votes
- announcements lifecycle
- maintenance/work-order status changes
- receivables and payables changes

## Dashboard Mapping (Driver Summary)
- Platform Admin Dashboard: tenants + platform controls + cross-tenant audit oversight.
- Tenant Admin Dashboard: memberships + operations + finance + governance + amenities/calendar.
- Owner Dashboard: active ownership relationships + receivables + notices + amenities/calendar.
- Resident / Renter Dashboard: active lease/relationship + receivables + maintenance + amenities/calendar.
- Leasing Agent Dashboard: active contract assignments + assigned unit scope.
- Property Manager / Staff Dashboard: operations/work orders/reservations + scoped finance/governance visibility.
## Required Domain Crosswalk
| Required Domain | Data Model Location |
|---|---|
| Identity and memberships | Module 1 |
| Unit ownership and leases | Module 2 |
| Financial receivables | Module 3 |
| Financial payables and building budgets | Module 4 |
| Condominium governance and board management | Module 5 |
| Amenities reservations and community calendar | Module 6 |
| Audit trail and reporting system | Audit sections below |

## Ownership and Lease Submodule Notes
- Ownership records are modeled via `unit_relationships` using `relationship_type` values like `owner`, `co_owner`, and `landlord`.
- Lease lifecycle is modeled via `leases`, with supporting occupant context in `unit_relationships`.
- Contract-bound leasing/realtor access is modeled via `contract_assignments`.

## Documents and Attachments (Cross-Cutting Module)
### Purpose
Store tenant legal, operational, board, and communication files with controlled visibility.

### Main entities
- `documents`
- `document_folders`
- `document_permissions`
- `document_versions`
- `media_library`
- `media_albums`

### Minimum suggested columns
#### document_folders
- id
- tenant_id
- name
- parent_folder_id (nullable)
- visibility_scope
- created_by
- created_at
- updated_at

#### documents
- id
- tenant_id
- folder_id (nullable)
- title
- document_type
- storage_path
- mime_type
- visibility_scope
- created_by
- created_at
- updated_at

#### document_permissions
- id
- tenant_id
- document_id
- role_scope
- user_id (nullable)
- can_view
- can_download
- can_edit
- created_at

#### document_versions
- id
- tenant_id
- document_id
- version_number
- storage_path
- uploaded_by
- uploaded_at
- change_note (nullable)

#### media_albums
- id
- tenant_id
- name
- visibility_scope
- created_by
- created_at
- updated_at

#### media_library
- id
- tenant_id
- album_id (nullable)
- title
- media_type
- storage_path
- tags_json
- visibility_scope
- created_by
- created_at
- updated_at

### Relation to other modules
- Links to leases, governance records (meetings/minutes), announcements, and templates.
- Visibility uses role scopes: `public`, `resident`, `owner`, `board`, `staff`.
### v4 Navigation Alignment: Community Documents & Media Library
This module supports the UI concept:
- Documents & Media
  - Document Library
  - Media Gallery
  - Forms & Templates

Recommended document/media classification fields:
- `library_section` (document_library, media_gallery, forms_templates)
- `document_category` (public_documents, community_documents, board_documents, ownership_documents, financial_documents)
- `visibility_scope` (public, resident, owner, board, staff)

Additional document/media behavior flags:
- `is_versioned` (bool)
- `allow_pdf_export` (bool)
- `allow_media_upload` (bool)
- `tags_json` (for file tagging/search)

## Audit Trail and Reporting System (Core Platform Module)
This module extends the audit requirements already defined above with query/reporting capabilities:
- audit dashboard and audit table
- security events view
- financial audit view
- configuration changes view
- dynamic filtering and search
- CSV export and PDF export/print



## v5 Expansion Modules

## 7) Vendor Management
### Business problem it solves
Manage third-party service providers with contractual and compliance visibility.

### Main entities / tables
- `vendors`
- `vendor_contracts`
- `vendor_ratings`

### How it connects to the rest of the system
- Links to payables, maintenance/work orders, inspections, and capital projects.
- Drives contract-expiry alerts and assigned work visibility.

## 8) Capital Projects
### Business problem it solves
Track large upgrade/repair initiatives from planning to completion.

### Main entities / tables
- `capital_projects`
- `project_bids`
- `project_milestones`
- `project_media` (progress photos)

### How it connects to the rest of the system
- Uses governance approvals, budget lines, vendor contracts, and notifications.

## 9) Reserve Fund Planning
### Business problem it solves
Plan long-term funding for major replacements and lifecycle costs.

### Main entities / tables
- `reserve_assets`
- `reserve_forecasts`
- `reserve_funding_targets`
- `reserve_analyses`

### How it connects to the rest of the system
- Feeds budgets, board decisions, and financial reporting.

## 10) Inspections
### Business problem it solves
Track compliance inspections and resulting actions.

### Main entities / tables
- `inspections`
- `inspection_reports`

### How it connects to the rest of the system
- Links to properties/buildings/units, vendors/contractors, work orders, and notification reminders.

## 11) Digital Forms
### Business problem it solves
Standardize operational request intake and approvals.

### Main entities / tables
- `forms`
- `form_submissions`

### How it connects to the rest of the system
- Connects to approvals, documents, notifications, and tenant operations.

## 12) Packages / Deliveries
### Business problem it solves
Track inbound package lifecycle and resident pickup status.

### Main entities / tables
- `packages`
- `package_events`

### How it connects to the rest of the system
- Uses resident context, notification center, and operational dashboard queues.

## 13) Notification Center
### Business problem it solves
Centralize user-facing reminders and event-driven messages.

### Main entities / tables
- `notifications`
- `notification_templates`
- `notification_events`

### How it connects to the rest of the system
- Triggered by finance due dates, reservations, inspections, packages, forms, governance deadlines.

## 14) Enhanced Documents & Media
### Business problem it solves
Support public/private document repository and media gallery with governance/financial segmentation.

### Main entities / tables
- `documents`
- `document_versions`
- `media_assets`
- `document_folders`
- `document_permissions`
- `media_albums`

### How it connects to the rest of the system
- Supports board documents, financial documents, public/community documents, forms/templates.

## 15) Audit Trail and Reporting (Strengthened)
### Required audit fields
- tenant_id
- actor_user_id
- action_type
- entity_type
- entity_id
- field_name
- old_value
- new_value
- performed_at
- ip_address (optional)
- user_agent (optional)
- reason (optional)

### Required audit capabilities
- date/date-range filters
- dynamic filtering
- CSV export
- PDF export
- printable reports


## v6 Access-Control and User Administration Additions

### Access-control entities
- `roles`
- `permissions`
- `role_permissions`
- `user_role_overrides` (optional)
- `user_invitations` (invitation access flow)

### Suggested minimum columns

#### roles
- id
- tenant_id
- role_name
- is_template
- status
- created_by
- created_at
- updated_at

#### permissions
- id
- tenant_id
- module_key
- action_key
- description
- created_at
- updated_at

#### role_permissions
- id
- tenant_id
- role_id
- permission_id
- is_allowed
- approval_required
- created_by
- created_at
- updated_at

#### user_role_overrides
- id
- tenant_id
- user_id
- role_id (nullable)
- permission_id
- override_effect (allow/deny)
- reason
- start_at (nullable)
- end_at (nullable)
- created_by
- created_at
- updated_at

#### user_invitations
- id
- tenant_id
- email
- invited_role_id
- invited_by
- status
- expires_at
- accepted_at (nullable)
- created_at
- updated_at

### Permission-change auditability
Permission changes must be auditable via `audit_logs` + `audit_log_field_changes`:
- role assignment changes
- role_permission changes
- override changes
- invitation state transitions
- approval workflow configuration changes

### Permission-driven widgets and dashboards
- Owner financial widgets are permission-driven and context-aware.
- Property manager financial widgets are permission-driven and limited by default.
- Dashboard layout resolution uses role + permission matrix + active context.


## Drupal-Inspired Content and Permission Model Direction

### Additional likely entities (model direction only)
- `content_items`
- `content_workflows`
- `workflow_states`
- `approval_records`
- `email_templates` (already present)
- `template_variables`
- `roles` (already present)
- `permissions` (already present)
- `role_permissions` (already present)
- `user_role_overrides` (already present)

### Content workflow direction
`content_items` should support:
- title/body/rich content
- media and document attachments
- current_state
- publish_at
- unpublish_at
- created_by / approved_by
- tenant_id

### Template rendering direction
`template_variables` should define reusable placeholders, such as:
- tenant_name
- building_name
- unit_number
- resident_name
- owner_name
- due_amount
- due_date
- meeting_date
- reservation_date

### Approval records direction
`approval_records` should track:
- workflow subject (entity_type/entity_id)
- requested_by
- approver_user_id
- decision
- decided_at
- reason

### Permission-driven visibility note
Owner and property-manager finance widgets must be permission-driven, not role-assumed.


## Platform Operations and Recovery (Design Direction)
Purpose: support secure SaaS operations, tenant-safe controls, and shared-database recoverability.

Suggested entities/modules:
- `session_policies` (global/tenant session timeout rules)
- `forced_logout_events` (global/tenant/user scoped invalidation events)
- `maintenance_mode_controls` (platform/tenant interruption state)
- `maintenance_notices` and/or `system_announcements` (maintenance communication)
- `tenant_feature_flags` (module enable/disable per tenant)
- `monitoring_events` and `tenant_health_events` (operational telemetry records)
- `backup_jobs` (platform backup lifecycle)
- `restore_jobs` (restore requests/approvals/executions)
- `incident_reports` / `support_tickets` (tenant support and issue reporting)
- `message_threads`, `messages` (future module only)

Notes:
- No SQL migration in this phase.
- Recovery design must assume shared-database isolation and strict validation.
- All control-plane actions require audit coverage.


## Monetization and Entitlements (Design Direction)
Purpose: support reusable add-on sales, billing, entitlement activation, suspension controls, and data preservation.

Suggested entities/modules:
- `add_on_catalog`
- `add_on_bundles`
- `tenant_entitlements`
- `tenant_subscriptions`
- `platform_billing_rules`
- `tenant_billing_invoices`
- `payment_reminders`
- `suspension_events`
- `listings`
- `listing_publication_scopes`

Behavior notes:
- entitlement activation after payment confirmation
- deactivation blocks feature access but preserves historical data
- reactivation restores access to prior data state
- listing scope supports tenant-local or platform-wide publication


## Core vs Optional Add-On Entity Separation

### Core Platform Entities (Conceptual)
- users, tenants, tenant_memberships
- roles, permissions, role_permissions, user_role_overrides
- properties, buildings, units, unit_relationships, leases, contract_assignments
- charges, payments, payment_allocations
- vendors, vendor_bills, vendor_payments, budgets, budget_lines
- boards, board_members, meetings, meeting_minutes, announcements
- amenities, amenity_reservations, community_events, calendar_entries
- audit_logs, audit_log_field_changes
- tenant_feature_flags (core control-plane)
- dashboards, dashboard_profiles, dashboard_widgets, dashboard_widget_settings, dashboard_widget_filters, dashboard_widget_series, dashboard_widget_permissions, dashboard_layouts

### Optional Add-On Entities (Conceptual; entitlement-gated)
- contractor_management: contractor_profiles, contractor_assignments
- support/issues: incident_reports, support_tickets, ticket_events
- advanced maintenance: maintenance_workflows, workflow_steps
- packages: packages, package_events
- inspections: inspections, inspection_reports
- communications: sms_messages, notification_automations, emergency_notifications
- advanced content/storage: document_retention_rules, storage_tiers, media_assets_pro, document_versions_pro
- advanced finance: advanced_reports, accountant_workspaces, billing_management_rules, collections_automation_rules, reserve_planning_models
- governance add-ons: votes, vote_results, agenda_items, resolutions
- listings: listings, listing_publication_scopes, listing_promotions
- professional_services_billing: clients, client_contacts, client_billing_profiles, service_types, projects, rate_schedules, rate_schedule_versions, time_entries, time_entry_approvals, billable_expenses, expense_receipts, invoice_numbering_profiles, invoices, invoice_lines, invoice_templates, invoice_line_rate_snapshots, withholding_rules, withholding_rule_versions, client_withholding_assignments, invoice_line_withholding_snapshots, payments_received, payment_allocations, invoice_status_history

### Add-On Commerce and Entitlement Entities (Core Control-Plane)
- add_on_catalog
- add_on_bundles
- tenant_entitlements
- tenant_subscriptions
- platform_billing_rules
- tenant_billing_invoices
- payment_reminders
- suspension_events

### Classification Notes
- Optional add-on entities must be isolated by entitlement checks and module flags.
- Deactivation must not hard-delete tenant historical data by default.
- Reactivation re-enables access to preserved data under policy.


### Optional Add-On Catalog Classification (Explicit)
The following capability groups are classified as Optional Add-On modules (Website Admin enablement + entitlement required):

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
- professional services billing / time / invoice

Implementation note:
- These do not need one-table-per-item.
- They are represented by `add_on_catalog` + entitlement records, with domain tables enabled by module flags.


## Plan, Lifecycle, and Offboarding (Conceptual Modules)
- plans
- plan_features
- tenant_plan_assignments
- tenant_lifecycle_events
- tenant_exports
- offboarding_records
- export_jobs

## Support Access and Impersonation (Conceptual Modules)
- impersonation_logs
- support_access_sessions

## Reusable Workflow/Approval Engine (Conceptual Modules)
- workflow_definitions
- workflow_states
- workflow_instances
- workflow_actions
- workflow_approvals

## Notification Preference Model (Conceptual Modules)
- notification_preferences
- notification_escalation_rules (future)

## Optional Registry Submodules (Conceptual; entitlement-gated)
- parking_assignments
- vehicles
- pets
- emergency_contacts

## Property and Unit Status Model (Conceptual)
Recommended status values:
- owner_occupied
- rented
- vacant
- for_sale
- under_maintenance
- inactive

## Platform-Wide Export Strategy (Conceptual)
Exports should support CSV/PDF/printable outputs for:
- statements
- audit logs
- budgets
- meetings
- reports
- invoices


## Platform Capability Entities (Conceptual, Pre-SQL)

### Retention and Archival
- retention_policies
- archival_jobs
- archival_records
- purge_approvals

### Rate Limiting and Abuse Protection
- rate_limit_policies
- abuse_events
- throttle_events

### API and Integrations
- api_clients
- api_tokens
- webhook_endpoints
- webhook_deliveries
- integration_connectors
- integration_credentials (secured)
- integration_sync_runs

### Background Jobs
- job_queue
- job_runs
- job_schedules
- dead_letter_jobs

### File Storage and Search
- storage_quotas
- file_scan_events
- search_index_jobs

### Data Imports
- import_jobs
- import_files
- import_row_errors
- import_mappings

### Activity Feed and Analytics
- activity_events
- usage_metrics_daily
- module_adoption_metrics

### Disaster Recovery and Environment Operations
- backup_validation_runs
- recovery_drills
- environment_configs
- demo_tenant_templates
- demo_reset_jobs

### Tenant Configuration and Localization
- tenant_settings
- tenant_localization_settings

Notes:
- Keep these conceptual until schema phase.
- Reuse shared engines and avoid duplicating domain-specific variants.


## Architectural Control Entities (Conceptual, Pre-SQL)

### Data Classification
- data_classification_policies
- visibility_rules
- classification_audit_events

### Permission Inheritance
- role_hierarchy_rules
- permission_inheritance_rules
- permission_resolution_events

### Feature Flag Resolution
- feature_resolution_policies
- feature_resolution_events

### Module Dependencies
- module_dependencies
- module_dependency_exceptions

### Configuration Versioning
- configuration_snapshots
- configuration_versions
- configuration_rollbacks

### Upgrade and Rollout
- upgrade_runs
- migration_runs
- rollout_cohorts
- rollout_events

### UI Capability Registry
- ui_capability_registry
- route_permission_bindings
- menu_capability_bindings

### Data Consistency Controls
- idempotency_keys
- operation_locks
- retry_policies
- consistency_incidents


## Document Management Entities (Detailed, Conceptual)
Core entities:
- documents
- document_folders
- document_permissions
- document_versions
- document_publish_jobs (scheduled publish/unpublish queue binding)
- tenant_storage_usage

Document metadata contract (minimum):
- title
- description
- category
- tags_json
- created_at
- created_by
- file_size_bytes
- storage_path
- version_number
- visibility_scope
- publish_date (nullable)
- unpublish_date (nullable)
- publish_status

Permission action model (minimum):
- can_view
- can_download
- can_upload
- can_edit_metadata
- can_replace_file
- can_delete
- can_publish
- can_unpublish
- can_schedule_publish
- can_schedule_unpublish

Bulk action audit model:
- document_bulk_actions
- action_type
- actor_user_id
- selection_scope
- before_state
- after_state
- executed_at

## CSV / Excel Import Engine Entities (Detailed, Conceptual)
Core entities:
- import_jobs
- import_files
- import_mappings
- import_validation_results
- import_preview_rows
- import_execution_results
- import_row_errors
- import_templates

Import result fields:
- created_count
- updated_count
- skipped_count
- failed_count
- error_report_path

Template catalog examples:
- owners_import_template.csv
- units_import_template.csv
- vendors_import_template.csv
- financial_balances_template.csv


## Import Safety and Isolation Entities (Conceptual, Pre-SQL)

### import_batches
Suggested fields:
- id
- tenant_id
- import_type
- uploaded_by
- source_filename
- source_format
- status
- started_at
- completed_at
- total_rows
- created_rows
- updated_rows
- failed_rows

### import_row_results
Suggested fields:
- id
- import_batch_id
- row_number
- status
- message
- external_legacy_id
- created_entity_id
- error_details

### import_field_mappings
Suggested fields:
- id
- import_batch_id
- source_column_name
- target_field_name
- transform_rule (nullable)
- created_at

### import_validation_rules
Suggested fields:
- id
- import_type
- rule_key
- rule_description
- severity
- is_active

### Legacy reference fields guidance
For tenant-scoped entities, allow only reference fields for external systems:
- external_legacy_id
- source_system_id

Internal ID rule:
- internal primary IDs are generated by platform only.
- imported files cannot define internal IDs or tenant ownership.


## Core Baseline vs Optional Premium Variants
Canonical rule:
- Core entities define baseline platform capability.
- Optional add-on entities define premium/extended behavior behind entitlement and feature flags.

Examples:
- `votes` baseline governance records can exist in core.
- governance automation packages remain optional add-ons.
- reserve baseline fields can exist in core budgets; advanced reserve analytics remain optional add-ons.


## Analytics, Dashboard, Entitlements, Notification Entities (Conceptual, Pre-SQL)

### Dashboard and Widget Model
- dashboards
- dashboard_widgets
- widget_filters
- widget_queries

### Analytics Events and Rollups
- analytics_events
- analytics_daily_rollups
- analytics_monthly_rollups

Example analytics event types:
- user_login
- payment_posted
- work_order_created
- document_uploaded
- vote_cast
- reservation_created
- add_on_enabled
- tenant_created

### Entitlements and Feature Enablement
- feature_catalog
- plan_features
- tenant_features
- tenant_entitlements

### Notification Engine
- notification_templates
- notification_channels
- notification_queue
- notification_logs

Design notes:
- all analytics and dashboard data must remain tenant-safe
- entitlement state must control widget/module visibility
- queued notification processing should be reusable and auditable


## Operations and Leasing Pipeline Entities (Conceptual, Pre-SQL)

### Preventive maintenance and assets
- preventive_maintenance_plans
- preventive_maintenance_tasks
- asset_register
- asset_lifecycle_events
- asset_maintenance_links

### Leasing pipeline
- leasing_applications
- showing_schedules
- unit_marketing_profiles
- marketing_files

### Owner ledger and operational documents
- owner-facing ledger as conceptual reporting artifact (derived from charges/payments/allocations)
- operational_documents
- inspection_forms
- contractor_documents

### Inspection due analytics
- inspection due/overdue metrics as conceptual derived models/views from inspections and schedules

## Canonical KPI Naming Registry (Conceptual)
- kpi_revenue_overview
- kpi_tenant_health
- kpi_add_on_adoption
- kpi_storage_usage
- kpi_platform_usage
- kpi_system_activity
- kpi_security_logs
- kpi_export_center


## Workforce Module Entities (Conceptual, Pre-SQL)

Scope and identity:
- workforce_profiles
- workforce_role_assignments
- workforce_scope_assignments (platform or tenant scope)

Time and attendance:
- workforce_time_entries
- workforce_time_entry_batches
- workforce_attendance_summaries

Leave and requests:
- workforce_leave_requests
- workforce_leave_balances
- workforce_request_approvals

Holidays and legal notices:
- workforce_holidays
- workforce_legal_notices
- workforce_notice_acknowledgements

Pay visibility and admin:
- workforce_pay_statements (visibility artifact)
- workforce_payroll_exports
- workforce_payroll_import_links

Analytics and dashboards:
- workforce_dashboard_profiles
- workforce_analytics_rollups
- workforce_pending_request_metrics

Design notes:
- workforce module is entitlement-gated
- tenant workforce records are tenant-scoped
- platform workforce records use platform scope with strict access boundaries
- all approvals and sensitive actions must be auditable


## Workforce Payroll Rule and Paystub Entities (Conceptual, Pre-SQL)

### Configurable withholding rules
- workforce_withholding_rules
- workforce_withholding_rule_versions
- workforce_withholding_rule_overrides (optional tenant override)
- workforce_withholding_rule_scopes (employee/contractor class support later)

Suggested conceptual fields (withholding rules):
- id
- scope_type (platform/tenant)
- tenant_id (nullable for platform scope)
- rule_name
- withholding_percentage
- exempt_threshold_amount
- effective_date
- status (active/inactive)
- version_number
- created_by
- created_at
- updated_at

### Pay statement / paystub breakdown artifacts
- workforce_pay_statement_lines
- workforce_pay_statement_summaries

Suggested conceptual fields (statement summary):
- gross_pay
- taxable_or_eligible_amount
- exempt_threshold_amount
- withholding_amount
- other_deductions
- total_deductions
- net_pay
- ytd_gross
- ytd_net
- ytd_withholding

Audit/version requirements:
- payroll rule changes must emit audit events
- rule versioning and effective-date history are mandatory


## Baseline Employee Identity Model (Conceptual, Pre-SQL)

Canonical workforce employee entity for data-model stage:

### employee_profiles
Identity:
- employee_id
- tenant_id or platform_org_id
- employee_number
- first_name
- last_name
- middle_name
- display_name

Contact:
- email
- phone_number
- address (optional)

Employment:
- hire_date
- employment_status
- employment_type
- department
- job_title
- supervisor_id

Access / Security:
- user_account_id
- invited_by
- invitation_status
- MFA_enabled
- last_login

Payroll / Compensation:
- pay_type
- pay_rate
- pay_currency
- withholding_rule_id
- tax_classification (future)

Workforce Settings:
- time_tracking_enabled
- leave_tracking_enabled
- payroll_visibility_enabled

System:
- created_at
- updated_at
- archived_at

Notes:
- This is a conceptual model direction only (no SQL yet).
- `tenant_id` applies for tenant workforce scope; `platform_org_id` applies for platform workforce scope.
- Internal IDs are platform-generated and access remains scope-safe.

## Related Employee Entities (Conceptual, Pre-SQL)
- employee_time_entries
- employee_leave_requests
- employee_pay_statements
- payroll_withholding_rules

Suggested alignment note:
- Existing `workforce_*` entities may be treated as naming variants/aliases while canonical naming is finalized.



## Professional Services Billing / Time / Invoice (Conceptual, Pre-SQL)

### Business problem it solves
Support historically correct professional-services billing where approved time, fixed-fee tasks, billable expenses, withholding, invoices, and partial payments must remain stable even when rates or tax-retention rules change later.

### Main entities / tables
- `clients`
- `client_contacts`
- `client_billing_profiles`
- `service_types`
- `projects`
- `rate_schedules`
- `rate_schedule_versions`
- `time_entries`
- `time_entry_approvals`
- `billable_expenses`
- `expense_receipts`
- `invoice_numbering_profiles`
- `invoices`
- `invoice_lines`
- `invoice_templates`
- `invoice_line_rate_snapshots`
- `withholding_rules`
- `withholding_rule_versions`
- `client_withholding_assignments`
- `invoice_line_withholding_snapshots`
- `payments_received`
- `payment_allocations`
- `invoice_status_history`

### Core conceptual rules
- one tenant/business context may have many clients
- approved time entries can be converted into invoices
- invoices may include hourly lines, fixed-fee task lines, expense lines, or mixed combinations
- rates are effective-dated and historical-safe
- one invoice may contain lines with different rates when rate changes occur during the billing period
- invoice lines preserve a rate snapshot and never recalculate from the current rate
- withholding rules are effective-dated and versioned
- invoice lines preserve a withholding snapshot and never recalculate from the current rule version
- historical invoices must remain unchanged after future rate/rule changes
- payments must support full, partial, and multi-payment allocation
- no SQL generation in this phase

### Suggested conceptual fields

#### clients
- id
- tenant_id
- client_name
- client_status
- billing_contact_name
- billing_contact_email
- billing_contact_phone
- billing_address
- payment_terms
- billing_settings_json
- active_rate_plan_id (nullable)
- active_withholding_assignment_id (nullable)
- created_at
- updated_at

#### service_types
- id
- tenant_id
- service_name
- billing_mode (hourly/fixed/mixed)
- status
- created_at
- updated_at

#### rate_schedules
- id
- tenant_id
- client_id (nullable)
- service_type_id (nullable)
- project_id (nullable)
- schedule_name
- status
- created_at
- updated_at

#### rate_schedule_versions
- id
- tenant_id
- rate_schedule_id
- effective_date
- rate_type
- hourly_rate (nullable)
- fixed_fee_amount (nullable)
- status
- created_at
- updated_at

#### time_entries
- id
- tenant_id
- client_id
- project_id (nullable)
- service_type_id
- work_date
- task_description
- is_billable
- hours
- approval_status
- created_by
- created_at
- updated_at

#### billable_expenses
- id
- tenant_id
- client_id
- project_id (nullable)
- linked_invoice_id (nullable)
- expense_date
- vendor_name
- amount
- expense_type
- is_billable
- is_reimbursable
- approval_status
- notes
- created_at
- updated_at

#### invoices
- id
- tenant_id
- client_id
- invoice_number
- invoice_date
- due_date
- invoice_format
- gross_amount
- exempt_threshold_amount
- withheld_amount
- total_deductions
- paid_amount
- balance_due
- status
- created_by
- created_at
- updated_at

#### invoice_lines
- id
- tenant_id
- invoice_id
- line_type
- source_type
- source_id (nullable)
- service_date (nullable)
- description
- quantity
- unit_rate
- line_amount
- created_at
- updated_at

#### invoice_line_rate_snapshots
- id
- tenant_id
- invoice_line_id
- source_rate_schedule_version_id (nullable)
- rate_type
- hourly_rate (nullable)
- fixed_fee_amount (nullable)
- effective_date_snapshot
- captured_at

#### withholding_rules
- id
- tenant_id
- rule_name
- status
- created_at
- updated_at

#### withholding_rule_versions
- id
- tenant_id
- withholding_rule_id
- effective_date
- withholding_percentage
- exempt_threshold_amount
- version_number
- status
- created_at
- updated_at

#### client_withholding_assignments
- id
- tenant_id
- client_id
- withholding_rule_id
- assigned_at
- active_from
- active_to (nullable)
- status
- created_at
- updated_at

#### invoice_line_withholding_snapshots
- id
- tenant_id
- invoice_line_id
- source_withholding_rule_version_id (nullable)
- withholding_percentage
- exempt_threshold_amount
- withheld_amount
- captured_at

#### payments_received
- id
- tenant_id
- client_id
- invoice_id (nullable)
- payment_date
- amount
- payment_method
- reference_number
- status
- unapplied_amount
- notes
- created_at
- updated_at

#### payment_allocations
- id
- tenant_id
- payment_received_id
- invoice_id
- allocated_amount
- allocated_at
- created_at
- updated_at

#### invoice_numbering_profiles
- id
- tenant_id
- prefix
- next_number
- numbering_mode
- status
- created_at
- updated_at

### Invoice and payment behavior notes
- automatic numbering must support prefix + next number
- optional manual numbering mode may exist
- uniqueness validation is mandatory
- numbering preference changes must be auditable
- invoice status examples: draft, sent, partially_paid, paid, overdue, canceled, void

### How it connects to the rest of the system
- Reuses workflow approvals for time entries, expenses, and invoices where policy requires
- Reuses document engine for expense receipts and invoice PDFs
- Reuses export/PDF engine for printable invoices
- Reuses analytics/dashboard layer for invoice and payment reporting
- Reuses audit engine for rate changes, withholding changes, numbering preference changes, invoice events, and payment allocations


## Dashboard / Widget Configuration Engine (Conceptual, Pre-SQL)

### Business problem it solves
Provide a reusable, historically stable dashboard configuration model so analytics and widgets can be defined once and reused across platform, tenant, role, and module contexts without hardcoded page-specific implementations.

### Main entities / tables
- `dashboards`
- `dashboard_profiles`
- `dashboard_widgets`
- `dashboard_widget_settings`
- `dashboard_widget_filters`
- `dashboard_widget_series`
- `dashboard_widget_permissions`
- `dashboard_layouts`
- `dashboard_saved_views` (future)

### Core conceptual rules
- dashboard definitions are metadata/configuration driven
- dashboard scope may be platform, tenant, module, role, or future user-custom
- widgets are reusable definitions, not isolated per-page implementations
- widget visibility must remain permission-aware and role-aware
- filter definitions should be reusable across modules where possible
- series configuration must support multi-line and multi-series interactive charts
- no SQL generation in this phase

### Suggested conceptual fields

#### dashboards
- id
- tenant_id (nullable for platform/global scope)
- dashboard_code
- dashboard_name
- scope_type
- module_key (nullable)
- role_key (nullable)
- status
- created_by
- created_at
- updated_at

#### dashboard_profiles
- id
- tenant_id (nullable)
- dashboard_id
- profile_name
- default_for_scope
- status
- created_at
- updated_at

#### dashboard_widgets
- id
- tenant_id (nullable)
- dashboard_id
- widget_code
- title
- module_key
- data_source_key
- chart_type
- is_enabled
- default_visibility
- layout_position
- size_key
- refresh_interval_seconds (nullable)
- allow_export
- allow_fullscreen
- allow_chart_switch
- allow_filtering
- allow_series_toggle
- created_at
- updated_at

#### dashboard_widget_settings
- id
- tenant_id (nullable)
- dashboard_widget_id
- setting_key
- setting_value_json
- created_at
- updated_at

#### dashboard_widget_filters
- id
- tenant_id (nullable)
- dashboard_widget_id
- filter_key
- filter_label
- filter_type
- data_source_key (nullable)
- is_required
- default_value_json (nullable)
- created_at
- updated_at

#### dashboard_widget_series
- id
- tenant_id (nullable)
- dashboard_widget_id
- series_key
- series_label
- aggregation_type
- axis_group
- color_token (nullable)
- default_visible
- is_enabled
- created_at
- updated_at

#### dashboard_widget_permissions
- id
- tenant_id (nullable)
- dashboard_widget_id
- role_id (nullable)
- permission_key
- visibility_mode
- created_at
- updated_at

#### dashboard_layouts
- id
- tenant_id (nullable)
- dashboard_id
- layout_name
- breakpoint_key
- layout_json
- created_at
- updated_at

#### dashboard_saved_views (future)
- id
- tenant_id
- dashboard_id
- user_id
- view_name
- filter_state_json
- layout_state_json
- created_at
- updated_at

### How it connects to the rest of the system
- reuses analytics rollups and widget query providers
- reuses permission engine for visibility and action gating
- reuses export engine for widget export and print/PDF outputs
- reuses audit engine for dashboard configuration changes
- serves platform, tenant, module, and role dashboards from one reusable engine



## Historical Identity and Naming Continuity (Conceptual, Pre-SQL)

### Business problem it solves
Preserve organizational, ownership, membership, naming, and administrative history so the platform can represent continuity over time without replacing the underlying master entities.

### Stable master entities
- `users`
- `user_profiles`
- `tenant_organizations`
- `properties`
- `buildings`
- `units`

### Historical relationship entities
- `tenant_memberships`
- `tenant_membership_history`
- `ownership_records`
- `occupancy_records`
- `lease_records`
- `agent_assignments`
- `administrator_assignments`
- `board_member_terms`

### Name / alias / path history entities
- `tenant_name_history`
- `property_name_history`
- `building_name_history`
- `tenant_domain_history`
- `tenant_path_history`

### Core conceptual rules
- stable master entities must remain stable even when names, personnel, or relationships change
- historical relationship changes must create new dated records rather than overwriting prior records
- current state should be derived from active/effective records
- expired/superseded records must remain queryable for audit, reporting, and operational review
- a tenant name change or public path/subdomain change must not create a new tenant identity
- an owner/resident/lease/agent/administrator change must not destroy prior history
- no SQL generation in this phase

### Suggested conceptual fields

#### user_profiles
- id
- user_id
- display_name
- legal_name (nullable)
- status
- created_at
- updated_at

#### tenant_organizations
- id
- tenant_id
- legal_name
- display_name
- status
- created_at
- updated_at

#### tenant_membership_history
- id
- tenant_membership_id
- tenant_id
- user_id
- role_snapshot
- status_snapshot
- effective_start
- effective_end (nullable)
- changed_by
- created_at

#### ownership_records
- id
- tenant_id
- property_id
- unit_id (nullable)
- owner_user_id
- effective_start
- effective_end (nullable)
- status
- created_at
- updated_at

#### occupancy_records
- id
- tenant_id
- property_id
- unit_id
- occupant_user_id
- occupancy_type
- effective_start
- effective_end (nullable)
- status
- created_at
- updated_at

#### lease_records
- id
- tenant_id
- unit_id
- primary_party_user_id
- lease_start
- lease_end
- status
- created_at
- updated_at

#### agent_assignments
- id
- tenant_id
- property_id (nullable)
- unit_id (nullable)
- assigned_user_id
- assignment_type
- effective_start
- effective_end (nullable)
- status
- created_at
- updated_at

#### administrator_assignments
- id
- tenant_id
- assigned_user_id
- admin_role
- effective_start
- effective_end (nullable)
- is_current
- status
- created_at
- updated_at

#### board_member_terms
- id
- tenant_id
- board_id
- user_id
- position
- term_start
- term_end (nullable)
- is_current
- status
- created_at
- updated_at

#### tenant_name_history
- id
- tenant_id
- previous_name
- current_name
- effective_date
- is_current
- created_at

#### property_name_history
- id
- property_id
- previous_name
- current_name
- effective_date
- is_current
- created_at

#### building_name_history
- id
- building_id
- previous_name
- current_name
- effective_date
- is_current
- created_at

#### tenant_domain_history
- id
- tenant_id
- previous_domain
- current_domain
- effective_date
- is_current
- created_at

#### tenant_path_history
- id
- tenant_id
- previous_path
- current_path
- effective_date
- is_current
- created_at

### Historical review note
Administrative, ownership, occupancy, lease, and naming tables must support historical review and must avoid destructive replacement of prior records.


## Canonical Entity Naming Alignment
Use these canonical entity concepts consistently across documentation:
- Users
- Tenant Organizations
- Properties
- Buildings
- Units
- Lease Records
- Ownership Records
- Occupancy Records
- Documents
- Maintenance Requests
- Inspections
- Invoices
- Payments
- Expenses
- Employees
- Timesheets
- Reservations

Alignment notes:
- legacy or transitional entities such as `unit_relationships` and `leases` may remain documented for compatibility, but canonical future-facing terminology should prefer `ownership_records`, `occupancy_records`, and `lease_records` where historical continuity matters
- canonical prose should distinguish stable entities from historical relationship records
- entity naming in architecture and menu docs should align to this model


### Dashboard entity normalization
Canonical dashboard configuration entities are:
- `dashboards`
- `dashboard_profiles`
- `dashboard_widgets`
- `dashboard_widget_settings`
- `dashboard_widget_filters`
- `dashboard_widget_series`
- `dashboard_widget_permissions`
- `dashboard_layouts`
- `dashboard_saved_views` (future)

Older shorthand references should be treated as incomplete aliases rather than the canonical list.


## Module Registry and Shared Service Support (Conceptual, Pre-SQL)

### Conceptual entities
- `module_registry`
- `module_routes`
- `module_menus`
- `module_permissions`
- `module_entities`
- `module_widget_registry`
- `module_notification_bindings`
- `module_job_bindings`
- `module_dependencies`
- `module_lifecycle_events`

### Rule
These are conceptual support models for reusable module registration and lifecycle governance only. No SQL generation in this phase.

## Platform Event Model (Conceptual, Pre-SQL)

### Conceptual entities
- `platform_events`
- `domain_events`
- `event_subscriptions`
- `event_delivery_attempts`

### Example event keys
- `user_invited`
- `membership_changed`
- `payment_posted`
- `invoice_generated`
- `document_published`
- `maintenance_request_created`
- `ownership_transferred`
- `add_on_enabled`

### Rule
Events should support analytics, notifications, monitoring, and workflow automation without introducing direct tight coupling between modules.

## Monitoring, Health, and Async Operations (Conceptual, Pre-SQL)

### Conceptual entities
- `application_health_checks`
- `tenant_health_checks`
- `job_failures`
- `notification_failures`
- `integration_failures`
- `quota_alerts`
- `security_alerts`
- `dead_letter_jobs`
- `job_retry_policies`

### Rule
Operational observability artifacts should be tenant-safe where applicable and auditable where operationally sensitive.

## Search and Indexing Support (Conceptual, Pre-SQL)

### Conceptual entities
- `search_index_definitions`
- `search_index_runs`
- `search_documents`
- `search_visibility_rules`

### Rule
Search models must preserve tenant scope and permission-aware visibility.

## Archive / Soft Delete / Retention Handling (Conceptual, Pre-SQL)

### Conceptual entities
- `archive_records`
- `soft_delete_records`
- `restore_requests`
- `restore_approvals`

### Rule
Immutable historical records should not be modeled as soft-deletable transactional rows where audit or legal continuity must be preserved.

## Status and Transition Models (Conceptual, Pre-SQL)

### Conceptual entities
- `status_definitions`
- `status_transitions`
- `entity_status_history`

### Intended workflow domains
- invoices
- documents
- maintenance requests
- invitations
- reservations

## Master Data vs Transactional Data Note
Conceptual modeling should separate:
- master/reference data entities
- transactional/event entities

This distinction should influence:
- indexing strategy
- archival policy
- reporting rollups
- write/read model design
