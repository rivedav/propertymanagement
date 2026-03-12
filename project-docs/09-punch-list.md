# Punch List

## Candidate Features and Improvements
- [ ] Tenant onboarding wizard
- [ ] Lease expiry reminder workflow
- [ ] Bulk unit import via CSV
- [ ] Maintenance SLA dashboard
- [ ] Resident document attachments
- [ ] Owner reporting views
- [ ] Payment allocation support (optional)
- [ ] Export audit logs by tenant/date

## Intake Rule
Add ideas here first. Do not implement until scoped and prioritized in sprint docs.

## v5 Follow-Up Delivery Items
- [ ] Add schema specs for vendor, projects, reserve, inspections, forms, packages, notifications modules.
- [ ] Define dashboard widget impacts for reserve, projects, inspections, packages.
- [ ] Validate role/access matrix coverage for new modules.
- [ ] Expand audit trail event map for all new entities.
- [ ] Define report/export requirements (CSV/PDF/print) per module.


## Platform Operations Follow-Up
- define shared-database backup strategy and retention policy
- define tenant-scoped restore strategy and guardrails
- define platform/tenant monitoring dashboard KPIs
- define issue reporting workflow and SLA states
- define feature-flag model and tenant rollout operations
- define future internal messaging boundaries and dependency map


## Monetization and Add-On Follow-Up
- define feature-flag architecture with module registry
- define add-on catalog and bundle model
- define subscription lifecycle and billing triggers
- define suspension/reactivation automation rules
- define preserved-data behavior after module disable
- define listings marketplace publication scopes and pricing options


## Follow-Up: Plans, Lifecycle, and Operations
- define plan/package matrix and limits
- define add-on commercial rule matrix
- define lifecycle transition rules and automation
- define export/offboarding runbook
- define impersonation guardrails and audit policy
- define unified workflow engine contracts
- define notification preference schema and fallback behavior
- define property/unit status normalization


## Follow-Up: Platform Architecture Capabilities
- define retention matrix by data domain
- define rate limiting strategy by endpoint type
- define API boundary and webhook contract model
- define connector framework and secrets policy
- define queue/job orchestration baseline
- define quota/scanning/storage controls
- define search indexing and query scope
- define import templates and validation contracts
- define activity feed event taxonomy
- define usage analytics KPIs
- define RTO/RPO and failover expectations
- define environment and demo reset governance
- define tenant settings and localization defaults


## Follow-Up: DMS and Import Engine Hardening
- finalize document metadata contract
- finalize document action permission matrix
- define bulk action authorization and auditing
- define publish/unpublish scheduling jobs
- define tenant storage usage/quota enforcement
- define import workflow UI contract
- define import validation rule catalog
- define import templates catalog and versioning
- define import result/error report format


## Follow-Up: Import Safety Hardening
- document strict rule: file cannot set tenant_id
- document strict rule: internal IDs are platform-generated only
- define global-entity allowlist for non-tenant-scoped matching
- add possible-duplicate/review-needed decision workflow
- add explicit import batch audit event schema


## Follow-Up: Analytics, Dashboard, Entitlements, Notifications
- define canonical dashboard widget catalog
- define analytics event taxonomy and retention
- define rollup job cadence (daily/monthly)
- define widget filter contract and query safety
- define entitlement impact matrix on dashboards/widgets
- define notification channel + queue + delivery log contracts


## Follow-Up: Workforce Module Family
- define workforce role-permission matrix
- define workforce dashboard widget catalog
- define attendance and leave workflow rules
- define legal notice acknowledgement flow
- define payroll visibility/export boundaries
- define entitlement and rollout controls for workforce module



## Follow-Up: Professional Services Billing / Time / Invoice
- define client billing profile contract and client-specific overrides
- define effective-dated rate schedule rules and version transitions
- define fixed-fee vs hourly line-generation behavior
- define withholding rule versioning and client assignment behavior
- define invoice numbering preference model and audit events
- define invoice PDF formats: detailed, summary, grouped
- define payment allocation rules for partial and multi-payment flows
- define invoice/payment table filters and export behavior
- define status-transition rules for draft, sent, partially_paid, paid, overdue, canceled, and void


## Follow-Up: Dashboard / Widget Configuration Engine
- define canonical dashboard scope model: platform, tenant, module, role, future user
- define widget metadata contract and allowed chart types
- define widget layout/breakpoint model
- define reusable filter catalog and filter-source contracts
- define series configuration rules for multi-line/multi-series charts
- define permission and entitlement impact on widget visibility
- define dashboard configuration audit events
- define future saved-view behavior without breaking tenant-safe defaults


## Follow-Up: Historical Identity and Continuity
- define canonical stable-entity rules for tenant, property, building, unit, and user identity
- define effective-date rules for administrator, board-member, owner, resident, renter, and agent changes
- define current-vs-historical projection rules
- define tenant name/domain/path history behavior without changing tenant identity
- define ownership/occupancy/lease transition rules and overlap constraints
- define admin UI filters and table behavior for historical review
- define audit events for identity-preserving relationship changes
