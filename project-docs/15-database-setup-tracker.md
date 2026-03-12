# Database Setup Tracker

## Foundation Schema Status
| Table | Status | Migration | Notes |
|---|---|---|---|
| users | Planned | | Global identity |
| tenants | Planned | | |
| tenant_memberships | Planned | | Org membership roles |
| properties | Planned | | |
| buildings | Planned | | |
| units | Planned | | |
| unit_relationships | Planned | | Owner/resident/renter/landlord/co-owner |
| leases | Planned | | Time-based occupancy contract |
| contract_assignments | Planned | | Leasing/realtor/vendor/contractor scope |
| announcements | Planned | | |
| maintenance_requests | Planned | | |
| charges | Planned | | |
| payments | Planned | | Manual-first |
| payment_allocations | Planned | | |
| audit_logs | Planned | | Tenant/global event log |

## Security Checklist
- [ ] RLS enabled on all tenant-scoped tables
- [ ] Membership check policy implemented
- [ ] Relationship and contract assignment policy checks implemented
- [ ] Write policies constrained by role, tenant, active dates, and scope
- [ ] Audit logging trigger or service in place

## Migration Conventions
- One migration per logical change set.
- Name migrations with timestamp and concise purpose.
- Update this tracker whenever a migration is added or applied.

## v5 Schema Expansion Status
| Table | Status | Migration | Notes |
|---|---|---|---|
| vendor_contracts | Planned | | Service contracts + expiry tracking |
| vendor_ratings | Planned | | Performance and quality tracking |
| capital_projects | Planned | | Registry and project lifecycle |
| project_bids | Planned | | Quote/bid workflow |
| project_milestones | Planned | | Progress checkpoints |
| project_media | Planned | | Progress photos |
| reserve_assets | Planned | | Asset lifecycle basis |
| reserve_forecasts | Planned | | Replacement/funding projections |
| reserve_funding_targets | Planned | | Funding strategy |
| reserve_analyses | Planned | | Reserve analysis snapshots |
| inspections | Planned | | Inspection schedule and status |
| inspection_reports | Planned | | Inspection outcomes |
| forms | Planned | | Digital form templates |
| form_submissions | Planned | | Submitted requests |
| packages | Planned | | Package registry |
| package_events | Planned | | Package status lifecycle |
| notifications | Planned | | In-app/email notifications |
| notification_templates | Planned | | Message templates |
| notification_events | Planned | | Trigger/event records |
| media_assets | Planned | | Media gallery items |

## v5 Audit Checkpoints
- [ ] Field-level audit diffs enabled for sensitive entities.
- [ ] Audit filters support date/date-range/user/entity/module/action.
- [ ] CSV/PDF/print report outputs defined for audit views.


## v6 Access-Control Schema Checkpoints
| Table | Status | Migration | Notes |
|---|---|---|---|
| roles | Planned | | Tenant role catalog + templates |
| permissions | Planned | | Module/action permission keys |
| role_permissions | Planned | | Role-to-permission matrix |
| user_role_overrides | Planned | | Per-user allow/deny overrides |
| user_invitations | Planned | | Invite / disable / reactivate flow |
| approval_workflow_rules | Planned | | Tenant approval policy definitions |

## v6 Access Audit Checkpoints
- [ ] Permission changes audited with old/new values.
- [ ] Role assignment changes audited.
- [ ] Override changes audited.
- [ ] Invitation state transitions audited.


## Platform Operations Schema Checkpoints
Planned checkpoints:
- session policies
- forced logout events
- maintenance controls and notices
- tenant feature flags
- monitoring/health events
- backup jobs and restore jobs
- incident reports/support tickets
- future messaging placeholders


## Monetization and Listings Schema Checkpoints
Planned checkpoints:
- add_on_catalog
- add_on_bundles
- tenant_entitlements
- tenant_subscriptions
- platform_billing_rules
- tenant_billing_invoices
- payment_reminders
- suspension_events
- listings
- listing_publication_scopes


## New Conceptual Schema Checkpoints
- plans / plan_features / tenant_plan_assignments
- tenant_lifecycle_events
- tenant_exports / offboarding_records / export_jobs
- impersonation_logs / support_access_sessions
- workflow_definitions / workflow_instances / workflow_approvals
- notification_preferences
- parking_assignments / vehicles / pets / emergency_contacts
- property_statuses / unit_statuses normalization


## Pre-Schema Capability Checkpoints
- retention_policies / archival_jobs
- rate_limit_policies / abuse_events
- api_clients / webhook_endpoints / webhook_deliveries
- integration_connectors / integration_sync_runs
- job_queue / job_runs / job_schedules
- storage_quotas / file_scan_events / search_index_jobs
- import_jobs / import_files / import_row_errors
- activity_events / usage_metrics_daily
- backup_validation_runs / recovery_drills
- environment_configs / demo_tenant_templates / demo_reset_jobs
- tenant_settings / tenant_localization_settings


## DMS and Import Detailed Checkpoints
- document_publish_jobs (scheduled publish/unpublish)
- tenant_storage_usage aggregation
- document_bulk_actions audit model
- import_validation_results / import_preview_rows
- import_execution_results summary counts
- import_templates registry
- import error report artifact storage


## Import Isolation Schema Checkpoints
- import_batches
- import_row_results
- import_field_mappings
- import_validation_rules
- legacy reference field policy (`external_legacy_id`, `source_system_id`)
- tenant-ownership enforcement checks for imports

