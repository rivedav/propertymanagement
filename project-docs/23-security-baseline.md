# Security Baseline

## Purpose
Define the minimum security baseline for the platform before schema design and implementation expand further.

## Security Principles
- secure by design
- least privilege by default
- tenant isolation is non-negotiable
- privileged actions must be auditable
- security decisions must be documented before implementation

## Identity and Account Creation
- account creation is invite-only unless future docs explicitly approve another model
- global user identity is controlled through approved onboarding flows
- high-risk onboarding changes must be documented before implementation

## MFA Baseline
MFA is required for privileged roles such as:
- Platform Admin
- Tenant Admin
- other elevated roles approved by policy

MFA coverage should apply to administrative and sensitive operational access.

## Session Rules
- session behavior must follow documented session policy
- privileged access must respect stricter session controls
- forced logout and maintenance/session controls must remain auditable
- session/security design must remain compatible with shared-database tenant isolation

## Password Handling Guidance
- password handling must rely on the authentication provider and approved security flows
- do not store raw passwords
- do not design custom password storage logic when managed auth covers the need
- password reset and credential recovery flows must remain controlled and auditable

## Audit Logging Requirements
The platform must audit:
- privileged access changes
- role and permission changes
- approval decisions
- financial state changes
- security-sensitive configuration changes
- support/impersonation actions
- backup/restore and operational control actions

## Backup and Restore Governance
- backup and restore operations are governance-controlled
- restore operations must be auditable
- tenant-safe restore constraints must be documented before implementation
- recovery workflows must not compromise tenant isolation

## Tenant Isolation Protection Expectations
- tenant context is mandatory for tenant-scoped operations
- cross-tenant leakage must be prevented by design
- access must consider role, relationship, lease, assignment, permission, and policy where applicable
- security decisions must remain compatible with RLS and app-layer authorization

## Implementation Guidance
- use approved auth and authorization patterns
- prefer reuse of security-capable shared engines
- do not defer auditability, permission boundaries, or tenant isolation decisions until after schema work
