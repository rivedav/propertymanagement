# Source of Truth Map

## Purpose
Define documentation ownership and precedence to reduce AI and implementation drift.

## Authoritative Scope
- `project-docs/*` is the authoritative source of truth for this repository.
- Top-level design/progress documents are contextual summaries only.
- If conflicts exist, follow the owning canonical file listed below.

## Canonical Ownership Map
- Source-of-truth and precedence owner: `project-docs/00-source-of-truth-map.md`
- Product vision owner: `project-docs/00-product-vision.md`
- MVP scope owner: `project-docs/01-mvp-scope.md`
- Roles/access owner: `project-docs/02-roles-and-access.md`
- Architecture owner: `project-docs/04-architecture.md`
- Data model owner: `project-docs/05-data-model.md`
- Current sprint owner: `project-docs/06-current-sprint.md`
- Tracking/status owner: `project-docs/08-project-tracking.md`
- Access/menu matrix owner: `project-docs/14-admin-access-menu-matrix.md`
- Assistant bootstrap owner: `project-docs/16-assistant-bootstrap-prompts.md`
- Governance/precedence owner: `project-docs/19-doc-governance-and-boundaries.md`
- AI skill allocation owner: `project-docs/20-ai-skill-matrix.md`
- AI operating rules owner: `project-docs/21-ai-operating-rules.md`
- Curated official references owner: `project-docs/22-reference-links.md`
- Security baseline owner: `project-docs/23-security-baseline.md`
- SaaS stack guidance owner: `project-docs/24-saas-stack-guidance.md`
- Global rules owner: `project-docs/12-global-rules.md`
- Project-specific implementation rules owner: `project-docs/13-project-rules.md`

## Context and Reference-Only Documents
These provide context but do not override canonical project-docs files:
- `CLAUDE.md`
- `README.md`
- `Detailed-Design-Document.md`
- `Implementation-Progress-Report.md`
- `project-docs/07-ai-worklog.md`
- `project-docs/11-frontend-backend-references.md`
- `project-docs/17-frontend-ideas-and-references.md`
- `project-docs/18-backend-ideas-and-references.md`

## Conflict Resolution Rule
When two files disagree:
1. Identify the owning canonical file from the ownership map.
2. Follow the canonical file.
3. Update non-canonical files to remove or clarify drift.
4. Record meaningful updates in `project-docs/07-ai-worklog.md`.

## Startup Alignment Rule
- `CLAUDE.md`, `project-docs/README.md`, and `project-docs/16-assistant-bootstrap-prompts.md` should all reference this file.
- If startup/read-set files drift from the canonical ownership map, update those startup/read-set files to match this file and `project-docs/19-doc-governance-and-boundaries.md`.
