# Rollback and recovery

Distinguish and evaluate independently:

- **Transaction rollback** — can the ongoing operation be undone within its own transaction?
- **Down migration** — can the schema step be logically reversed without data loss?
- **Code rollback** — can the previous app version keep working against the new schema?
- **Schema rollback** — can the schema be returned to the previous state?
- **Data restore** — is a confirmed backup available, and is the restore path understood or tested in the relevant environment?
- **Forward fix** — is fixing forward safer than rolling back?

For each strategy, state: preconditions, data-loss potential, expected duration only if evidenced, availability impact, verification steps, limits.

## Reality checks

- A backup alone does not prove a safe recovery path.
- Lovable / Supabase backups count as confirmed only if demonstrable in the relevant environment.
- A code revert is not a database rollback.
- Irreversible steps (drops, overwrites, lossy casts, value merges, enum removals, permanent anonymization, cascade deletes) do not have a real "down migration" — say so explicitly instead of inventing one.
