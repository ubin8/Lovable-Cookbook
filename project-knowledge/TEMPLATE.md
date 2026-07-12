# Project Knowledge Template

Defines durable context for one Lovable project. Replace placeholders and remove irrelevant sections. Workspace Knowledge still applies unless this file is more specific.

## Product

- Name: [PRODUCT NAME]
- Type: [SAAS / MARKETPLACE / INTERNAL TOOL / OTHER]
- Stage: [PROTOTYPE / MVP / BETA / PRODUCTION]
- Description: [WHAT THE PRODUCT DOES]
- Main goal: [PRIMARY BUSINESS OUTCOME]
- Primary users: [USER TYPES AND GOALS]
- Non-goals: [LIST]

## Critical Flows

1. [FLOW]
2. [FLOW]
3. [FLOW]
4. [FLOW]

Critical flows need clear loading, success, empty, and error states; duplicate-submit prevention; trusted authorization; data integrity; and recovery where practical.

## Domain Model

- [CONCEPT]: [MEANING]
  - Relationships: [RELATIONSHIPS]
  - Rules: [BUSINESS RULES]
- [CONCEPT]: [MEANING]
  - Relationships: [RELATIONSHIPS]
  - Rules: [BUSINESS RULES]

Use domain terms consistently in UI, code, database naming, validation, and documentation.

## Roles and Permissions

| Role | Scope | Allowed | Restricted |
|---|---|---|---|
| [ROLE] | [GLOBAL / TENANT / RESOURCE] | [ACTIONS] | [LIMITS] |
| [ROLE] | [GLOBAL / TENANT / RESOURCE] | [ACTIONS] | [LIMITS] |

- Authentication proves identity; authorization determines allowed actions.
- Enforce permissions at a trusted backend or database boundary.
- UI visibility, routes, UUIDs, and client state are not authorization.
- Undefined access is denied by default.
- [PROJECT-SPECIFIC ACCESS RULES]

## Tenant Model

Remove if no tenant boundary exists.

- Tenant type: [ORGANIZATION / WORKSPACE / TEAM / ACCOUNT]
- Tenant identifier: [FIELD OR CONCEPT]
- Membership model: [DESCRIPTION]
- Multiple memberships: [YES / NO]
- Active tenant selection: [DESCRIPTION]
- Shared resources: [NONE / DESCRIPTION]
- Tenant resources: [LIST]

- Derive access from trusted membership/resource relationships.
- A client-supplied tenant ID is not proof of membership.
- Scope data, files, search, exports, analytics, jobs, integrations, caches, and realtime to the effective tenant.
- Validate invitations, role changes, removals, ownership transfer, and tenant switching server-side.
- Document intentional cross-tenant sharing.

## Backend and Data

- Backend: [LOVABLE CLOUD / SUPABASE / OTHER]
- Authentication: [METHODS]

| Object | Purpose | Scope | Sensitivity |
|---|---|---|---|
| [OBJECT] | [PURPOSE] | [USER / TENANT / GLOBAL] | [CLASSIFICATION] |
| [OBJECT] | [PURPOSE] | [SCOPE] | [CLASSIFICATION] |

- Use versioned migrations.
- Preserve data unless deletion is explicit and reviewed.
- Use constraints for enforceable integrity.
- Use RLS for exposed private data in Lovable Cloud or Supabase.
- Keep privileged operations server-side.
- Do not trust client-provided owner, tenant, role, price, status, or permission values.
- Avoid destructive changes without dependency and data analysis.

Retention/deletion: [RULES]

## Account Lifecycle

States: [INVITED / ACTIVE / SUSPENDED / DEACTIVATED / DELETED]

- [SIGN-UP OR INVITATION RULE]
- [SESSION OR VERIFICATION RULE]
- [ROLE-CHANGE RULE]
- [DEACTIVATION OR DELETION RULE]
- [OWNED-DATA RULE]

An authentication invitation does not automatically grant application membership or role authorization.

## Backend Logic and Integrations

Trusted operations: [EDGE FUNCTION / SERVER ROUTE / RPC / JOB / WEBHOOK]

Approved integrations:
- [PROVIDER]: [PURPOSE], scoped to [USER / TENANT / GLOBAL]

