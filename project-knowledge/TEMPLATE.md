# Project Knowledge

This file defines durable context for one Lovable project. Replace the placeholders and remove irrelevant sections. Workspace Knowledge still applies unless this file defines a more specific rule.

## 1. Product

- Name: [PRODUCT NAME]
- Type: [SAAS / MARKETPLACE / INTERNAL TOOL / OTHER]
- Stage: [PROTOTYPE / MVP / BETA / PRODUCTION]
- Primary language: [LANGUAGE]
- Description: [WHAT THE PRODUCT DOES]
- Main goal: [PRIMARY BUSINESS OUTCOME]

Primary users:

- [USER TYPE]: [GOAL OR RESPONSIBILITY]
- [USER TYPE]: [GOAL OR RESPONSIBILITY]

Non-goals:

- [NON-GOAL]
- [NON-GOAL]

## 2. Critical Flows

1. [FLOW]
2. [FLOW]
3. [FLOW]
4. [FLOW]

Critical flows must:

- provide loading, empty, success, and error states
- prevent duplicate submissions
- preserve data integrity
- enforce authorization at a trusted boundary
- provide a clear recovery path where practical

## 3. Domain Model

Core concepts:

- [CONCEPT]: [MEANING]
  - Relationships: [RELATIONSHIPS]
  - Rules: [BUSINESS RULES]

- [CONCEPT]: [MEANING]
  - Relationships: [RELATIONSHIPS]
  - Rules: [BUSINESS RULES]

Use domain terms consistently in UI, code, database naming, validation, and documentation.

## 4. Roles and Permissions

| Role | Scope | Allowed | Restricted |
| --- | --- | --- | --- |
| [ROLE] | [GLOBAL / TENANT / RESOURCE] | [ACTIONS] | [LIMITS] |
| [ROLE] | [GLOBAL / TENANT / RESOURCE] | [ACTIONS] | [LIMITS] |

Rules:

- Authentication proves identity; authorization determines allowed actions.
- Enforce permissions at the database, server route, Edge Function, RPC, or another trusted backend boundary.
- UI visibility, routes, UUIDs, and client state are not authorization.
- Undefined access is denied by default.
- [PROJECT-SPECIFIC ACCESS RULE]
- [PROJECT-SPECIFIC ACCESS RULE]

## 5. Tenant Model

Remove this section when the project has no tenant boundary.

- Tenant type: [ORGANIZATION / WORKSPACE / TEAM / ACCOUNT]
- Tenant identifier: [FIELD OR CONCEPT]
- Membership model: [DESCRIPTION]
- Multiple memberships per user: [YES / NO]
- Active tenant selection: [DESCRIPTION]
- Shared resources: [NONE / DESCRIPTION]
- Global roles: [NONE / ROLES]

Tenant-scoped resources:

- [RESOURCE]
- [RESOURCE]

Rules:

- Derive tenant access from trusted membership or resource relationships.
- A client-supplied tenant ID is not proof of membership.
- Scope data, files, search, exports, analytics, jobs, integrations, caches, and realtime to the effective tenant.
- Validate invitations, role changes, removals, ownership transfer, and tenant switching server-side.
- Document every intentional cross-tenant sharing rule.

## 6. Backend and Data

- Backend: [LOVABLE CLOUD / SUPABASE / OTHER]
- Database: [POSTGRESQL / OTHER]
- Authentication: [METHODS]
- Storage: [PROVIDER]

Important data:

| Object | Purpose | Scope | Sensitivity |
| --- | --- | --- | --- |
| [OBJECT] | [PURPOSE] | [USER / TENANT / GLOBAL] | [PUBLIC / INTERNAL / CONFIDENTIAL / SENSITIVE] |
| [OBJECT] | [PURPOSE] | [SCOPE] | [CLASSIFICATION] |

Rules:

- Use versioned migrations for schema changes.
- Preserve existing data unless deletion is explicit and reviewed.
- Use database constraints for enforceable integrity.
- Use RLS for exposed private data in Lovable Cloud or Supabase.
- Review read and write authorization separately.
- Keep privileged operations server-side.
- Do not trust client-provided owner, tenant, role, price, status, or permission values.
- Do not perform destructive changes without dependency and data analysis.

Retention and deletion:

- [RULE]
- [RULE]

## 7. Account Lifecycle

Account states:

- [INVITED / ACTIVE / SUSPENDED / DEACTIVATED / DELETED]

Rules:

- [SIGN-UP OR INVITATION RULE]
- [SESSION OR VERIFICATION RULE]
- [ROLE-CHANGE RULE]
- [DEACTIVATION RULE]
- [DELETION RULE]
- [OWNED-DATA RULE]

An authentication invitation does not automatically grant application membership or role authorization.

## 8. Backend Logic and Integrations

Trusted operations:

- [EDGE FUNCTION / SERVER ROUTE / RPC / JOB / WEBHOOK]

Approved integrations:

- [PROVIDER]: [PURPOSE], scoped to [USER / TENANT / GLOBAL]

Rules:

