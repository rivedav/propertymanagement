# Debugging and Validation Execution Standard

## Pre-Implementation Gate
Before changing code, complete:
- Deterministic reproduction steps
- Root-cause hypothesis
- Validation plan with pass/fail criteria

## Minimum Validation Sequence
1. Run targeted check for the failing behavior.
2. Run related module checks.
3. Run broad safety checks (at least typecheck and lint where available).

## Repetition Rule
For high-risk areas (auth, permissions, tenancy, payments, migrations):
- Repeat critical verification 2 to 3 times.
- Use at least two contexts when applicable (different role or tenant).

## Multi-Tenant Validation Rules
- Verify no cross-tenant data read.
- Verify no cross-tenant write.
- Verify role constraints in tenant scope.
- Verify audit log entry for privileged actions.

## Completion Criteria
A task is complete only when:
- Root cause is proven.
- Fix is minimal and scoped.
- Validation checks pass.
- AI worklog entry is updated with what/why/risk.
