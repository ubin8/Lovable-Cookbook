# Tenant model

Do not evaluate a cross-tenant rule before this is clear.

## What to capture

- Tenant term — organization, workspace, team, customer account, location, school/class, partner/reseller, seller/merchant, or another logically separated customer domain.
- Tenant identifier — column name(s), type (UUID/bigint/composite), where it lives (own table, embedded in every table, derived from parent).
- Membership — table linking users to tenants, columns, uniqueness constraints, whether one user may belong to multiple tenants.
- Roles — enum/table, per-tenant vs global, owner/admin/member split, custom roles.
- Owner model — single owner, multiple owners, transferable, last-owner protection.
- Global roles — support, staff, break-glass, impersonation; who grants them; audit trail.
- Shared resources — resources intentionally visible across tenants (marketplaces, directories, public catalogs), and by what mechanism.
- Parent-child structure — tenant → project → item chains; whether the child inherits the tenant of the parent or stores its own tenant_id.
- Active-tenant selection — how the server determines which tenant the request acts on: JWT claim, membership lookup keyed by URL/path/subdomain, header, request body, session state.
- Tenant lifecycle — creation, invitation, join, switch, leave, tenant deletion, tenant suspension.

## Resource classification

- global
- tenant-bound
- user-bound
- shared across tenants (intentionally)
- derived (aggregates, embeddings, search indexes, materialized views)
- temporary (exports, signed URLs, job payloads, queue messages)
- externally stored (Storage buckets, third-party providers)

## Never conflate

- Supabase platform organization (billing/ownership of the Supabase project)
- Lovable workspace (project management surface)
- App tenant (business-level customer/organization inside the running app)
- In-app customer organization for that app's users

A user with permissions in the Supabase dashboard is not automatically privileged in the app; conversely an app-tenant admin has no Supabase-project privileges.

## Assumptions

If evidence is missing for any of the above and cannot be derived from schema/code, either ask a blocking question (see SKILL.md) or continue with a clearly marked assumption. Never invent tenants, roles, or memberships.
