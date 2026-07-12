# Migration workflow

Full sequence for the review. Analysis only — no execution.

## 1. Capture the change exactly

Document affected tables, columns, types, constraints, indexes, views, functions, triggers, RLS policies, desired semantics, source state, target state, target environment, expected deployment order.

Classify the change as one or more of: schema, data migration, permission, API contract, purely internal.

Split coupled changes into logically verifiable steps.

## 2. Schema inventory

See `schema-inventory.md`. Prefer catalog evidence over assumption. Never recommend `DROP ... CASCADE` as a default; if considered, enumerate every transitively affected object.

## 3. Profile existing data

Read-only only. See `data-profiling.md`. Never emit sensitive raw values when aggregates suffice.

Gate rules:

- `NOT NULL` only preparable if no unexpected NULLs remain
- `UNIQUE` only preparable if duplicates are understood and handled
- `FOREIGN KEY` only preparable once orphans are assessed
- type change only preparable if all values are deterministically convertible
- `CHECK` only preparable if existing data satisfies it or a cleanup plan exists

## 4. Dependency analysis

See `dependency-analysis.md`. Combine DB catalogs with code search (Supabase client calls, RPCs, generated types, validation schemas, edge functions, cron, webhooks, realtime, views, triggers, tests, exports, analytics, third-party integrations).

Classify each dependency: compatible / needs prep / needs parallel support / blocks migration / not verifiable.

## 5. Compatibility classification

See `compatibility-and-versioning.md`. Distinguish additive, potentially incompatible, destructive, data-changing, API-visible, internal.

## 6. RLS and security impact

See `rls-and-security-impact.md`. Focus on how *this* change alters access. Do not stage a full authorization audit.

## 7. Locking and runtime risk

See `postgres-locking-and-performance.md`. Consider lock mode, contention, table rewrite, index build, constraint validation, WAL, timeouts.

## 8. Deployment order

Design a safe order across schema, code, backfill, switch. For incompatible changes, default to Expand → Dual Compatibility → Migrate → Validate → Switch → Contract.

## 9. Backfill

See `data-backfill.md`. Deterministic, resumable, idempotent, bounded batches, concurrent-write aware.

## 10. Rollback / recovery / forward fix

See `rollback-and-recovery.md`. Do not conflate transaction rollback, down migration, code rollback, schema rollback, data restore, forward fix.

## 11. Validation

See `validation.md`. Define preflight checks with explicit stop criteria and post-migration checks across schema, data, app, security, operations.

## 12. Consolidate

Merge blockers and conditions. Emit the decision and the report per `decision-and-reporting.md`.
