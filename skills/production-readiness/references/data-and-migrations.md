# Data and Migrations

Focus on what could break, corrupt, or make production data unrecoverable.

## Check
- Data model: required columns, types, defaults, constraints, foreign keys, unique constraints.
- Migrations required for this release; applied vs intended DB state.
- Drift between repository migrations and the live DB.
- Backward compatibility with the previously deployed code (rolling deploys, cached clients).
- Impact on existing production data: nullable/default changes, column drops/renames, type changes, backfills.
- Data transformations and their ordering with code deploy.
- Test/seed/demo data leaking into production.
- Deletion, retention, export policies for user data.
- Backup availability and restore procedure.

## Rollback classes — never conflate
- **Code rollback** — redeploy the prior build.
- **Schema rollback** — reverse the migration (often not possible after data is written).
- **Data rollback** — restore data from a backup or compensating transaction.

A code rollback does not prove data changes can be undone. If a migration is destructive or hard to reverse, flag that explicitly.

## Risky migrations
For any risky migration, require clarity on:
- How it will be validated before/after.
- Expected duration and whether it locks tables or blocks writes.
- Which write paths are affected and how they behave during migration.
- Whether a maintenance window is needed.
- What happens on partial failure and how the system recovers.

## Evidence rules
- Do not claim backups or restore capability without proof.
- If the live database state is launch-relevant but not observable, mark it as `Not verifiable`. Add a `Required before launch` verification step when the release depends on migrations, existing production data, schema compatibility, or deployed database configuration.
- If no production database exists or the project does not use one, mark the area as `Not present` or `Not applicable`.
