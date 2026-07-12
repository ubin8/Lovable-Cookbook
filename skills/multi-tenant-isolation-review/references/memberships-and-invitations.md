# Memberships and invitations

Authentication invitation is not tenant-membership authorization. A Supabase Auth invite does not by itself prove safe organization membership. The app must bind tenant and role separately.

## Memberships

- Table structure and uniqueness (user_id, tenant_id, role).
- Who may INSERT/UPDATE/DELETE membership rows — via RLS, RPC, or edge function.
- Whether a user can grant themselves membership.
- Whether tenant_id or role in a membership row is client-settable.
- Cascade behavior on user deletion, tenant deletion, role removal.
- Multi-membership: is switching atomic; does the active tenant travel via URL/subdomain/claim?

## Invitations

- Who may invite; to which tenant; with which role.
- Server-side binding of tenant, role, and recipient (never derived from client payload alone).
- Expiry, revocation, single-use vs reusable.
- Behavior when recipient already exists / already a member / changed email.
- Redirect and metadata manipulation (open redirect, role escalation via metadata).
- Race conditions and double acceptance.
- Invitation after the inviter's role was revoked.
- Whether the invitation carries a role claim that the acceptance flow trusts blindly.
- Whether accepting one tenant's invitation can grant membership in another.

## Common failures

- Invitation token encodes tenant_id/role in the client-visible payload and the accept endpoint trusts it.
- Accept endpoint uses auth.uid() but reads tenant_id from the request body.
- The acceptance flow has no authoritative server-side invitation state for tenant, role, recipient, expiry, revocation, and prior use.
- A client-visible token or metadata payload is treated as the sole source of truth.
- Reusing the same invitation URL grants multiple memberships or roles.
- Acceptance short-circuits when the user is already logged in as a different tenant's member, silently overwriting the active tenant claim.

## Identity and SSO lifecycle

Check:

- How multiple auth identities are linked to one application user.
- Whether email address or domain is incorrectly treated as proof of tenant membership.
- How an SSO connection maps issuer, organization, domain, and user to an app tenant.
- Whether linking a new provider can inherit memberships or roles from the wrong account.
- Whether unlinking, provider removal, or IdP deprovisioning removes or preserves tenant access as intended.
- Whether the same email across different providers or tenants can cause unintended account merging.
- Whether existing sessions and claims remain valid after SSO or identity changes.
