# Document Governance and Boundaries

## Purpose
Prevent overlap across project docs by assigning one source-of-truth file per topic.

## Ownership Map
- Source-of-truth and precedence map: 00-source-of-truth-map.md
- Product goals and target users: 00-product-vision.md
- MVP inclusion and exclusions: 01-mvp-scope.md
- Roles and access model: 02-roles-and-access.md
- Tenant isolation rules: 03-multitenancy-rules.md
- Technical structure and deployment flow: 04-architecture.md
- Entity model and relationships: 05-data-model.md
- Current execution plan: 06-current-sprint.md
- Change history: 07-ai-worklog.md
- Delivery status and milestones: 08-project-tracking.md
- Future ideas backlog: 09-punch-list.md
- Tools/accounts inventory: 10-tools-and-accounts.md
- External technical references: 11-frontend-backend-references.md
- Cross-session behavior rules: 12-global-rules.md
- Project-specific engineering rules: 13-project-rules.md
- Access/menu matrix by role: 14-admin-access-menu-matrix.md
- Database implementation status: 15-database-setup-tracker.md
- Assistant startup prompts: 16-assistant-bootstrap-prompts.md
- Frontend inspiration/checklist: 17-frontend-ideas-and-references.md
- Backend inspiration/checklist: 18-backend-ideas-and-references.md
- Doc governance: 19-doc-governance-and-boundaries.md
- AI tool responsibility matrix: 20-ai-skill-matrix.md
- AI operating rules: 21-ai-operating-rules.md
- Curated official references: 22-reference-links.md
- Security baseline: 23-security-baseline.md
- SaaS stack guidance: 24-saas-stack-guidance.md
- Playbooks (fixing/debug/validation commands): project-docs/playbooks/*
- Reusable skill templates: project-docs/skills/*

## Non-Overlap Rules
- Do not duplicate full rules across multiple files; link to the owner file.
- Do not put implementation status in architecture files; put status in tracking files.
- Do not put backlog items in sprint files; put backlog in punch list.
- Keep prompts short and reference governing files instead of restating them.

## Change Rule
When adding a new markdown file, add its scope and owner here first.

## Mandatory Pre-Coding Read Set for AI Agents
Before coding, AI agents must read:
- `project-docs/05-data-model.md`
- `project-docs/02-roles-and-access.md`
- `project-docs/04-architecture.md`
- `project-docs/06-current-sprint.md`

Note:
Current sprint file is `06-current-sprint.md` (not `16-current-sprint.md`).

## Schema Change Rule (Explicit)
Never change database schema decisions without updating:
- `project-docs/05-data-model.md`
and, when applicable:
- `project-docs/15-database-setup-tracker.md`

## v5 Reference Artifact Rule
Use `C:\PropertyManagement\property_management_platform_access_xmind_freemind_v5.mm` as a reference artifact for menu/access structure.
It is a navigation reference, not a replacement for source-of-truth rule/data docs.

## Pre-Coding Read Priority (Reinforced)
AI agents must read before coding:
1. 05-data-model.md
2. 02-roles-and-access.md
3. 04-architecture.md
4. 06-current-sprint.md
5. 14-admin-access-menu-matrix.md


## v6 Permission-Model Documentation Rule
Permission model changes must update both:
- `02-roles-and-access.md`
- `14-admin-access-menu-matrix.md`
before coding implementation begins.

## v6 Reference Artifact Rule
Treat `C:\PropertyManagement\property_management_platform_access_xmind_freemind_v6.mm` as an access/menu reference artifact.


## Anti-Duplication Architecture Rule for AI
AI agents must not create duplicated architectures when reusable patterns already exist in documented design.
Extend existing shared modules/services/components first.

## Permission and Workflow Documentation Gate
Before coding permission/workflow changes, docs must be aligned in:
- 02-roles-and-access.md
- 14-admin-access-menu-matrix.md
- 05-data-model.md
- 04-architecture.md


## Governance Rule: Platform Operations Are Architecture-Level
The following are architecture-level requirements and must be documented and approved before implementation:
- maintenance/interruption controls
- forced logout/session policies
- monitoring and health visibility
- backup/restore strategy
- tenant feature enablement
- issue reporting module boundaries
- future internal messaging boundaries

AI agents must not proceed with implementation of these capabilities unless corresponding docs are updated and aligned.


## Monetization Documentation Gate
Monetization/add-on architecture must be documented and approved before implementation:
- catalog/bundles
- entitlements/subscriptions
- billing/suspension/reactivation
- data-preservation behavior
- listings publication scope and pricing model


## Module Classification Documentation Gate
Before implementation, docs must explicitly classify affected modules as:
- Core Platform
- Optional Add-On
- Design-Now / Implement-Later
- Future

Any data-model change for optional add-ons must update:
- 01-mvp-scope.md (classification)
- 05-data-model.md (core vs add-on entity separation)
- 14-admin-access-menu-matrix.md (admin control behavior)


## Architecture Expansion Documentation Gate
Before implementation, docs must be updated for:
- plan/package impacts
- lifecycle/offboarding impacts
- impersonation/support mode impacts
- workflow/approval engine reuse impacts
- export/reporting impacts
- optional add-on commercial rule impacts


## Architectural Controls Documentation Gate
Before schema or implementation changes, docs must be updated for:
- data classification policy
- permission inheritance model
- feature flag resolution order
- module dependency rules
- configuration versioning/rollback
- upgrade/rollout strategy
- UI capability registry mapping
- data consistency strategy


## Import Isolation Documentation Gate
Before import-schema or import-service implementation:
- architecture must state tenant context is authoritative for imports
- docs must prohibit file-controlled tenant_id/internal IDs
- matching scope rules (tenant/default vs global/allowlist) must be documented
- batch-level import audit requirements must be documented


## v7 Diagram Alignment Gate
Before schema or implementation updates for v7 operations:
- verify preventive maintenance and asset register coverage
- verify leasing pipeline coverage
- verify owner ledger and operational documents coverage
- verify canonical KPI naming consistency across architecture, data model, and menu matrix




## AI Governance and Reference Rule
- AI operating behavior is governed by 20-ai-skill-matrix.md and 21-ai-operating-rules.md.
- Curated external references belong in 22-reference-links.md.
- Security baseline guidance belongs in 23-security-baseline.md.
- Stack/platform direction belongs in 24-saas-stack-guidance.md.
- These files support source-of-truth execution and must not duplicate owner-file business rules.




## Canonical Documentation Structure
Expected primary structure:
- 00-product-vision.md
- 00-source-of-truth-map.md
- 01-mvp-scope.md
- 02-roles-and-access.md
- 03-multitenancy-rules.md
- 04-architecture.md
- 05-data-model.md
- 06-current-sprint.md
- 07-ai-worklog.md
- 09-punch-list.md
- 11-frontend-backend-references.md
- 12-global-rules.md
- 13-project-rules.md
- 14-admin-access-menu-matrix.md
- 17-frontend-ideas-and-references.md
- 18-backend-ideas-and-references.md
- 19-doc-governance-and-boundaries.md
- 20-ai-skill-matrix.md
- 21-ai-operating-rules.md
- 23-security-baseline.md
- 24-saas-stack-guidance.md

Validation rule:
Startup/read-set files should reference the source-of-truth structure consistently and avoid orphaning canonical owner documents.


## Foundation Readiness Gate
Before architecture diagram v8 or database schema design begins, documentation should explicitly cover:
- module registry and lifecycle
- shared service boundaries
- platform/domain event model
- monitoring and health signals
- background job operating rules
- search/indexing strategy
- archive vs soft delete vs immutable historical record rules
- state transition direction for key workflow entities
- master/reference data vs transactional data
- dangerous admin action safeguards
- performance intent and scalability direction
