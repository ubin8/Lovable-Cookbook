# Dependency analysis

Combine database + code evidence. Catalog dependencies alone are insufficient.

## Database side

- views, materialized views, functions, triggers referencing the object
- foreign keys pointing in and out
- policies referencing affected columns
- publications, replication slots
- scheduled jobs (pg_cron)

## Application side

Search the codebase for:

- Supabase client calls: `select`, `insert`, `update`, `delete`, `upsert`
- `select('*')` vs explicit column lists
- filters, joins, order-by, group-by
- RPC calls
- generated TypeScript types
- validation and form schemas
- edge functions, cron, webhooks
- realtime subscriptions
- tests, exports, analytics, BI queries
- third-party integrations

## Semantic dependencies

Not visible to the catalog but often break:

- status strings, role values, enum values
- JSON keys
- tenant / owner fields
- default semantics
- sort order assumptions
- NULL-handling assumptions
- timezone and time semantics

## Classification

For each dependency: compatible / needs prep / needs parallel support / blocks / not verifiable. Record evidence source.
