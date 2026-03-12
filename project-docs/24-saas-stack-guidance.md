# SaaS Stack Guidance

## Purpose
Define the intended SaaS stack and technical direction for implementation planning and schema-aligned architecture work.

## Core Stack Direction
- Next.js App Router is the frontend/application baseline
- React is the component and UI composition baseline
- Supabase Auth is the authentication direction
- Supabase Postgres with RLS is the data-access isolation direction
- shared-database multitenancy remains the working model unless explicitly changed in approved docs

## Frontend Direction
- use React component composition with reusable UI primitives
- prefer configuration-driven screens and reusable module shells
- keep dashboards, widgets, forms, tables, and approval surfaces reusable across modules
- avoid one-off page-specific implementations when a reusable engine is appropriate

## Auth and Data Direction
- use Supabase Auth for identity and approved invite flows
- use RLS plus app-layer authorization
- keep tenant context explicit in application behavior
- align schema direction with documented access and ownership rules

## Service Boundary Direction
- prefer modular service boundaries by domain
- centralize reusable engines for permissions, workflow, audit, documents, notifications, exports, analytics, and dashboards
- avoid isolated mini-systems when a shared engine should own the capability

## Background Jobs and Tasks
- background jobs/tasks are an approved architectural direction
- scheduled actions, exports, notifications, and heavy processing should align with documented async/job architecture
- job design must remain auditable and tenant-safe

## API and Integration Layer
- API and integration behavior should use documented service boundaries
- webhook/integration behavior must remain rate-limited, auditable, and tenant-safe
- external integrations should be introduced through reusable integration patterns, not ad hoc module code

## Feature Flags and Entitlements
- feature flags and entitlements are part of the intended SaaS stack
- optional modules must remain compatible with packaging, activation, deactivation, and preserved-data rules
- module availability should be configuration-driven where possible

## Dashboard / Widget Direction
- reusable dashboard/widget architecture is part of the stack direction
- dashboards should be metadata-driven rather than hardcoded
- widget rendering, filtering, permissions, export behavior, and layout should remain reusable across module families

## Engineering Preference
- prefer scalable and maintainable patterns over fast one-off delivery
- prefer documentation-aligned architecture before schema design begins
- keep stack decisions consistent with `project-docs/04-architecture.md` and `project-docs/05-data-model.md`
