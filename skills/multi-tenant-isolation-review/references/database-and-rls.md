# Database, RLS, grants, views, functions

## Tables

For every tenant-bound table:

- RLS enabled?
- SELECT / INSERT / UPDATE / DELETE policies, USING and WITH CHECK clauses.
- Membership joins vs denormalized tenant_id.
- Parent-child consistency (child row's tenant matches parent's tenant).
- Whether tenant_id is client-settable on INSERT and mutable on UPDATE.
- Permissive vs restrictive policies; interaction when multiple exist.
- Grants to anon, authenticated, PUBLIC, service_role.
- Default privileges on the public schema.

RLS enabled without any policy equals deny by default. RLS disabled with broad grants equals wide open — distinct failure modes.

## Especially check

- User can set tenant_id on INSERT to another tenant.
- User can move a resource to another tenant via UPDATE.
- Parent and child rows belong to different tenants.
- The membership table itself is directly manipulable.
- A permissive policy USING (true) or USING (auth.role() = 'authenticated') accidentally grants global reads.
- A function or trigger runs SECURITY DEFINER and bypasses RLS without re-checking tenant.
- A view exposes rows that would have been filtered by RLS on the underlying table.
- A materialized view stores cross-tenant results (see caching-and-derived-data).

## Views, functions, RPCs

- Views: determine whether underlying-table privileges and RLS are evaluated using the view owner or the invoking user, and verify that this execution model matches the intended tenant boundary. Consider view owner, grants, exposition, base tables, `security_invoker`, `security_barrier` where relevant, and any functions embedded in the view. Do not report absence of `security_invoker` by itself; demonstrate how ownership, grants, view definition, or reachability widens tenant access.
- Functions: SECURITY DEFINER functions must set search_path and re-validate tenant and membership. Grants to authenticated/anon on such functions define the exposed API.
- RPCs: parameters are client-controlled. A supplied tenant identifier may be used only after the function validates that the caller is authorized for that tenant and for the target resource or operation. Never treat the argument itself as proof of membership.
- Triggers: cross-table writes must preserve tenant consistency.

## Service-role paths

service_role bypasses RLS. Every privileged query must explicitly bound tenant and resource from a trusted context. See edge-functions-and-rpcs.md.