- Keep secrets and privileged credentials server-side.
- Validate the actor, target resource, and effective tenant.
- Service-role clients must enforce authorization explicitly.
- Bind provider accounts, OAuth connections, credentials, and external IDs to the correct user, resource, or tenant.
- Verify webhooks with the provider-supported method.
- Make retryable processing idempotent and handle duplicate, delayed, missing, and out-of-order events.
- Do not expose tokens, secrets, or sensitive provider payloads to browser code or logs.

## Files, Async, Search, AI, and Exports

Remove irrelevant lines.

- Files/buckets: [DESCRIPTION]
- Jobs/queues: [DESCRIPTION]
- Realtime: [DESCRIPTION]
- Search/analytics: [DESCRIPTION]
- AI/RAG: [DESCRIPTION]
- Exports: [DESCRIPTION]

- Authorize private files independently from UI visibility.
- Validate client-supplied paths against the actor, resource, and tenant.
- Treat signed URLs as temporary bearer credentials.
- Jobs must use a server-validated resource and tenant context.
- Search, analytics, AI retrieval, and exports must enforce source-data access boundaries.
- Derived data must retain an enforceable relationship to its source.
- Store temporary exports privately with expiration and deletion.
- AI prompts, embeddings, logs, and provider payloads must not contain unauthorized or unnecessary sensitive data.

## Billing

Remove if unused.

- Provider: [PROVIDER]
- Scope: [USER / TENANT / USAGE / TRANSACTION]
- Plans: [PLANS]
- Rules: [TRIAL / UPGRADE / DOWNGRADE / CANCELLATION / REFUND / FAILED PAYMENT / ENTITLEMENTS]

Client-supplied plan, price, discount, payment status, or entitlement values are untrusted. Paid access must come from trusted billing state.

## Design and Content

- Visual style: [STYLE]
- Brand traits: [TRAITS]
- Theme: [LIGHT / DARK / BOTH]
- Tone: [TONE]
- Preferred terminology: [TERMS]

- Follow the connected design system and shared components.
- Avoid one-off components when a shared component fits.
- Make destructive actions distinct and require proportionate confirmation.
- Keep UI text clear and consistent.
- Do not promise unimplemented features, security, compliance, or outcomes.
- Accessibility is required for critical flows.

## Security and Privacy

- Sensitive data: [LIST]
- Compliance/policy: [REQUIREMENTS]
- Project-specific rules: [LIST]

- Collect only data required for the product purpose.
- Restrict access by user, role, tenant, and resource relationship.
- Never place secrets or sensitive data in client code, URLs, logs, analytics, or user-visible errors.
- Security-sensitive actions fail closed.
- Do not claim compliance, encryption, backup, restore, deletion, or isolation without evidence.
- Apply retention/deletion across database, storage, exports, caches, logs, and external systems.

## Testing and Operations

- Required checks: [BUILD / TYPE CHECK / LINT / TESTS / BROWSER TESTS]
- Critical flows to verify: [LIST]

- Test allowed and denied behavior for protected features.
- Test relevant loading, empty, validation, error, retry, and duplicate-submit states.
- Use controlled accounts and fixtures.
- Inspect relevant console and network failures.
- Do not mark untested behavior as verified.
- Use timeouts and bounded retries for external calls.

## Prohibited Patterns

Do not:
- replace the established frontend/backend stack without approval
- create parallel auth, database, storage, or permission systems without a clear requirement
- use client state as authorization
- expose secrets or privileged credentials
- weaken RLS, validation, constraints, or permissions to make a feature work
- perform destructive changes without impact analysis
- claim testing, deployment, security, or production readiness without evidence
- leave mocks, bypasses, debug code, or test behavior enabled in production

## Decision Priority

1. Explicit instruction for the current task
2. Project Knowledge
3. Connected design-system rules and established architecture
4. Workspace Knowledge
Do not silently resolve material conflicts. Preserve the safer or less destructive behavior until clarified.

## Completion Standard

1. Confirm requested and affected existing behavior.
2. Verify relevant business rules and permissions.
3. Run or describe applicable checks.
4. Review security, data, compatibility, design, and operational impact.
5. Remove temporary code/debugging.
6. State unverified areas, assumptions, and residual risks.
