# Schema inventory

Inventory the actual source state before evaluating the change.

## What to inspect

Where evidence is available:

- tables and partitions; column order, types, defaults, nullability
- primary keys, unique, check, foreign keys with ON DELETE / ON UPDATE
- indexes including expressions and partial indexes; validity
- sequences and identity columns
- enums and domains
- views and materialized views
- functions, RPCs, triggers (and their SECURITY DEFINER status)
- RLS enablement, policies (USING, WITH CHECK, roles)
- grants, default privileges
- realtime publications
- storage bindings
- installed extensions
- applied vs pending migrations in the repo and in the DB

## State separation

Track separately:

- intended repo state (migrations, declarative schema)
- observed test state
- observed live state (only if evidence exists)
- unverified assumptions

Drift between these is itself a finding.

## Dependency handling

Use `pg_depend` / `information_schema` where available, but do not rely on catalog dependencies alone — dynamic SQL, function bodies, edge functions, app code, JSON keys, and status strings are invisible to the catalog.

`DROP ... CASCADE` is never a default recommendation. If cascade is considered, list every object it would remove and assess each.
