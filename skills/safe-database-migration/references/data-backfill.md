# Data backfill

Every backfill needs:

- clearly defined source and target
- deterministic transformation
- handling for invalid source values
- idempotency
- resumability after failure
- batching strategy
- progress measurement
- handling of concurrent writes (source of truth)
- error logging
- validation

## Batching

Specify batch key, stable ordering, batch size, commit boundaries, retry behavior, locking mode, index usage, rate limiting / pauses, progress markers, and conflict handling with concurrent writes.

A single large `UPDATE` is not automatically a safe backfill strategy.

## Backfill side effects

Determine whether the backfill triggers:

- audit or history rows,
- `updated_at` changes,
- notifications or webhooks,
- realtime events,
- queue jobs,
- billing or analytics events,
- synchronization triggers,
- cascading updates.

Do not disable triggers by default. If trigger suppression is
considered, enumerate the integrity and business logic that would also
be skipped and define an alternative validation path.

## Invalid rows

If some source rows cannot be transformed, choose explicitly among:

- quarantine table
- error list for manual triage
- explicit fallback value (only if business semantics confirm it)
- abort before contract

Do not silently drop data or apply an implicit default without confirmed semantics.

## Enum and type changes

For enums, distinguish adding a value (usually safe) from renaming, removing, or freely reordering (often not). Complex changes typically require: new type → new column → explicit conversion → backfill → app switch → later drop of old type.

For type changes, evaluate implicit/explicit casts, `USING` expressions, lossy conversion, index and constraint impact, table rewrite, dependent views/functions, and old app-version compatibility.

## Partitioning

Only deep-dive if the project uses partitioning or this change introduces it. Consider partition key, existing distribution, default partition, constraints, global vs partitioned indexes, attach/detach strategy, locking, FKs, triggers, RLS, query pruning, realtime/API exposure. Partitioning is not a "simple perf tweak"; it changes operational and migration cost significantly.
