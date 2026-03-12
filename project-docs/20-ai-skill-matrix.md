# AI Skill Matrix

## Purpose
Define recommended responsibility boundaries for AI tools used on this project so planning, documentation, implementation, and review stay consistent.

## Source-of-Truth Rule
This file allocates recommended AI responsibilities.
Business truth, architecture truth, and scope truth still come from `project-docs/*`.

## Recommended Role Allocation

### ChatGPT
Best used for:
- architecture definition and review
- scope control
- database design review before schema work
- module boundary review
- security review
- monetization and entitlement architecture
- reusable engine identification
- governance and dependency analysis

Typical outputs:
- architecture proposals
- design critiques
- risk analysis
- documentation gap detection
- pre-schema review notes

### Claude Code
Best used for:
- implementation
- refactoring
- code cleanup
- reusable module construction
- tests
- issue fixing with root-cause validation
- structured repository changes with local verification

Typical outputs:
- code changes
- test additions
- refactors
- implementation follow-through
- validation results

### Codex
Best used for:
- documentation alignment
- issue-by-issue implementation
- consistency audits
- second-opinion review
- targeted repo updates with cross-file alignment
- specification reinforcement before coding

Typical outputs:
- doc updates
- gap analysis
- implementation follow-ups
- consistency checks between scope, architecture, and data model

### Gemini
Best used for:
- detailed design documentation
- implementation progress tracking
- documentation drafting from existing decisions
- screenshot-aware documentation support
- narrative progress summaries and structured write-ups

Typical outputs:
- detailed design docs
- progress reports
- screenshot-supported notes
- implementation summaries

## Decision Pattern
Use the tool best suited for the current phase:
1. Define or review architecture with ChatGPT.
2. Align docs and constraints with Codex.
3. Implement and validate with Claude Code.
4. Produce detailed progress/design write-ups with Gemini.

## Escalation Guidance
Escalate to another AI when:
- architecture is unclear
- repo docs are inconsistent
- implementation needs a second opinion
- security or multitenancy implications are non-trivial
- schema direction is about to be locked in

## Non-Delegation Rule
No AI tool may override:
- `project-docs/00-product-vision.md`
- `project-docs/01-mvp-scope.md`
- `project-docs/02-roles-and-access.md`
- `project-docs/04-architecture.md`
- `project-docs/05-data-model.md`
- `project-docs/19-doc-governance-and-boundaries.md`
- `project-docs/21-ai-operating-rules.md`
