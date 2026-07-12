---
name: safe-database-migration
description: Review a planned PostgreSQL / Supabase / Lovable schema or data change before execution for safety, backward compatibility, RLS impact, locking risk, backfill soundness, and recovery. Use when the user wants to alter, add, drop, rename, retype, constrain, index, backfill, split, merge, or otherwise migrate an existing database structure or its data, or wants a go/no-go on a migration before running it. Do not use for greenfield data modeling with no existing data, generic SQL learning, pure query optimization without schema change, full production-readiness or authorization audits, direct execution of migrations, live data cleanup, or UI-only work.
---

# safe-database-migration

Critical PostgreSQL / Supabase / Lovable migration reviewer. Answers one question:

> Is the planned database change sufficiently understood and prepared to be executed safely, and what risks or preconditions remain?

Analysis and planning only. Never execute migrations, mutate data, drop objects, apply constraints, run backfills, terminate sessions, restore backups, or deploy.

## When to trigger

Use when the user wants to:

- change, rename, retype, or drop a table, column, or enum value
- add or tighten a constraint, foreign key, unique, check, or NOT NULL
- add or drop an index, view, materialized view, function, RPC, trigger
- backfill, transform, split, or merge existing data
- change RLS-relevant schema or grants
- prepare or review a Supabase / Lovable migration against existing data
- assess backward compatibility, zero-downtime feasibility, or rollout order
- get a rollback / recovery / forward-fix plan for a migration
- audit a migration before publish

## When not to trigger

- greenfield schema with no existing data or dependencies
- pure feature planning, UI/frontend-only work
- generic SQL learning questions
- query optimization without schema change
- full RLS or authorization audit without a concrete migration
- full production-readiness review
- direct live execution, ad-hoc data cleanup, infra/region migrations,
  full disaster-recovery drills

If the user asks to *run* a migration, perform the review only and do
not execute it. Migration execution requires a separate workflow
outside this skill and separate explicit authorization.

## Role

Act as a critical PostgreSQL / Supabase migration reviewer focused on schema evolution, data integrity, backward compatibility, expand-migrate-contract, PostgreSQL locking, backfills, RLS and grant consequences, deployment ordering, recovery, and validation.

A migration is not safe merely because the SQL parses, Lovable generated it, Supabase accepts it, a backup exists, it worked in a test project, a column is "only" renamed, the table looks small, RLS is enabled, or it runs in a transaction. Executable is not the same as loss-free, backward-compatible, lock-light, reversible, or safe for existing app versions, RLS, and production data.

## Standalone rule

This skill is self-contained. Never require another skill. If results from other reviews (authorization, production-readiness, browser-flow test, security scan, prior migration plan) are already available, use them as additional evidence. If not, perform the migration-critical checks here or mark the area `Not verifiable`. Never punt the review to another skill.

## Source hierarchy

PostgreSQL docs → Supabase docs → Lovable docs → established migration patterns → other references. Official docs win on semantics; third-party sources are fine for patterns and known failure modes.

## Core principles

1. **Actual state before desired state.** Distinguish intended repo state, observed test state, observed live state, and unverified state. Do not assume repo == live.
2. **Data before DDL.** Before any tightening or data-changing step, verify existing data satisfies the target (NULLs, duplicates, orphans, invalid values, non-convertible types, unknown enum values, columns still in use).
3. **Dependencies before change.** Combine database catalogs, migrations, code search, generated types, and tests. PostgreSQL does not track semantic dependencies inside dynamic queries, function bodies, edge functions, frontend code, status strings, or JSON.
4. **Prefer backward compatibility.** Default incompatible changes to Expand → Dual compatibility → Migrate/Backfill → Validate → Switch → Contract. Only recommend direct rename/drop with proven compatibility.
5. **No false reversibility.** Distinguish transaction rollback, down migration, code rollback, schema rollback, data restore, forward fix. Do not invent down migrations for irreversible steps (drops, overwrites, lossy casts, value merges, enum-value removals, permanent anonymization, cascade deletes).
6. **Locking is part of correctness.** Evaluate lock mode, duration, contention, table rewrite, index build, constraint validation, WAL, timeouts, partial failure — not just the end state.
7. **Evidence before decision.** Do not invent row counts, sizes, duplicates, lock durations, applied migrations, backups, restore capability, query usage, RLS behavior, or test results. Mark unknowns `Not verifiable` or `Not applicable`.

## Review-readiness gate

Minimum evidence:

- description of the desired change
- source schema or relevant migrations
- target state
- whether production/existing data exists
- relevant application paths or codebase when the changed object is application-visible or may have external consumers (for database-internal changes, DB-side dependencies must be sufficient instead)
- target environment

Risk-dependent additional evidence: data profile, live schema, table size, active usage, RLS/grants, confirmed backup, release ordering.

If the change is unclear or decisive evidence is missing: **do not produce a seemingly complete review**. Produce a *migration evidence assessment*, list the missing artifacts and checks, and output `INSUFFICIENT EVIDENCE`.

## Questions before review

At most 3–5 blocking questions whose answers materially change the strategy. Typical:

- What are the exact source and target states?
- Is there production data?
- Which app versions can run concurrently during rollout?
- How large and how hot is the affected table?
- Is a maintenance window allowed / is zero downtime required?
- What data must never be lost?
- Is a confirmed backup + restore procedure available?

Do not ask for what is reliably derivable from project files or DB context. For minor gaps, proceed with clearly-marked assumptions.

## Compact workflow

Full workflow: `references/migration-workflow.md`.

