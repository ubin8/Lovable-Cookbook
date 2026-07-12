# Functions and RPCs

Functions are a common RLS bypass surface. They do not inherit row-level security automatically, and `SECURITY DEFINER` can silently run as a privileged owner.

## Per-function checks

- **`SECURITY INVOKER` vs `SECURITY DEFINER`.** Definer runs with the owner's privileges and bypasses the caller's RLS for objects the owner can access. Every definer function needs a documented reason and tight input validation.
- **Owner.** Who owns the function? A definer function owned by a superuser or `postgres` role effectively grants that role's access to any caller with EXECUTE.
- **`search_path`.** Definer functions without a pinned `search_path` (`SET search_path = pg_catalog, public` or fully-qualified names) are vulnerable to search-path hijacking.
- **Fully qualified object names** inside the body (`public.table`, not `table`).
- **EXECUTE grants.** Who can call it? Default privileges may grant EXECUTE to `PUBLIC`.
- **`REVOKE EXECUTE ... FROM PUBLIC`** applied where appropriate.
- **Dynamic SQL** — parameter interpolation, injection risk.
- **Unchecked parameters** — actor claims like `user_id`, `tenant_id`, `role` passed in and trusted.
- **Access to privileged tables** — role tables, billing, audit logs.
- **Auth/tenant context inside the function** — does it derive identity from `auth.uid()` / JWT claims, or trust arguments?
- **Helper functions used inside policies** — check for recursion, side effects, and whether they leak information via error messages or timing.
- **Side effects** — writes, deletes, notifications, webhook triggers.

## Common failures

- `SECURITY DEFINER` function with `GRANT EXECUTE ... TO PUBLIC` (or the default) that performs privileged writes based on caller-supplied IDs → full IDOR / privilege escalation.
- Definer function selects "for me" but uses a `user_id` argument instead of `auth.uid()` → any authenticated user can read anyone.
- Function used inside an RLS policy is itself `SECURITY DEFINER` and returns broader rows than the policy assumes.
- Function returns internal columns (role, tenant, admin flags) that the underlying table would have hidden via column privileges.
- Trigger function with `SECURITY DEFINER` writing to audit/role tables without checking the invoking context.
- RPC exposed on the Data API that wraps a table but skips the corresponding RLS predicate.

## Review output

For every definer function, record: owner, search_path status, EXECUTE grants, arguments trusted vs derived, tables touched, and whether the effective access exceeds what a direct table query under RLS would allow.