- Keep secrets and privileged credentials server-side.
- Use the least-privileged client or role.
- Validate the authenticated actor, target resource, and effective tenant.
- Service-role clients must enforce authorization explicitly.
- Bind provider accounts, OAuth connections, credentials, and external IDs to the correct user, resource, or tenant.
- Verify webhooks using the provider's supported method.
- Make retryable processing idempotent.
- Handle duplicate, delayed, missing, and out-of-order events.
- Do not expose tokens, secrets, or sensitive provider payloads to browser code or logs.

## 9. Files, Async, Search, AI, and Exports

Remove irrelevant lines.

- Files or buckets: [DESCRIPTION]
- Jobs or queues: [DESCRIPTION]
- Realtime: [DESCRIPTION]
- Search or analytics: [DESCRIPTION]
- AI or RAG: [DESCRIPTION]
- Exports: [DESCRIPTION]

Rules:

- Authorize private files independently from UI visibility.
- Validate client-supplied paths against the authenticated actor, resource, and tenant.
- Treat signed URLs as temporary bearer credentials.
- Jobs must use a server-validated resource and tenant context.
- Search, analytics, AI retrieval, and exports must enforce the same access boundary as source data.
- Do not retrieve globally and hide unauthorized results only in the frontend.
- Derived data must retain an enforceable relationship to its source.
- Authorize exports when requested and downloaded.
- Store temporary exports privately and define expiration and deletion.
- AI prompts, embeddings, logs, and provider payloads must not contain unauthorized or unnecessary sensitive data.

## 10. Billing

Remove this section when billing is not used.

- Provider: [PROVIDER]
- Billing scope: [USER / TENANT / USAGE / TRANSACTION]
- Plans: [PLANS]

Rules:

- [TRIAL RULE]
- [UPGRADE OR DOWNGRADE RULE]
- [CANCELLATION OR REFUND RULE]
- [FAILED PAYMENT RULE]
- [ENTITLEMENT RULE]

Client-supplied plan, price, discount, payment status, or entitlement values are untrusted. Paid access must come from trusted billing state.

## 11. Design and Content

- Visual style: [STYLE]
- Brand traits: [TRAITS]
- Density: [COMPACT / BALANCED / SPACIOUS]
- Theme: [LIGHT / DARK / BOTH]
- Responsive priority: [MOBILE / DESKTOP / BOTH]
- Tone: [TONE]
- Preferred terminology: [TERMS]

Rules:

- Follow the connected design system and existing shared components.
- Preserve consistent spacing, typography, colors, and interaction patterns.
- Do not create one-off components when an appropriate shared component exists.
- Make destructive actions visually distinct and require proportionate confirmation.
- Keep UI text clear, direct, and consistent.
- Do not promise features, security, compliance, or outcomes that are not implemented.
- Accessibility is required for critical flows.

## 12. Security and Privacy

Sensitive data:

- [DATA TYPE]
- [DATA TYPE]

Requirements:

- [COMPLIANCE OR POLICY REQUIREMENT]
- [PROJECT-SPECIFIC SECURITY RULE]

Rules:

- Collect only data required for the product purpose.
- Restrict access by user, role, tenant, and resource relationship.
- Never place secrets or sensitive data in client code, URLs, logs, analytics, or user-visible errors.
- Security-sensitive actions fail closed.
- Use controlled test data instead of real third-party user data.
- Do not claim compliance, encryption, backup, restore, deletion, or isolation guarantees without evidence.
- Apply retention and deletion rules across database, storage, exports, caches, logs, and external systems.

## 13. Testing and Operations

Required checks:

- [BUILD]
- [TYPE CHECK]
- [LINT]
- [TESTS]
- [BROWSER TESTS]

Critical flows to verify:

- [FLOW]
- [FLOW]

Rules:

- Test allowed and denied behavior for protected features.
- Test relevant loading, empty, validation, error, retry, and duplicate-submission states.
- Use controlled accounts and fixtures.
- Verify direct routes and SSR behavior when applicable.
- Inspect relevant console and network failures.
- Do not mark untested behavior as verified.
- Use pagination or limits for growing datasets.
- Use timeouts and bounded retries for external calls.
- Make important failures observable without logging protected data.

## 14. Prohibited Patterns

Do not:

- replace the established frontend or backend stack without explicit approval
- create parallel auth, database, storage, or permission systems without a clear requirement
- use client state as authorization
- expose secrets or privileged credentials
- weaken RLS, validation, constraints, or permissions to make a feature work
- perform destructive changes without impact analysis
- introduce broad rewrites when a focused change is sufficient
- claim testing, deployment, security, or production readiness without evidence
- leave mocks, bypasses, debug code, or test behavior enabled in production

## 15. Decision Priority

When requirements conflict:

1. Explicit instruction for the current task
2. Project Knowledge
3. Connected design-system rules
4. Established project architecture
5. Workspace Knowledge
6. General preference

Do not silently resolve material conflicts. Preserve the safer or less destructive behavior until clarified.

## 16. Completion Standard

Before declaring work complete:

1. Confirm the requested outcome.
2. Confirm affected existing behavior.
3. Verify relevant business rules and permissions.
4. Run or describe applicable checks.
5. Review security, data, compatibility, design, and operational impact.
6. Remove temporary code and debugging output.
7. State unverified areas, assumptions, and residual risks.
