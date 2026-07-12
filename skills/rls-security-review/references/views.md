# Views

Views can silently widen access because, by default, they run with the view owner's privileges and the owner's RLS context.

## Checks

- **View owner.** In Supabase migrations, views are often owned by `postgres`. Base-table RLS for the caller then does **not** apply — the owner's access does.
- **`security_invoker`.** With `security_invoker = true`, the view uses the caller's privileges and RLS. Prefer this for any view over user-scoped tables. Absent by default on older Postgres versions.
- **`security_barrier`.** Prevents leaky operator pushdown but is **not** a substitute for `security_invoker`. A `security_barrier` view can still expose rows the caller could not read directly.
- **Grants on the view.** `SELECT` to `anon` / `authenticated` / `PUBLIC`.
- **RLS on base tables.** If base tables have no RLS, `security_invoker` alone does not help.
- **Updatable views.** Simple views may accept INSERT/UPDATE/DELETE. Check whether that path bypasses base-table policies and whether `WITH CHECK OPTION` is set.
- **Materialized views.** Snapshots of underlying data; RLS on the base table does not apply to the materialized copy. Grants on the mat-view directly govern access. Also review: owner and grants, who can `REFRESH MATERIALIZED VIEW` (and whether `CONCURRENTLY` is used), refresh mechanism and schedule, whether stale rows remain after access revocation or tenant changes, whether sensitive columns are copied into the materialized result, and whether the materialized view is exposed through REST or GraphQL.
- **Functions inside views.** A `SECURITY DEFINER` function used in the view SELECT can leak privileged data.
- **Data API / GraphQL exposure.** Any view in an exposed schema is queryable by the Data API subject to grants.

## Common failures

- View owned by `postgres` in `public` with `GRANT SELECT TO authenticated`, no `security_invoker` → all rows of the base table visible to every signed-in user regardless of the base table's RLS.
- Materialized view of a tenant-scoped table exposed to `authenticated`.
- Updatable view without `WITH CHECK OPTION` permits rows to be written into a state that is no longer visible through the view. Determine separately whether base-table RLS and the view's execution context still prevent unauthorized writes.
- View joins base table with a public lookup, and grants make the combined result reachable while the base table alone would not be.
- View used to expose a "safe" projection but includes a `SECURITY DEFINER` helper that returns unfiltered data.

## Review output

For every view in an exposed schema: owner, `security_invoker`, `security_barrier`, grants, base-table RLS status, whether it is updatable, and whether the effective visible/writable set matches the authorization matrix.
