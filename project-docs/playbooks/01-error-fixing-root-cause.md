# Error Fixing and Root Cause Playbook

## Objective
Fix defects by proving the true root cause before implementing code changes.

## Mandatory Protocol
1. Reproduce consistently.
2. Capture evidence (error message, stack trace, logs, request payload, DB state).
3. Isolate failing layer (UI, API, auth, policy, DB, integration).
4. Form a falsifiable root-cause hypothesis.
5. Validate hypothesis with a focused check (query, unit test, or minimal repro).
6. Implement the smallest safe fix.
7. Verify with targeted tests and regression checks.
8. Document root cause and fix in AI worklog.

## Root Cause Standard
A fix is not complete unless all are true:
- The specific failing condition is identified.
- The mechanism of failure is explained.
- Evidence shows the fix addresses that mechanism.
- A regression check prevents silent reintroduction.

## Anti-Patterns to Avoid
- Patching symptoms without proving cause.
- Large refactors during urgent bug fixes.
- Multiple unrelated changes in one fix.
- Declaring success after one unrepeatable run.

## Evidence Checklist
- Exact error text
- Time and route/action that triggered issue
- User/tenant context used in reproduction
- Pre-fix and post-fix behavior notes
