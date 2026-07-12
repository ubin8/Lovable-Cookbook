# PostgreSQL locking and performance

Evaluate operational risk, not only the end state.

## Per operation, determine

- required lock mode
- whether reads are blocked
- whether writes are blocked
- whether the operation can wait behind existing locks
- whether the table is rewritten
- whether indexes are rebuilt
- WAL volume
- replication / backup impact
- whether a maintenance window is required

## Safer alternatives

- `CREATE INDEX CONCURRENTLY` / `DROP INDEX CONCURRENTLY` (reduces write blocking but has its own constraints; a failed concurrent build leaves an INVALID index that must be dropped)
- add constraint `NOT VALID`, then `VALIDATE CONSTRAINT` later (supported for CHECK and FK; not all constraint kinds)
- batched backfill instead of a single large UPDATE
- add a new column and dual-write instead of in-place type change
- phased deployment
- smaller transactions to bound lock and WAL exposure

## Caveats

- A transaction does not shield against all operational effects (bloat, replication lag, long-running-transaction problems).
- Large single UPDATEs create locks, WAL, bloat, and long transactions.
- Do not promise zero downtime without evidence.

## Timeouts and bounded execution

For production DDL, define whether a bounded `lock_timeout` and an
appropriate `statement_timeout` are required. A migration should not
wait indefinitely for a lock and then begin at an unpredictable time.

Document:

- timeout values only when project-specific evidence supports them,
- expected behavior when lock acquisition fails,
- whether retry is safe,
- cleanup required after partial failure,
- whether the migration runner wraps the operation in a transaction.

Do not recommend disabling timeouts globally.

## Migration serialization

Verify how concurrent migration execution is prevented. Inspect:

- migration history,
- deployment concurrency controls,
- CI/CD serialization,
- advisory-lock or migration-runner behavior,
- partially applied or failed migration records.

Do not assume that a migration filename alone prevents two runners from
modifying the database concurrently.

## Transactionality per step

For every migration step, determine whether it:

- may run inside the migration runner's transaction,
- must run outside a transaction block,
- becomes usable only after commit,
- can leave invalid or partial artifacts after failure,
- requires a separate migration file or deployment phase.

Relevant cases include `CREATE INDEX CONCURRENTLY`, `DROP INDEX
CONCURRENTLY`, certain enum changes, materialized view refreshes, and
extension or maintenance operations. A logically correct plan can still
be incompatible with the actual migration runner.

## Monitoring before/during

Where evidence and permissions allow, consider `pg_stat_activity`, `pg_locks`, `pg_blocking_pids(...)`, relevant `pg_stat_user_*`, size functions, and supported progress views (`pg_stat_progress_create_index`, `pg_stat_progress_copy`, `pg_stat_progress_analyze`, `pg_stat_progress_vacuum`, `pg_stat_progress_cluster`). Do not claim progress visibility for operations without a progress view. Treat cumulative stats as operational hints, not exact real-time truth. If monitoring is not available, mark it. Never auto-terminate sessions or cancel queries.
