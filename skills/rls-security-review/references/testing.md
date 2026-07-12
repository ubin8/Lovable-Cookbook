# Verification testing

For every finding above `Informational`, derive at least one concrete test that would confirm the issue and later validate the fix.

## Actor matrix

Cover, as applicable to the project:

- anonymous
- user A
- user B (peer, same tenant)
- user C (different tenant)
- tenant member (baseline role)
- tenant admin
- global/support admin
- demoted/removed user (stale session)
- service/backend path

## Operation matrix

- SELECT own / peer / cross-tenant
- INSERT own / attributed to peer / attributed to another tenant
- UPDATE own → allowed state / own → forbidden state (ownership, tenant, role, status) / peer / cross-tenant
- DELETE own / peer / cross-tenant / membership rows
- RPC calls with own ID / peer ID / cross-tenant ID
- Storage upload / download / list / move / delete under own path / peer path / cross-tenant path
- View reads under each actor
- Edge function calls with valid / missing / forged auth

## Test forms

- **pgTAP** for policy-level assertions inside Postgres.
- **Supabase CLI database tests** (`supabase test db`) for reproducible RLS checks in CI.
- **Application-level tests** through a real Supabase JS client with different sessions — closest to production behavior; catches grant + policy + client construction interactions.
- **Negative tests are required** — asserting that forbidden operations return zero rows or an error is more valuable than asserting allowed operations succeed.
- **CI execution** — the whole suite should run on every migration change.

## Ground rules

- Do not run destructive tests against real data.
- Do not use the service role to simulate an end user or to validate ordinary RLS enforcement — that bypasses what you are trying to verify. A privileged test client may be used only for isolated fixture setup or for explicitly testing a backend/service path, and its role must be stated.
- Prefer seed data owned by test users; assert row counts and specific IDs, not just non-error.
- Cover both "returns empty" and "returns error" cases; PostgREST behavior differs by operation.
- Community test helpers may be mentioned as optional; they are not a prerequisite.
