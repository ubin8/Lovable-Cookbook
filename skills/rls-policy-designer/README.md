# RLS Policy Designer

Design or refactor PostgreSQL Row Level Security for Lovable and Supabase projects from an explicit authorization model before producing SQL.

## Use this skill when

Use `/rls-policy-designer` when you need to:

- design RLS for new or existing tables;
- introduce ownership-, membership-, role-, tenant-, sharing-, invitation-, or public-access rules;
- replace or simplify an existing policy system;
- produce migration-ready policy SQL;
- define an operation-level authorization matrix;
- prepare positive, negative, cross-tenant, role, storage, RPC, and service-path tests;
- document rollout, compatibility, and rollback constraints.

The skill supports two modes:

- **Design** — create a new authorization model and policy system.
- **Refactor** — reconstruct effective access, compare it with intended access, and replace or simplify policies without silently preserving insecure behavior.

## Do not use it for

- a read-only audit of current RLS and bypass surfaces;
- generic security reviews;
- frontend route guards or UI visibility;
- live migration execution;
- non-PostgreSQL authorization;
- incident response or data recovery.

Use [`/rls-security-review`](../rls-security-review/README.md) for an independent read-only audit. Use `/rls-policy-designer` when the requested outcome is a new or corrected authorization design and migration-ready SQL.

## Files

| File | Purpose |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Authoritative workflow, trigger rules, gates, modes, output contracts, and hard constraints |
| [`references/authorization-model.md`](references/authorization-model.md) | Actors, resources, tenants, ownership, membership, roles, lifecycle, and trusted relationships |
| [`references/authorization-matrix.md`](references/authorization-matrix.md) | Operation-level access decisions, negative cases, and current-versus-intended matrices |
| [`references/policy-strategy.md`](references/policy-strategy.md) | Enforcement boundaries, policy strategy, helpers, views, RPCs, and migration choices |
| [`references/canonical-policy-patterns.md`](references/canonical-policy-patterns.md) | Reusable ownership, tenant, sharing, invitation, storage, audit, and service-only patterns |
| [`references/policy-generation.md`](references/policy-generation.md) | Migration and SQL construction rules |
| [`references/policy-validation.md`](references/policy-validation.md) | Validation of policies, privileges, surfaces, sessions, migrations, and tests |
| [`references/common-mistakes.md`](references/common-mistakes.md) | Diagnostic symptoms and recurring RLS failure modes |
| [`references/performance.md`](references/performance.md) | Query plans, indexes, recursion, and operational RLS performance |
| [`references/supabase-notes.md`](references/supabase-notes.md) | Supabase roles, keys, auth helpers, Data API, storage, realtime, and Edge Functions |
| [`references/postgres-notes.md`](references/postgres-notes.md) | PostgreSQL RLS semantics, grants, policy composition, views, functions, and bypass behavior |

## Core workflow

```text
Discover evidence
→ model authorization
→ build the authorization matrix
→ resolve access-changing ambiguity
→ choose the smallest correct strategy
→ generate migration-ready SQL
→ validate the proposed change set
→ define tests
→ document rollout and rollback
```

Final SQL is blocked when tenant boundaries, ownership, membership, role, public, admin, or service access rules remain materially unresolved.

## Expected output

When authorization intent is sufficient, the skill returns:

- mode and scope;
- environment evidence;
- authorization model;
- authorization matrix;
- consistency result;
- policy strategy;
- existing-policy assessment in refactor mode;
- migration SQL and policy explanations;
- complexity and performance review;
- validation report;
- authorization test plan;
- rollout and rollback notes;
- residual uncertainty;
- a final decision of `READY FOR REVIEW`, `CONDITIONAL`, or `BLOCKED`.

When intent is insufficient, it returns an authorization-readiness report without speculative SQL.

## Limitations

The skill does not execute migrations, modify production data, deploy code, rotate secrets, create external provider resources, or issue blanket security certification. Repository state, observed database state, and deployed state must remain explicitly separated.

## Recommended Automatic use

**Off.**

The skill can ask blocking authorization questions, replace existing policy systems, and generate migration-ready SQL. It should be selected deliberately when policy design or remediation is the requested task, rather than activating for generic Supabase, database, or security work.
