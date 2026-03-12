# Multitenancy Rules

## Two-Dimensional Multi-Tenancy
This platform supports both:
1. SaaS multi-tenancy: one platform serves many tenant organizations.
2. Cross-tenant user participation: one global user can participate in multiple tenants.

## Core Isolation Rules
- All business tables must include `tenant_id` unless truly global.
- Every business query must be tenant-scoped.
- RLS enforces tenant boundaries in the database.
- Backend never trusts client-provided `tenant_id` without server-side validation.

## Access Sources
Access may come from one or more active sources:
1. tenant membership (`tenant_memberships`)
2. active unit relationship (`unit_relationships`, `leases`)
3. active contract assignment (`contract_assignments`)

## Contextual Access Resolution
Access is determined by:
- selected tenant
- selected dashboard context
- active dates
- relationship type
- contract assignment type
- permission scope

## Audit Trail Requirements (Cross-Tenant Safety)
Business audit trail must be tenant-aware and capture:
- tenant_id
- actor_user_id
- action_type
- entity_type
- entity_id
- field_name (when field-level change exists)
- old_value
- new_value
- performed_at
- ip_address (optional)
- user_agent (optional)
- reason_or_note (optional)

Required auditable domains:
- role and permission changes
- lease lifecycle changes
- ownership/relationship changes
- reservations and approvals
- budgets and budget line changes
- board records, meetings, minutes, votes
- announcements publication changes
- maintenance status and assignment changes
- financial receivables and payables lifecycle changes

## Business Audit Trail vs Technical/System Logs
- Business audit trail: who changed business data and authorization-relevant state.
- Technical/system logs: infrastructure/runtime telemetry (errors, performance, system events).
- Both are required; technical logs do not replace business audit trail.

## Security Guarantees
- No cross-tenant read leakage.
- No cross-tenant write leakage.
- No cross-tenant aggregate leakage in reporting.

## Examples
- User is owner in Condo A and renter in Building D.
- User owns units across multiple tenants.
- User acts as leasing agent for Condo Z without standard tenant admin membership.
