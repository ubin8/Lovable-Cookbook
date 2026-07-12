# Auth and session lifecycle

Authorization decisions depend on claims and sessions that change over time. A correct policy today can grant access based on a stale token tomorrow. Review lifecycle explicitly.

## Custom claims

- Source of truth: auth hook reading a trusted table (role/membership) vs `raw_user_meta_data` (user-editable, untrusted).
- Grants on the hook function and any tables it reads.
- Where the claim is consumed: RLS policies, `authorize()` helpers, edge functions, application code.
- Whether the claim name is namespaced to avoid collision with user-controllable metadata.

## Role changes

- When a role is granted or revoked in the database, existing JWTs still carry the old claim until refresh.
- Policies that read directly from a role/membership table pick up changes immediately; policies that read from `auth.jwt()` do not.
- Prefer database-backed checks (or a mix) for revocation-sensitive privileges.

## Token refresh window

- Determine the configured access-token lifetime. Do not assume a fixed default.
- A demoted user may retain claim-based privileges until the current access token expires or is refreshed.
- Document the resulting stale-privilege window and whether it is acceptable for the sensitivity of the operation.

## Role revocation / user removal from tenant

- Does the app remove the user from the membership table only, or also revoke the session?
- Are realtime subscriptions torn down on revocation, or does the socket keep streaming under the old auth?
- Are cached authorization decisions in edge functions invalidated?

## Account suspension / deletion

- Suspension: is the account flagged such that policies deny access even with a valid JWT?
- Deletion: cascade behavior on owned rows, membership rows, storage objects, signed URLs.
- Sessions explicitly invalidated (Auth Admin API sign-out) vs left to expire naturally.

## Session revocation

- Verify what session-revocation mechanisms the project actually uses and whether they invalidate only refresh capability or also block still-valid access tokens.
- Do not assume that revoking refresh tokens immediately invalidates every already-issued access token.
- Where high-sensitivity operations exist, design policies to re-check authorization from a trusted source rather than relying purely on JWT claims.

## Signed URL lifetime

- TTL choice: short enough that leak windows are bounded, long enough for the legitimate use case.
- Signed URLs are bearer credentials — anyone with the URL has access until expiry, regardless of the original user's session state.
- Never used as an authorization mechanism for sensitive assets; combine with private buckets + RLS.

## Realtime exposure

First determine which Realtime features are used:

- Postgres Changes
- Broadcast
- Presence
- private channels

Do not assume they share the same authorization model.

For Postgres Changes, verify the publication, subscribed tables, role context, and effective RLS behavior at the time changes are emitted.

For Broadcast, Presence, and private channels, inspect the channel authorization mechanism separately.

Test revocation, token refresh, reconnect behavior, and whether an already-open connection retains access after a role or membership change.

## Delayed permission changes

- Cached JWT claims, cached role decisions, materialized permission views, background jobs holding old sessions.
- Any place a permission is captured once and reused later is a candidate for stale-privilege bugs.
- Prefer re-evaluating authorization at each sensitive action rather than trusting a cached decision.

## Review output

State the concrete stale-privilege window per surface (REST, realtime, edge functions, signed URLs, background jobs), and whether the app has any mechanism to shrink it for high-sensitivity operations.
