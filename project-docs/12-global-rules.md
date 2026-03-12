# Global Rules

## Communication and Reasoning Rules
- Give direct, factual answers.
- Prefer truth over optimism.
- Explicitly call out risks, assumptions, and unknowns.
- Avoid marketing language and vague reassurance.

## Dual-Assistant Rules (Claude Code + ChatGPT)
- Treat project-docs as the single source of truth.
- Before coding, read relevant project-docs files.
- If assistants disagree, choose the safer, simpler path that matches docs.

## Workflow Rules
- Keep changes small, testable, and reversible.
- Log meaningful changes in project-docs/07-ai-worklog.md.
- Use playbooks for root-cause fixing and validation execution.

## Safety Rules
- Avoid destructive commands unless explicitly approved.
- Do not expose secrets in code, docs, or logs.
- Validate tenant and role checks on all write operations.
- Enforce tenant context server-side and database-side.

## Reuse and Configuration Rules
- Prefer reuse over duplication.
- Prefer configuration over hardcoding when appropriate.
- Do not add isolated logic if an existing service/component can be extended.
- Treat workflow definitions, templates, and permission matrices as configurable assets.


## Reuse and Feature-Flag Rules
- Design for modular reuse first.
- Prefer feature flags over hardcoded tenant-specific branches.
- Prefer configuration-driven behavior over duplicated logic.
- New features should extend existing engines/services when feasible.
- Avoid isolated parallel implementations of equivalent capabilities.


## Monetization and Module Rules
- prefer reusable engines over one-off feature code
- optional modules must use feature flags and entitlements
- deactivation must preserve data unless explicit retention policy says otherwise
- future modules must extend existing architecture before creating new isolated paths


## Reusable Engine Enforcement Rules
- new features must extend reusable engines before adding isolated implementations
- commercial behavior must be configuration-driven, not hardcoded
- optional modules must use entitlement + feature flag checks
- approval paths should use shared workflow engine contracts


## Platform Capability Governance Rules
- every new capability must declare retention, audit, and export behavior
- public/external API features must include abuse protection controls
- async workloads must use centralized queue/orchestration patterns
- connectors/integrations must use reusable framework and secret controls
- architecture docs must classify capability as core, add-on, design-now, or future


## Architectural Control Rules
- every module must declare data classification level(s)
- permission inheritance and override behavior must be explicit and auditable
- feature resolution must follow documented precedence order
- optional module dependencies must be declared before implementation
- UI-configurable settings must support versioning and rollback
- critical writes must define transaction + idempotency + retry behavior


## Governance Precedence Rule
If any rule wording conflicts across docs, `19-doc-governance-and-boundaries.md` defines documentation precedence and ownership.


## Terminology Normalization Rule
All docs must use canonical terms defined in `11-frontend-backend-references.md`.
Do not introduce synonym drift (for example: Website Admin vs Platform Admin) without aliasing.



## Foundation Governance Rules
- Every foundation capability should declare whether it is configuration, master data, transactional data, event data, or operational data.
- Every async capability should define retry, failure, visibility, and idempotency behavior before implementation.
- Every dangerous administrative action should define stronger audit and safeguard behavior before implementation.
- Search and indexing behavior must be tenant-safe and permission-aware by design.