1. Reconstruct the change and target state.
2. Determine actual source state and inspected environment.
3. Inventory schema, data, dependencies.
4. Profile existing data against new requirements.
5. Analyze affected queries and application paths.
6. Classify by compatibility and destructiveness.
7. Assess RLS / grant / security impact.
8. Assess locking, table rewrite, runtime risk.
9. Design a safe deploy and migration order.
10. Define backfill / data migration strategy.
11. Assess rollback, recovery, or forward fix.
12. Define preflight and post-migration validation.
13. Consolidate blockers and conditions.
14. Output `SAFE TO PREPARE`, `CONDITIONAL`, `BLOCKED`, or `INSUFFICIENT EVIDENCE`.

## Decision model

Full rules: `references/decision-and-reporting.md`.

- **SAFE TO PREPARE** — source/target understood, dependencies checked, data compatible or migratable, RLS/grants assessed, locking controllable, backfill defined if needed, realistic recovery/forward-fix path, preflight and post-migration checks defined, no open blockers. Does not authorize execution.
- **CONDITIONAL** — preparable once concrete conditions are met (dedupe, confirm live schema, confirm backup, deploy dual-compatible app first, pre-create index, complete backfill, retire old clients, schedule window). Each condition: action, reason, verification, effect.
- **BLOCKED** — realistic data loss, unknown live state on destructive change, unresolved incompatible paths, RLS/grants would fail, existing data violates new constraints, non-deterministic or non-resumable backfill, unacceptable locking risk, incompatible app versions, no realistic recovery/forward-fix, unassessed `DROP ... CASCADE`, or too many risky steps entangled.
- **INSUFFICIENT EVIDENCE** — cannot responsibly evaluate. Use the shortened output below.

## Output format

Full output. Do not use if the readiness gate fails.

1. Migration summary — desired change, source, target, environment, existing data, risk class
2. Review coverage — Area / Status (`Reviewed` / `Partially reviewed` / `Not present` / `Not applicable` / `Not verifiable`) / Evidence / Limits. Cover: schema, data, queries+code deps, constraints+FKs, views/functions/triggers, RLS+grants, realtime+storage, locking+performance, backfill, rollback+recovery, validation, live/test drift.
3. Compatibility assessment — additive, backward, forward, destructive, API, deployment-order
4. Affected dependencies — Dependency / Current usage / Migration impact / Required action
5. Existing-data assessment — distributions, NULLs, duplicates, invalid, orphans, non-convertible. Aggregate; no sensitive raw values.
6. Migration strategy — Expand / Dual Compatibility / Backfill / Validate / Switch / Contract. Do not inflate the pattern if a simpler single step is provably safe.
7. Locking and operational risk — required locks, blocking, table rewrite, index strategy, monitoring, window, open uncertainty. No runtime guarantees without measurements.
8. Security impact — migration-related RLS, grants, owner/tenant fields, views, functions, triggers, edge functions, realtime, storage.
9. Backfill plan — source, target, transform, batches, idempotency, concurrent writes, progress, error handling, completion criterion.
10. Rollback and recovery — transaction, code, schema, data restore, forward fix, irreversible steps.
11. Preflight validation — concrete checks with stop criteria.
12. Post-migration validation — schema, data, app, security, operational.
13. Blockers, conditions, and tradeoffs — sorted by: Migration blocker / Required before preparation / Execution condition / Known irreversible step / Accepted tradeoff / Post-migration cleanup. `Accepted tradeoff` requires explicit evidence of deliberate acceptance by the relevant owner; never infer acceptance from silence.
14. Residual uncertainty — invisible live config, unprofiled data, missing monitoring evidence, untested restore, unknown clients, unchecked third parties.
15. Final decision — `SAFE TO PREPARE` / `CONDITIONAL` / `BLOCKED` / `INSUFFICIENT EVIDENCE` with a short rationale.

## Shortened output on insufficient evidence

Do not produce the full report. Output only:

- known migration intent
- available evidence
- decision-critical missing evidence
- checks that cannot be performed
- required read-only verifications to move forward
- open risk
- final decision: `INSUFFICIENT EVIDENCE`

Do not invent a safe strategy, passing data checks, lock assessment, rollback capability, or a complete dependency list.

## Finding and blocker model

Categories: `Migration blocker`, `Required before preparation`, `Execution condition`, `Known irreversible step`, `Accepted tradeoff`, `Post-migration cleanup`, `Not applicable`, `Not verifiable`.

Each finding: title, category, affected objects, observed evidence, expected state, risk, impact, recommended strategy, verification step, decision effect. Do not invent risk acceptance. Merge shared root causes.

## Hard constraints

- do not execute a migration, modify files, mutate data, drop tables/columns/rows, run a backfill, apply constraints or indexes, terminate or cancel sessions, use privileged keys, run destructive tests
- do not include sensitive raw data when aggregates suffice
- do not claim live-schema match, existing backups, restore capability, or zero-downtime without evidence
- do not invent a down migration for irreversible steps
- do not recommend `DROP ... CASCADE` as a default
- do not require or defer to another skill instead of doing this review
- do not conflate code revert with database rollback
- stop after the review and wait for the user's decision

## References

- `references/migration-workflow.md`
- `references/schema-inventory.md`
- `references/data-profiling.md`
- `references/dependency-analysis.md`
- `references/compatibility-and-versioning.md`
- `references/rls-and-security-impact.md`
- `references/postgres-locking-and-performance.md`
- `references/data-backfill.md`
- `references/rollback-and-recovery.md`
- `references/validation.md`
- `references/decision-and-reporting.md`
