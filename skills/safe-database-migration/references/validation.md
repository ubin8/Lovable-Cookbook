# Validation

## Preflight (before migration)

Concrete checks with explicit stop criteria. Examples:

- schema matches expected source state
- target migration not yet applied
- data profile: NULLs, duplicates, orphans, invalid values
- dependency inventory current
- critical query baseline captured
- RLS baseline captured
- row counts (and optional checksums / aggregates)
- active sessions and locks acceptable
- backup / recovery confirmed, if relevant

Each check must define a stop condition, e.g. *"Stop if any duplicate values remain before adding the unique constraint."*

## Guarded statements are not source-state validation

Do not use `IF EXISTS` or `IF NOT EXISTS` as a substitute for source-state
validation. Before applying a guarded statement, verify that an existing
object—if present—has the expected type, ownership, definition,
constraints, and privileges. Fail on unexpected drift instead of
silently skipping the step.

## Post-migration

Concrete checks across:

- **Schema** — target structure present; old structure only removed if planned; indexes valid (no INVALID from failed concurrent builds); constraints validated
- **Data** — backfill complete; no unexpected NULLs; no new duplicates; FKs valid; row counts plausible; checksums/aggregates plausible
- **Application** — critical queries work; RPCs work; edge functions work; realtime and storage work if affected
- **Security** — RLS and grants behave as intended for each role
- **Operational** — no unexpected lock or performance regressions; error logs clean of new relevant errors

## Post-change statistics and plans

After large data changes, assess whether statistics need refreshing and
whether representative critical queries still use acceptable plans.
Where identity or sequence-backed keys were imported or rewritten,
verify that the sequence remains ahead of the maximum stored value.
Not every migration needs `ANALYZE`; the check is adaptive.
