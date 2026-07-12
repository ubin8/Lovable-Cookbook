# Multi-Tenant Isolation Review

`/multi-tenant-isolation-review` performs a deep, evidence-based review of tenant isolation in a multi-tenant Lovable and Supabase SaaS application.

It checks whether one organization, workspace, team, or customer account can reach, influence, or expose another tenant's data, resources, actions, files, jobs, exports, analytics, or administrative paths.

> [!IMPORTANT]
> This is a review-only skill. It never modifies files, policies, grants, data, sessions, memberships, invitations, jobs, webhooks, exports, or production configuration.

## Automatic use

**Recommended setting: Off**

This skill starts a broad, formal tenant-isolation audit. It may reconstruct the tenant model, request multiple test identities, inspect many application surfaces, and return `ISOLATION FAILED` or `INSUFFICIENT EVIDENCE`.

Use it deliberately when a complete cross-tenant review is intended. Do not let it replace normal feature implementation, policy changes, or focused Supabase work.

## Use this skill when

Use `/multi-tenant-isolation-review` when you need to determine whether:

- users can access another organization's records through direct object references;
- tenant IDs, workspace IDs, or parent-resource IDs can be tampered with;
- administrators in one tenant can influence another tenant;
- memberships, invitations, role changes, or tenant switching cross boundaries;
- RLS, grants, views, RPCs, or edge functions enforce the intended tenant model;
- storage paths, signed URLs, exports, search, RAG, analytics, or caches leak tenant data;
- queues, jobs, webhooks, cron tasks, or realtime channels preserve trusted tenant context;
- member removal, ownership transfer, suspension, offboarding, or tenant deletion revoke access correctly.

## Do not use it for

- single-tenant applications;
- generic RLS questions without a multi-tenant model;
- direct implementation or remediation;
- feature planning;
- general application-security reviews such as XSS, CSRF, CSP, or dependency scanning;
- performance reviews;
- production penetration testing;
- a focused database migration preflight.

For a narrower Supabase authorization review, use [`/rls-security-review`](../rls-security-review/README.md). For migration safety, use [`/safe-database-migration`](../safe-database-migration/README.md).

## What it reviews

| Area | Examples |
| --- | --- |
| Tenant model | Tenant definition, membership model, active-tenant selection, shared resources |
| Direct access | BOLA/IDOR, object identifiers, CRUD boundaries, parent-child relationships |
| Membership lifecycle | Invitations, acceptance, role changes, removal, last owner, ownership transfer |
| Database and RLS | Policies, grants, views, functions, RPCs, tenant-bound relations |
| Privileged paths | Edge Functions, service-role clients, global and support roles |
| Storage | Bucket exposure, paths, listing, upload, move, copy, delete, signed URLs |
| Search and derived data | Search, RAG, embeddings, exports, analytics, materialized views, caches |
| Async systems | Jobs, queues, cron, webhooks, realtime, external integrations |
| Offboarding | Session staleness, tenant switching, suspension, deletion, resource transfer |
| Verification | Positive and negative cross-tenant test matrices |

## Expected result

The skill produces one of four decisions:

- **ISOLATION VERIFIED** — reviewed tenant boundaries are supported by sufficient evidence and negative tests, with low residual uncertainty.
- **CONDITIONAL** — no confirmed Critical or High boundary violation remains, but concrete verification gaps or lower-severity issues must be resolved.
- **ISOLATION FAILED** — at least one confirmed Critical or High cross-tenant violation exists.
- **INSUFFICIENT EVIDENCE** — the tenant model or decisive implementation evidence is too incomplete for a responsible classification.

A full report includes the tenant model, coverage table, executive summary, resource-access matrix, cross-tenant test matrix, findings, lifecycle assessment, async and derived-data assessment, remediation order, verification plan, and residual uncertainty.

## Required context

Useful evidence includes:

