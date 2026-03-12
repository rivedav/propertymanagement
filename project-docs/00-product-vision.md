# Product Vision

## Objective
Build a multi-tenant property management platform where one platform can serve many tenant organizations, each with isolated data and role-based operations.

## Core Goals
- Deliver a secure MVP with strong tenant isolation and predictable behavior.
- Keep architecture simple: Next.js + Supabase + Vercel.
- Prioritize operational workflows before advanced integrations.
- Use repository documentation as the single source of truth for AI and humans.

## Primary Users
- Platform admin
- Tenant admin
- Property and operations staff
- Residents
- Owners

## Value Proposition
- Tenant organizations can manage properties, units, residents, leases, maintenance, and finance basics in one system.
- Platform admin can onboard and manage tenants with audit visibility and governance controls.

## Non-Goals (Current Stage)
- Stripe and automated payment rails
- Subdomain tenancy
- Dockerized environments
- n8n and worker automation stack
- Embedded AI features in product UX

## SaaS Operations and Control
The platform includes operational controls for secure SaaS administration across tenants:
- platform and tenant maintenance/interruption controls
- forced logout and configurable session timeout policies
- tenant/global operational monitoring
- backup/restore planning for shared-database architecture
- modular feature enablement (per tenant) for staged rollout and packaging


## Monetization and Add-On Vision
The platform includes a monetization layer based on reusable modules and tenant-scoped entitlements.

Principles:
- add-ons are module-driven and permission-driven
- activation/deactivation is configuration-driven
- architecture remains scalable and maintainable
- low-code/UI-driven administration is preferred where feasible


## Plans, Lifecycle, and Modular SaaS Direction
The platform uses a modular SaaS model with:
- base plans/packages (Basic, Standard, Premium, Enterprise)
- optional add-ons with configurable commercial rules
- tenant lifecycle governance and operational controls
- reusable engine architecture for scalable feature growth

Commercial rule options (per module/add-on):
- free
- one-time purchase
- recurring subscription
- fixed fee
- percentage fee
- bundle/bulk offer
- included by plan
- tenant-specific override


## Platform Reliability and Operability Goals
The platform architecture includes:
- retention and archival policy governance
- abuse protection and rate limiting
- API and integration readiness
- async job processing
- disaster recovery targets
- multi-environment delivery discipline
- observability and usage analytics


## Analytics and Dashboard Goal
The platform must provide tenant-safe analytics, flexible dashboards, entitlement-aware module visibility, and reusable notification/reminder architecture using modular shared engines.


## Workforce Module Family Vision
The platform supports an optional Employee Portal / Workforce module family with two scopes:
- Platform Workforce (SaaS company employees)
- Tenant Workforce (tenant organization employees)

This module family must follow existing platform standards:
- invitation-only onboarding
- MFA/security controls
- tenant-safe access
- reusable modular architecture
- workflow/approval support
- dashboard/analytics support



## Professional Services Billing / Time / Invoice Module Family Vision
The platform supports an optional Professional Services Billing / Time / Invoice module family for tenants or business contexts that bill external clients for services, time, tasks, and reimbursable expenses.

This module family is distinct from generic invoicing screens and must be designed as a reusable, monetizable module family.

Core business coverage:
- clients and client billing profiles
- approved time entry to invoice flow
- hourly and fixed-fee billing
- billable and reimbursable expenses
- effective-dated rate management with historical safety
- configurable withholding / tax retention
- automatic invoice numbering
- payment receipt and partial allocation tracking
- PDF invoice generation and printable invoice formats
- invoice and payment reporting tables

Boundary clarification:
- Workforce / Employee Portal focuses on employee self-service, time logging, profile, leave, and paystub visibility.
- Professional Services Billing / Time / Invoice focuses on clients, pricing, billable time, expenses, withholding, invoice generation, and receivables tracking.
