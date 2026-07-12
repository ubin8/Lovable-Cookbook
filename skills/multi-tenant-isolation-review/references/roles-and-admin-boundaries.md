# Roles and admin boundaries

A tenant admin acts only within their tenant. Global roles must be explicit, narrow, and auditable. Supabase dashboard roles are not app-tenant roles.

## Check

- Self-escalation (member to admin, admin to owner).
- Admin-to-owner escalation.
- Changing another user's role (within tenant, across tenants).
- Last-owner protection.
- Owner transfer flow (atomicity, parallel changes, stale ownership).
- Removing other admins; removing the last owner.
- Global admin role: how it is granted, revoked, and logged.
- Support role and impersonation: scope, expiry, break-glass.
- Cross-tenant admin actions (must be justified, scoped, and audited).
- Auditability of role changes.
- Effect of role revocation on open sessions, JWTs, realtime channels, running jobs, signed URLs.

## Common failures

- Authorization roles are stored in a location that users can modify directly or indirectly.
- A global role field is used where the role should be tenant-scoped.
- Role lookup helpers, including `SECURITY DEFINER` functions, fail to bind the role to the target tenant or expose overly broad execution rights.
- RLS policy checks role but not tenant membership on the same row.
- Admin endpoints authorize by role only, ignoring which tenant the target resource belongs to.
- Impersonation reuses the operator's session without a distinct audit trail.
- Global roles inferred from an email domain or a staff = true flag on the profile.

No single data-model choice (role column, separate `user_roles` table, membership table with `(user_id, tenant_id, role)`) is universally correct or universally unsafe. The right structure depends on whether roles are global or tenant-scoped, how they are written, and how lookups are authorized. Evaluate the actual write path, grants, and lookup binding rather than the shape of the table.
