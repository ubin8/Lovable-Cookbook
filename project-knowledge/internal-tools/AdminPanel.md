# Admin Panel Project Knowledge

Defines durable context for one Lovable admin panel project. Replace placeholders, remove irrelevant sections, and keep the final text below Lovable's limit. Workspace Knowledge still applies unless this file is more specific.

## Product

- Name: [PRODUCT NAME]
- Purpose: [WHAT THE ADMIN PANEL CONTROLS]
- Primary users: [SUPPORT / OPERATIONS / ADMIN / SECURITY / OTHER]

## Admin Scope

- Managed system: [PRODUCT / PLATFORM / TENANT / WORKSPACE]
- Admin types: [SUPPORT / OPERATIONS / TENANT ADMIN / GLOBAL ADMIN / OTHER]
- Tenant boundary: [NONE / ORGANIZATION / WORKSPACE / ACCOUNT]
- Support access model: [DIRECT / APPROVAL / TIME-BOUND / IMPERSONATION]

Rules:

- Admin access is not automatically global.
- Derive access from trusted role, tenant, team, case, and resource relationships.
- Client-supplied role, tenant, target user, action scope, or approval state is not authorization proof.
- Distinguish tenant-admin, support, operations, and global-platform powers.
- Document intentional cross-tenant or global actions.

## Roles and Permissions

| Role | Scope | Allowed | Restricted |
|---|---|---|---|
| [SUPPORT] | [CASE / USER / TENANT] | [ACTIONS] | [LIMITS] |
| [OPERATIONS] | [RESOURCE / TENANT] | [ACTIONS] | [LIMITS] |
| [ADMIN] | [TENANT / GLOBAL] | [ACTIONS] | [LIMITS] |

Rules:

- Enforce permissions at a trusted backend or database boundary.
- Use least privilege for every admin role.
- Separate read, edit, approve, delete, impersonate, export, and billing powers.
- High-risk actions may require step-up authentication, approval, or dual control.
- [PROJECT-SPECIFIC ACCESS RULES]

## Critical Flows

1. [USER SEARCH OR RECORD LOOKUP]
2. [VIEW ACCOUNT OR TENANT]
3. [CHANGE ROLE OR MEMBERSHIP]
4. [SUSPEND, REACTIVATE, OR DELETE]
5. [MANUAL CORRECTION OR OVERRIDE]
6. [IMPERSONATION OR SUPPORT SESSION]
7. [EXPORT OR AUDIT REVIEW]
8. [BILLING OR ENTITLEMENT CORRECTION]

Critical flows need clear states, trusted authorization, duplicate-submit prevention, auditability, and recovery where practical.

## User and Tenant Administration

- User states: [INVITED / ACTIVE / SUSPENDED / DEACTIVATED / DELETED]
- Tenant states: [TRIAL / ACTIVE / SUSPENDED / CLOSED / DELETED]
- Membership roles: [LIST]

Rules:

- Validate role, membership, suspension, deletion, and ownership changes server-side.
- Do not remove the last required owner or leave a tenant without recoverable administration.
- Role changes must account for stale claims, caches, signed URLs, realtime, and queued jobs.
- Define suspension effects on login, API access, jobs, webhooks, files, and billing.
- [PROJECT-SPECIFIC LIFECYCLE RULES]

## Support Access and Impersonation

Remove if impersonation is not used.

- Impersonation roles: [ROLES]
- Maximum duration: [DURATION]

Rules:

- Impersonation must be explicit, time-bound, attributable, and revocable.
- Require a reason, case/ticket reference, and target scope.
- Show a persistent impersonation indicator.
- Never reveal passwords, raw tokens, secrets, or authentication factors.
- Log start, end, actor, target, reason, scope, and actions.
- [PROJECT-SPECIFIC IMPERSONATION RULES]

## Manual Corrections and Overrides

- Override types: [STATUS / OWNERSHIP / ENTITLEMENT / BILLING / DATA / OTHER]
- Approval model: [NONE / MANAGER / DUAL CONTROL]

Rules:

