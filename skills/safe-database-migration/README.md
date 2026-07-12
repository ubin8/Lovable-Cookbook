# Safe Database Migration

`/safe-database-migration` performs a read-only safety review before an existing PostgreSQL, Supabase, or Lovable database schema or its data is changed.

It is designed for migrations that can affect existing rows, application compatibility, authorization behavior, locks, deployment order, recovery, or data integrity.

> [!IMPORTANT]
> This skill reviews and plans migrations. It never executes SQL, mutates data, applies constraints, creates indexes, runs backfills, drops objects, restores backups, or deploys changes.

## Recommended Automatic use setting

**Automatic use: On**

This skill is a strong candidate for automatic use because database migrations are often requested with dangerously little context. A request such as “rename this column,” “make this field required,” or “remove the old table” can affect existing data, RLS, running clients, locks, and rollback options.

The trigger must remain narrow:

> Use automatically when an existing database schema or production data would be changed. Review only; never execute the migration.

Do not trigger it for greenfield data modeling, generic SQL questions, query tuning without schema changes, UI-only work, full authorization audits, or general production-readiness reviews.

See [Automatic use recommendations](../../guides/automatic-use.md) for the library-wide configuration.

## Use this skill when

Use `/safe-database-migration` before changes such as:

- adding, renaming, retyping, splitting, merging, or dropping columns or tables;
- adding or tightening `NOT NULL`, unique, check, or foreign-key constraints;
- adding or dropping indexes, views, materialized views, functions, RPCs, or triggers;
- backfilling or transforming existing data;
- changing enum values;
- changing schema elements used by RLS policies, grants, realtime, storage, or application code;
- planning zero-downtime or staged migrations;
- reviewing a migration before publish or execution;
- deciding whether a migration is safe, conditional, blocked, or insufficiently evidenced.

## Do not use it for

- a new schema with no existing data or consumers;
- direct migration execution;
- live data cleanup;
- generic SQL education;
- query optimization without a schema change;
- full RLS or authorization audits without a concrete migration;
- full production-readiness reviews;
- UI-only or frontend-only changes.

Use [`/rls-security-review`](../rls-security-review/README.md) for a deep authorization audit and [`/production-readiness`](../production-readiness/README.md) for a complete launch gate.

## What it reviews

| Area | Examples |
| --- | --- |
| Source and target state | Actual current schema, intended target, environment and drift |
| Existing data | NULLs, duplicates, orphans, invalid values, conversion failures |
| Dependencies | Queries, generated types, views, functions, triggers, clients and integrations |
| Compatibility | Backward compatibility, concurrent app versions, deployment order |
| PostgreSQL operations | Locking, table rewrites, index strategy, WAL and runtime risk |
| Security impact | RLS, grants, ownership fields, tenant boundaries, storage and realtime |
| Backfills | Batching, idempotency, concurrent writes, retries and completion criteria |
| Recovery | Transaction rollback, code rollback, schema rollback, restore and forward fix |
| Validation | Preflight checks, stop criteria and post-migration verification |

## Expected result

The skill returns one of four decisions:

- **SAFE TO PREPARE** — the migration is sufficiently understood and may be prepared, but this does not authorize execution.
- **CONDITIONAL** — preparation can continue only after explicit, verifiable conditions are satisfied.
- **BLOCKED** — unresolved risks make the proposed migration unsafe to prepare or execute.
- **INSUFFICIENT EVIDENCE** — the available schema, data, dependency, environment, or recovery evidence is too weak for a defensible decision.

A full report covers review evidence, compatibility, dependencies, existing data, migration sequencing, locking, security consequences, backfill design, recovery, validation, blockers, and residual uncertainty.

## Useful context

Provide as much of the following as possible:

- the exact desired change;
- source schema or relevant migration files;
- target schema;
- target environment;
- whether existing or production data is present;
- relevant application code and generated types;
- table size and usage characteristics;
- current RLS policies and grants;
- supported concurrent application versions;
- downtime constraints;
- backup, restore, and recovery evidence.

Missing evidence must remain visible. The skill does not invent row counts, clean data, lock duration, backup availability, live-schema state, or rollback capability.

## Files

| File | Purpose |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Authoritative trigger rules, review workflow, output contract, decisions, and constraints |
| [`references/migration-workflow.md`](references/migration-workflow.md) | Complete review sequence |
| [`references/schema-inventory.md`](references/schema-inventory.md) | Current schema and environment inventory |
| [`references/data-profiling.md`](references/data-profiling.md) | Existing-data checks before DDL or backfills |
| [`references/dependency-analysis.md`](references/dependency-analysis.md) | Database and application dependency discovery |
| [`references/compatibility-and-versioning.md`](references/compatibility-and-versioning.md) | Backward compatibility and deployment ordering |
| [`references/rls-and-security-impact.md`](references/rls-and-security-impact.md) | Migration-specific authorization consequences |
| [`references/postgres-locking-and-performance.md`](references/postgres-locking-and-performance.md) | Locks, rewrites, index operations, WAL, and runtime risk |
| [`references/data-backfill.md`](references/data-backfill.md) | Safe and resumable data migration design |
| [`references/rollback-and-recovery.md`](references/rollback-and-recovery.md) | Rollback, restore, recovery, and forward-fix analysis |
| [`references/validation.md`](references/validation.md) | Preflight and post-migration verification |
| [`references/decision-and-reporting.md`](references/decision-and-reporting.md) | Decision model and report structure |

## Installation

Copy the complete folder and preserve its internal paths:

```text
safe-database-migration/
├── README.md
├── SKILL.md
└── references/
    ├── compatibility-and-versioning.md
    ├── data-backfill.md
    ├── data-profiling.md
    ├── decision-and-reporting.md
    ├── dependency-analysis.md
    ├── migration-workflow.md
    ├── postgres-locking-and-performance.md
    ├── rls-and-security-impact.md
    ├── rollback-and-recovery.md
    ├── schema-inventory.md
    └── validation.md
```

Do not copy only `SKILL.md`. The reference modules contain required review and reporting rules.

## Limitations

- The review is only as reliable as the evidence for the actual environment and existing data.
- SQL that parses or succeeds in a test project is not proof of production safety.
- A backup does not prove restore capability unless the restore path is verified.
- Runtime and lock-duration guarantees require real measurements.
- The skill does not replace organization-specific approvals, controlled execution, monitoring, or recovery ownership.

## Related resources

- [`/feature-planner`](../feature-planner/README.md) — plan product changes before implementation.
- [`/rls-security-review`](../rls-security-review/README.md) — deep Supabase authorization and tenant-isolation review.
- [`/production-readiness`](../production-readiness/README.md) — complete evidence-based launch gate.
- [How to use this cookbook](../../guides/how-to-use-this-cookbook.md) — select and apply the right artifact.
