# SaaS Project Knowledge

Defines durable context for one Lovable SaaS project. Replace placeholders, remove irrelevant sections, and keep the final text below Lovable's limit. Workspace Knowledge still applies unless this file is more specific.

## Product

- Name: [PRODUCT NAME]
- Stage: [PROTOTYPE / MVP / BETA / PRODUCTION]
- Description: [WHAT THE PRODUCT DOES]
- Main goal: [PRIMARY BUSINESS OUTCOME]
- Primary users: [USER TYPES AND GOALS]
- Non-goals: [LIST]

## SaaS Model

- Tenant: [ORGANIZATION / WORKSPACE / TEAM / ACCOUNT]
- Membership model: [DESCRIPTION]
- Multiple memberships: [YES / NO]
- Tenant resources: [LIST]

Rules:

- Derive tenant access from trusted membership/resource relationships.
- A client-supplied tenant ID is not proof of membership.
- Scope data, files, search, analytics, exports, integrations, caches, jobs, webhooks, and realtime to the effective tenant.
- Evaluate roles in the correct tenant/resource scope.

## Roles and Permissions

| Role | Scope | Allowed | Restricted |
|---|---|---|---|
| [ROLE] | [TENANT / GLOBAL / RESOURCE] | [ACTIONS] | [LIMITS] |
| [ROLE] | [TENANT / GLOBAL / RESOURCE] | [ACTIONS] | [LIMITS] |
| [ROLE] | [TENANT / GLOBAL / RESOURCE] | [ACTIONS] | [LIMITS] |

- Authentication proves identity; authorization determines allowed actions.
- Enforce permissions at the database, server route, Edge Function, RPC, or another trusted boundary.
- UI visibility, hidden controls, routes, UUIDs, and client state are not authorization.
- Undefined access is denied by default.
- [PROJECT-SPECIFIC ACCESS RULES]

## Critical Flows

1. [SIGN-UP OR LOGIN]
2. [TENANT CREATION OR JOIN]
3. [INVITATION AND MEMBERSHIP]
4. [PRIMARY PRODUCT FLOW]
5. [BILLING OR PLAN CHANGE]
6. [OFFBOARDING OR DELETION]

Critical flows need clear states, duplicate-submit prevention, trusted authorization, data integrity, and recovery where practical.

## Domain Model

- Tenant: [MEANING AND RULES]
- Membership: [MEANING AND RULES]
- Role: [MEANING AND RULES]
- [CORE RESOURCE]: [MEANING, RELATIONSHIPS, RULES]
- [CORE RESOURCE]: [MEANING, RELATIONSHIPS, RULES]
- Subscription/entitlement: [MEANING AND RULES]

Use domain terms consistently in UI, code, database naming, validation, and documentation.

## Backend and Data

- Backend: [LOVABLE CLOUD / SUPABASE]
- Authentication: [METHODS]

| Object | Purpose | Scope | Sensitivity |
|---|---|---|---|
| [OBJECT] | [PURPOSE] | [USER / TENANT / GLOBAL] | [CLASSIFICATION] |
| [OBJECT] | [PURPOSE] | [SCOPE] | [CLASSIFICATION] |

- Use versioned migrations.
- Use constraints for enforceable integrity.
- Use RLS for exposed private data in Lovable Cloud or Supabase.
- Keep privileged operations server-side.
- Do not trust client-provided owner, tenant, role, plan, price, status, or permission values.
- Apply retention/deletion across database, files, exports, caches, logs, and external systems.

## Membership and Account Lifecycle

States: [INVITED / ACTIVE / SUSPENDED / REMOVED / DEACTIVATED / DELETED]

- An authentication invitation does not automatically grant tenant membership or role authorization.
- Bind invitations server-side to tenant, intended role, recipient, expiry, revocation, and prior use.
- Validate who may invite, assign roles, remove members, and transfer ownership.
- A user must not change tenant or role through payloads, metadata, or redirects.
- Membership/role changes must follow the session model and account for stale claims, caches, signed URLs, realtime, exports, and queued jobs.
- [LAST OWNER RULE]
- [OWNERSHIP TRANSFER RULE]
- [REMOVAL RULE]
- [ACCOUNT DELETION RULE]

## Billing and Entitlements

Remove if unused.

- Scope: [TENANT / USER / USAGE]
- Plans: [LIST]
- Trial model: [DESCRIPTION]
- Entitlement source: [DESCRIPTION]