- the exact tenant concept and tenant identifier;
- roles, memberships, global roles, and intended cross-tenant exceptions;
- whether users may belong to multiple tenants;
- sensitive tenant-bound resources;
- schema, RLS policies, grants, views, functions, and RPCs;
- edge functions and privileged clients;
- storage policies and path conventions;
- search, RAG, export, analytics, cache, queue, job, webhook, and realtime logic;
- invitation, role-change, removal, ownership-transfer, and deletion flows;
- at least two controlled tenants and suitable test identities where possible.

The skill never invents missing tenants, roles, resources, tests, or evidence.

## Files

| File | Purpose |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Authoritative trigger rules, workflow, decision model, output contract, and hard constraints |
| [`references/isolation-workflow.md`](references/isolation-workflow.md) | Full review workflow |
| [`references/tenant-model.md`](references/tenant-model.md) | Tenant model reconstruction and resource classification |
| [`references/resource-access-matrix.md`](references/resource-access-matrix.md) | Intended access matrix design |
| [`references/memberships-and-invitations.md`](references/memberships-and-invitations.md) | Membership and invitation lifecycle |
| [`references/roles-and-admin-boundaries.md`](references/roles-and-admin-boundaries.md) | Tenant and global administrative boundaries |
| [`references/database-and-rls.md`](references/database-and-rls.md) | Database, RLS, grants, views, and functions |
| [`references/storage-and-files.md`](references/storage-and-files.md) | Storage and file isolation |
| [`references/edge-functions-and-rpcs.md`](references/edge-functions-and-rpcs.md) | Privileged server paths and RPCs |
| [`references/search-rag-exports-and-analytics.md`](references/search-rag-exports-and-analytics.md) | Search, RAG, exports, analytics, and materialized views |
| [`references/async-jobs-webhooks-and-realtime.md`](references/async-jobs-webhooks-and-realtime.md) | Async and realtime tenant context |
| [`references/caching-and-derived-data.md`](references/caching-and-derived-data.md) | Cache and derived-data isolation |
| [`references/lifecycle-and-offboarding.md`](references/lifecycle-and-offboarding.md) | Revocation, switching, offboarding, and deletion |
| [`references/isolation-testing.md`](references/isolation-testing.md) | Positive and negative test matrix |
| [`references/severity-and-reporting.md`](references/severity-and-reporting.md) | Validation status, severity, and reporting rules |

## Installation

Copy the complete folder and preserve its internal paths:

```text
multi-tenant-isolation-review/
├── README.md
├── SKILL.md
└── references/
    ├── async-jobs-webhooks-and-realtime.md
    ├── caching-and-derived-data.md
    ├── database-and-rls.md
    ├── edge-functions-and-rpcs.md
    ├── isolation-testing.md
    ├── isolation-workflow.md
    ├── lifecycle-and-offboarding.md
    ├── memberships-and-invitations.md
    ├── resource-access-matrix.md
    ├── roles-and-admin-boundaries.md
    ├── search-rag-exports-and-analytics.md
    ├── severity-and-reporting.md
    ├── storage-and-files.md
    └── tenant-model.md
```

Do not copy only `SKILL.md`. The reference modules contain required review and reporting rules.

## Limitations

- The result is only as reliable as the available evidence and test coverage.
- `ISOLATION VERIFIED` does not mean the entire application is secure.
- A correct UI tenant switcher does not prove backend authorization.
- RLS alone does not cover privileged services, storage, exports, caches, jobs, or external systems.
- Missing live configuration, test identities, lifecycle evidence, or async implementation remains explicit uncertainty.
- The skill does not replace penetration testing, legal review, incident response, or organization-specific approval.

## Related resources

- [`/rls-security-review`](../rls-security-review/README.md) — focused Supabase data-authorization review.
- [`/safe-database-migration`](../safe-database-migration/README.md) — migration safety preflight.
- [`/production-readiness`](../production-readiness/README.md) — full release-readiness gate.
- [Automatic use recommendations](../../guides/automatic-use.md) — routing recommendations for all skills.
