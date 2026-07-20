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
| [`README.md`](README.md) | Human-readable purpose, usage, boundaries, expected output, and Automatic-use recommendation |
| [`SKILL.md`](SKILL.md) | Authoritative self-contained workflow, trigger rules, gates, modes, output contracts, validation requirements, and hard constraints |

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

## Useful combinations

- Use after `analyze-existing-project`, `create-acceptance-criteria`, `assess-change-impact`, or `plan-feature` when authorization intent needs to become a concrete policy design.
- Use `/safe-database-migration` for a separate migration-safety preflight when the resulting policy migration changes existing schema objects, grants, functions, indexes, or production data.
- Use `/rls-security-review` after implementation when an independent read-only audit is required.
- Use `/multi-tenant-isolation-review` when the risk extends beyond database RLS into storage, search, exports, caches, jobs, realtime, integrations, or tenant lifecycle.

## Limitations

The skill does not execute migrations, modify production data, deploy code, rotate secrets, create external provider resources, or issue blanket security certification. Repository state, observed database state, and deployed state must remain explicitly separated.

## Recommended Automatic use

**Off.**

The skill can ask blocking authorization questions, replace existing policy systems, and generate migration-ready SQL. It should be selected deliberately when policy design or remediation is the requested task, rather than activating for generic Supabase, database, or security work.