- Client-supplied plan, price, discount, payment status, or entitlement values are untrusted.
- Paid access must come from trusted billing/entitlement state.
- Bind customers, subscriptions, invoices, and provider accounts to the correct tenant.
- Verify billing webhooks with the provider-supported method.
- Webhook authenticity alone does not prove the event belongs to the correct tenant.
- Make processing idempotent and handle duplicate or out-of-order events.
- Define upgrades, downgrades, failed payments, cancellation, grace periods, refunds, and reactivation.
- [PROJECT-SPECIFIC BILLING RULE]

## Backend Logic and Integrations

Trusted operations: [EDGE FUNCTION / SERVER ROUTE / RPC / JOB / WEBHOOK]

Approved integrations:
- [PROVIDER]: [PURPOSE], scoped to [USER / TENANT / GLOBAL]

- Keep secrets and privileged credentials server-side.
- Validate the actor, target resource, and effective tenant.
- Service-role clients must enforce authorization explicitly.
- Bind provider accounts, credentials, webhooks, and external IDs to the correct tenant.
- Job payloads, provider IDs, and webhook metadata are not authorization proof.
- Make retries idempotent.

## Files, Search, Analytics, AI, and Exports

Remove irrelevant lines.

- Files/buckets: [DESCRIPTION]
- Search/analytics: [DESCRIPTION]
- AI/RAG: [DESCRIPTION]
- Exports: [DESCRIPTION]

- Authorize private files independently from UI visibility.
- Validate client-supplied paths against the actor, resource, and tenant.
- Search, analytics, AI retrieval, and exports must enforce source-data access boundaries.
- Derived data must retain an enforceable source-tenant/resource relationship.
- AI prompts, embeddings, logs, and provider payloads must not contain unauthorized sensitive data.

## Async Processing and Realtime

Remove if unused.

- Jobs/queues: [DESCRIPTION]
- Webhooks: [DESCRIPTION]
- Realtime: [DESCRIPTION]

- Jobs need a server-validated resource and tenant context.
- Queue messages and webhook payloads are not authorization proof.
- Revalidate relevant membership, ownership, or tenant relationships before privileged processing.
- Make retries idempotent and safe against duplicate delivery.
- Realtime topics and payloads must follow the source-data tenant boundary.
- Consider role changes, removal, token refresh, reconnect, and open subscriptions.

## Design and Content

- Visual style: [STYLE]
- Brand traits: [TRAITS]
- Theme: [LIGHT / DARK / BOTH]
- Tone: [TONE]
- Preferred terminology: [TERMS]

- Follow the connected design system and shared components.
- Avoid one-off components when a shared component fits.
- Make destructive actions distinct and require proportionate confirmation.

## Security and Privacy

- Sensitive data: [LIST]
- Compliance/policy requirements: [LIST]
- Project-specific security rules: [LIST]

- Restrict access by user, role, tenant, and resource relationship.
- Never place secrets or sensitive data in client code, URLs, logs, analytics, or user-visible errors.
- Do not claim compliance, backup, deletion, or tenant isolation without evidence.

## Testing and Operations

- Required checks: [BUILD / TYPE CHECK / LINT / TESTS / BROWSER TESTS]
- Critical flows to verify: [LIST]

- Test allowed and denied behavior for protected features.
- Use two controlled tenants and multiple roles for tenant-sensitive tests.
- Test own-tenant, same-tenant, cross-tenant, removed-user, and stale-session cases where relevant.
- Test relevant loading, validation, error, retry, and duplicate-submit states.
- Do not mark untested behavior as verified.

## Prohibited Patterns

Do not:

- replace the established frontend/backend stack without approval
- create parallel auth, database, storage, or permission systems without a clear requirement
- use client state as authorization
- expose secrets or privileged credentials
- weaken RLS, validation, constraints, or permissions to make a feature work
- trust tenant, role, billing, or ownership values only because they came from the client
- perform destructive changes without impact analysis
- claim testing, security, tenant isolation, or production readiness without evidence
- leave mocks, bypasses, debug code, or test behavior enabled in production

## Decision Priority

1. Explicit instruction for the current task
2. Project Knowledge
3. Connected design-system rules and established architecture
4. Workspace Knowledge

Do not silently resolve material conflicts. Preserve the safer or less destructive behavior until clarified.

## Completion Standard

1. Confirm requested and affected existing behavior.
2. Verify business rules, tenant boundaries, and permissions.
3. Run or describe applicable checks.
4. Review security, data, billing, compatibility, design, and operational impact.
5. Remove temporary code/debugging and state unverified areas, assumptions, and residual risks.
