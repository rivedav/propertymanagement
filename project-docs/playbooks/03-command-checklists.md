# Command Checklists

## Purpose
Provide a single command runbook for consistent validation before and after changes.

## Standard Validation (Default)
1. `npm run lint`
2. `npm run typecheck`
3. `npm test`
4. `npm run build`

## Bug Fix Validation (After Root Cause Confirmed)
1. Run the targeted failing check first (module/test/route-specific).
2. Run standard validation sequence.
3. Re-run the targeted failing check.

## High-Risk Changes (Auth, RLS, Permissions, Migrations)
1. Run targeted check in tenant/user context A.
2. Run targeted check in tenant/user context B.
3. Repeat critical check 2-3 times.

## Database and Policy Changes
1. Apply migration in local/dev environment.
2. Verify policy behavior with allowed role.
3. Verify policy denial with disallowed role.
4. Verify cross-tenant denial.
5. Run standard validation sequence.

## If a Command Fails
1. Capture exact error output.
2. Map failing layer (UI/API/Auth/Policy/DB).
3. Return to root-cause playbook before editing additional code.

## Notes
- If a script does not exist yet in `package.json`, add it before enforcing this checklist.
- Keep this file updated when tooling changes (Vitest, Playwright, Supabase CLI, etc.).