- Prefer normal product flows over direct database mutation.
- Validate actor, target, current state, requested state, and side effects.
- Do not trust client-provided status, owner, role, entitlement, price, or balance.
- Preserve before/after values for sensitive changes.
- Define override side effects on notifications, jobs, billing, and recalculation.
- [PROJECT-SPECIFIC OVERRIDE RULES]

## Bulk Actions

- Supported bulk actions: [LIST]

Rules:

- Bulk actions need explicit scope, counts, filters, preview, and confirmation.
- Revalidate every affected record at execution time.
- Authorize every record independently.
- Support partial-failure reporting and safe retry.
- Use idempotency where duplicate execution could cause harm.
- [PROJECT-SPECIFIC BULK RULES]

## Audit Logs

- Audited actions: [LIST]

Rules:

- Record actor, target, action, time, scope, reason, and relevant before/after values.
- Protect audit logs from ordinary modification.
- Exclude passwords, secrets, raw tokens, and unnecessary sensitive payloads.
- Audit access must be authorized and auditable.
- Distinguish user actions, admin actions, automated jobs, and provider events.
- Do not claim immutable or tamper-proof logging without evidence.

## Search, Filters, and Data Visibility

- Searchable records: [USERS / TENANTS / ORDERS / CASES / OTHER]
- Sensitive fields: [LIST]
- Cross-tenant search: [NONE / LIMITED / GLOBAL]

Rules:

- Search results must respect admin scope and tenant boundaries.
- Do not retrieve globally and hide unauthorized rows only in the frontend.
- Mask or omit sensitive fields unless required for the task.
- Filters, counts, autocomplete, and aggregates must not leak restricted records.
- Saved views and exports must preserve access boundaries.
- [PROJECT-SPECIFIC SEARCH RULES]

## Billing and Entitlements

Remove if unused.

- Billing provider: [PROVIDER]

Rules:

- Provider state is the source of truth where applicable.
- Client-supplied plan, price, balance, payment, refund, or entitlement values are untrusted.
- Bind billing objects and refunds to the correct tenant.
- Separate support visibility from financial and entitlement powers.
- [PROJECT-SPECIFIC BILLING RULES]

## Backend, Security, and Privacy

- Backend: [LOVABLE CLOUD / SUPABASE]
- Authentication: [METHODS]
- Sensitive data: [LIST]

Rules:

- Use RLS for exposed private data.
- Keep privileged operations and service-role clients server-side.
- Validate actor, target, tenant, role, approval, and current state for protected actions.
- Apply stronger controls to exports, impersonation, role changes, deletion, and financial actions.
- Apply retention/deletion across database, files, exports, caches, logs, and external systems.

## Design and Content

- Visual style: [STYLE]

Rules:

- Show actor, target, scope, current state, and consequences for high-impact actions.
- Distinguish read-only data, editable fields, derived values, and provider-controlled state.
- Make destructive, financial, permission, and impersonation actions distinct.
- Avoid ambiguous labels such as "Fix", "Reset", or "Override" without explaining the result.

## Testing

- Required checks: [BUILD / TYPE CHECK / LINT / TESTS / BROWSER TESTS]
- Critical flows to verify: [LIST]

Rules:

- Test support, operations, tenant-admin, and global-admin scopes separately.
- Test own/other tenants, unauthorized records, suspended users, stale sessions, and revoked roles.
- Test impersonation start, scope, expiry, revocation, and audit records where applicable.

## Prohibited Patterns

Do not:

- grant global access merely because a user is an admin
- trust client-supplied role, tenant, target, approval, status, entitlement, or billing values
- expose passwords, tokens, secrets, authentication factors, or unrelated sensitive data
- use impersonation without attribution, scope, expiry, and audit logging
- execute bulk changes without preview, confirmation, per-record authorization, and failure reporting
- mutate production data directly when a safer product flow exists
- weaken RLS, validation, constraints, or permissions to make an admin action work
- claim auditability, compliance, recovery, or production readiness without evidence

## Completion Standard

Verify admin scope, authorization, tenant boundaries, approvals, audit records, side effects, and applicable checks. State unverified areas, assumptions, and residual risks.
