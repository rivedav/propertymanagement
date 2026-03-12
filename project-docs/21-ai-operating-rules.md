# AI Operating Rules

## Purpose
Define hard operating rules for all AI agents working in this repository.

## Core Authority Rules
- `project-docs/*` is the project source of truth.
- Do not invent business rules.
- Do not invent roles, permissions, module scope, or approval behavior.
- Do not treat top-level reports or conversations as authoritative over `project-docs/*`.

## Documentation Gates
- Never change schema direction without updating `project-docs/05-data-model.md`.
- Never change roles or permissions without updating `project-docs/02-roles-and-access.md`.
- Never change access/menu behavior without updating `project-docs/14-admin-access-menu-matrix.md`.
- Never change architecture direction without updating `project-docs/04-architecture.md`.
- Never expand MVP or add-on scope without updating `project-docs/01-mvp-scope.md`.
- Never introduce new documentation topics without aligning `project-docs/19-doc-governance-and-boundaries.md`.

## Architecture Rules
- Prefer modular, reusable, scalable architecture.
- Do not add isolated one-off code when reusable engines already exist or are documented.
- Prefer configuration-driven or UI-driven behavior where feasible.
- Reuse workflow, permissions, audit, export, analytics, and document engines before introducing new isolated subsystems.
- Preserve tenant-safe and shared-database-safe design.

## Consistency Rules
- Preserve terminology consistency across docs and code discussions.
- Use the canonical terms already established in `project-docs/*`.
- If terminology drifts, align docs before implementation proceeds.
- Keep module classification consistent: Core, Optional Add-On, Design-Now / Implement-Later, Future.

## Governance Rules
- Do not bypass governance docs.
- Do not implement sensitive features before documentation approval when a gate already exists.
- High-risk changes require explicit review of architecture, roles/access, and data model docs first.

## Security Rules
- Default to least privilege.
- Preserve tenant isolation in every proposal.
- Treat authentication, session policy, RLS, approval flows, and auditability as design-time concerns, not cleanup tasks.
- Do not weaken audit coverage for privileged actions.

## Working Rules for AI Agents
- Read the mandatory docs before coding.
- State assumptions when docs are incomplete.
- Prefer updating docs first when design decisions are changing.
- Flag conflicts instead of silently choosing one interpretation.
- Do not present guesses as decisions.
- Do not generate SQL or schema migrations until documentation direction is approved.

## Change Discipline
- Small, targeted documentation updates are preferred over broad rewrites.
- Cross-reference owner files instead of duplicating full rules.
- Keep design guidance synchronized across scope, architecture, roles, and data model documents.
