# Review workflow

Execute these steps in order. If an area is confirmed absent, mark it as `not present`. If the available context is insufficient, mark it as `not verifiable` and record the missing evidence. Do not silently skip areas.

## 1. Scope and coverage

- List which data-access paths will be reviewed (client → Data API, RPC, storage, edge functions, server functions).
- Enumerate exposed schemas and relevant roles (`anon`, `authenticated`, `service_role`, custom).
- Record areas that are unavailable (no access to migrations, no auth hook code, etc.).
- Never issue an unrestricted clearance when substantial areas were not inspected.

## 2. Reconstruct the authorization model

Identify:

- **Subjects:** anonymous, authenticated users, service accounts, admin/support roles.
- **Resources:** tables, rows, files, views, RPCs.
- **Relationships:** owner, member, admin, tenant/organization.
- **Operations:** read/create/update/delete + special actions (invite, role change, export, moderation).

Produce a compact target access matrix (see `authorization-matrix.md`). Mark every cell as evidence-based or assumption.

## 3. Attack-surface inventory

- Exposed tables (public/API schemas).
- Views and materialized views.
- Functions/RPCs callable via PostgREST.
- Storage buckets (public vs private) and `storage.objects` policies.
- Edge functions (auth mode, service client usage).
- Direct client access vs server-only paths.
- Admin/service clients and where they run.
- Grants to `anon`, `authenticated`, `PUBLIC`.

## 4. RLS activation

- Every exposed & sensitive table has `ENABLE ROW LEVEL SECURITY`.
- New tables created via ad-hoc SQL are not silently exposed without RLS.
- Table owners: RLS does not apply to owners unless `FORCE ROW LEVEL SECURITY` — check when relevant.
- No unexpected `BYPASSRLS` roles.
- Join/membership/mapping tables are protected too.
- Distinguish "RLS on, no policy" (deny-by-default) from "RLS off, grants present" (open).

## 5. Policies per operation

See `rls-policy-checks.md`.

## 6. Policy combinations

- Permissive policies combine with `OR` → widen access.
- Restrictive policies combine with `AND` → narrow access.
- `FOR ALL` policies affect every operation; verify intent.
- Policies on `PUBLIC` role apply to `anon` and `authenticated` alike.
- Policies that check only `auth.uid() is not null` are authentication, not authorization.
- Watch for recursive policy dependencies and inconsistencies between related tables.
- Evaluate the effective combined logic, not each policy in isolation.

## 7. Multi-tenant isolation

See `multitenancy.md`.

## 8. Grants and Data API exposure

See `grants-and-api-exposure.md`.

## 9. Functions and RPCs

See `functions-and-rpc.md`.

## 10. Views

See `views.md`.

## 11. Storage

See `storage.md`.

## 12. Auth, claims, and session lifecycle

See `auth-and-session-lifecycle.md`.

## 13. Edge functions

See `edge-functions.md`.

## 14. Service / secret key usage

- Service role key in frontend or shipped bundles → Critical.
- Secret keys in client code.
- Privileged Supabase clients in generally reachable server functions without extra authorization.
- Admin client used where a user-scoped client would suffice.
- Publishable / legacy anon key is not secret and is intended for the client when RLS is correct.

## 15. Column-level privileges

Check whether sensitive columns are too broadly readable/writable even when row access is correct:

- role fields, admin flags, tenant_id, payment info, internal notes, moderation/billing status, security metadata.

Prefer separate tables, safe views, or targeted APIs over complex column-level privileges. If column-level privileges are used, check table-level grants, explicit column grants, `SELECT *` effects, and insert/update/delete behavior.

## 16. Performance and availability

Performance issues are not automatically security issues. Report separately unless they enable a realistic availability attack. Check: indexes on policy columns, per-row function calls, expensive membership joins, recursive policies, scaling of tenant checks.

## 17. Verification tests

See `testing.md`.
