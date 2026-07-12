# Multi-tenant isolation

Tenant isolation is the highest-blast-radius area of an RLS review. One bug here typically leaks across every table that shares the same broken derivation.

## Checks

- **Tenant column** on every tenant-scoped table (`tenant_id`, `organization_id`, `workspace_id`). NOT NULL, foreign-keyed to the tenant table.
- **Tenant context derivation.** Tenants must be derived from trusted authentication — a membership table joined on `auth.uid()`, or a JWT claim populated by an auth hook that reads a trusted source. Never from a client-supplied tenant ID on the request.
- **Membership table** itself protected by RLS: users cannot insert themselves into arbitrary tenants, cannot elevate their role within a tenant, cannot delete other members.
- **Admin scope.** Tenant admins administrate their own tenant only. Global admin exceptions are explicit, narrow, and rarely reachable from the client.
- **Nested resources** inherit the same tenant. Verify child tables (comments on posts, line items on invoices) enforce their own tenant check rather than relying on the parent's policy through joins.
- **Cross-tenant leaks via joins/views/RPCs/storage/exports.** Every derived surface must re-apply the tenant predicate; RLS on the base table does not automatically follow into a view owned by `postgres`.
- **Offboarding.** When a user leaves a tenant, existing sessions may hold stale JWTs. Check whether tenant/role checks re-validate via the database or trust the JWT claim.
- **Horizontal vs vertical escalation** treated separately:
  - Horizontal: user A → data of user B in same tenant; tenant X → tenant Y.
  - Vertical: user → org admin; org admin → global admin.

## Common failures

- Tenant ID trusted from request body on INSERT (no `WITH CHECK` binding to membership).
- UPDATE policy allows changing `tenant_id` → move own row into another tenant.
- Membership table with `USING (true)` for INSERT.
- Global-admin bypass implemented as `OR is_admin(auth.uid())` where `is_admin` reads from `raw_user_meta_data`.
- View joining tenant data with a public lookup, owned by `postgres`, exposed to `authenticated`.
- Storage bucket for uploads without tenant folder prefix in the policy.
- Edge function that receives `tenant_id` in the payload and acts on it with the service key.
- Invitations table lets a user insert an invitation for another tenant.

## Review output

Explicit statement of the tenant model (single-org, multi-org, org+workspace, etc.), the trusted source of tenant identity, and per-table confirmation that both row visibility and mutation stay within that tenant.
