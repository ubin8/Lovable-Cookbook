# Grants and Data API exposure

RLS filters rows; grants decide whether a role can reach the object at all. Both layers must be correct.

## Roles to check

- `anon` — unauthenticated Data API caller. Grants here are effectively public.
- `authenticated` — any signed-in user, across all tenants.
- `PUBLIC` — every role, including `anon` and `authenticated`.
- `service_role` — bypasses RLS. Grants matter only for defense in depth; the real risk is *where the key is used*.
- Custom roles, if any.

## What to inspect

- Explicit `GRANT` / `REVOKE` statements in migrations.
- Default privileges (`ALTER DEFAULT PRIVILEGES`).
- Table, view, sequence, schema, and function privileges.
- Grants to `anon` — every one is a public exposure decision.
- Grants to `authenticated` on tables whose RLS is disabled or missing policies.
- Unnecessary grants to `service_role` (usually harmless but worth flagging when noisy).
- Objects living in `public` (or another API-exposed schema) that should live in a non-exposed internal schema.
- Whether the Data API could be disabled entirely for schemas that don't need it.

## Semantics to keep straight

- A `GRANT SELECT` on an RLS-enabled table with no matching policy → still deny-by-default; row visibility remains zero.
- A `GRANT SELECT` on an RLS-disabled table → full table exposure for that role.
- Functions do **not** run under RLS by default; `EXECUTE` grants control who can call them. See `functions-and-rpc.md`.
- Views can bypass base-table RLS depending on `security_invoker`. See `views.md`.

## Public-anon exposure checklist

For every object granted to `anon`:

- Is public access an explicit product decision?
- Are the SELECT policies scoped to safe rows (e.g. `is_public = true`, `published = true`)?
- Are sensitive columns projected only through a safe view, or is the raw table exposed?
- Can `anon` enumerate IDs and then combine with a poorly-scoped RPC to escalate?

## Common failures

- Table created via SQL editor without an explicit grants block → invisible to Data API OR (worse, if a broad `GRANT` was added) fully open.
- `GRANT ALL ... TO PUBLIC` on tables or functions.
- `GRANT USAGE ON SCHEMA` to `anon` combined with default privileges producing surprise exposure.
- Auth-only tables with `GRANT SELECT TO anon` even though every policy scopes to `auth.uid()` — no data leaks *today* but widens the surface for future policy mistakes.
- Internal helper tables in `public` that should live in a private schema not published via the Data API.
