# Edge functions, RPCs, privileged clients

Server-side is not authorized. A service_role client bypasses RLS entirely.

## Check per endpoint

- Which Supabase client is used: user-scoped (JWT-forwarded) vs service-role.
- verify_jwt setting and auth mode.
- Where tenant comes from: JWT claim, membership lookup, or request payload.
- Whether the handler re-validates membership before every privileged read/write.
- Public endpoints and webhooks (unauthenticated by design) — see async-jobs-webhooks-and-realtime.
- RPC parameter tampering (tenant_id, user_id, role passed by client).
- Function grants and SECURITY DEFINER usage; search_path pinning.
- Admin client is not the default data-API client for ordinary reads.

## Common failures

- Handler receives tenant_id in the body and queries with service-role — no membership check.
- Handler forwards the user's JWT but uses service-role for a "just this one" privileged step that touches user-scoped data.
- Edge function accepts a user_id param and impersonates that user without authorization.
- Public endpoint under /api/public/* performs writes without signature verification.
- SECURITY DEFINER function used as a "bypass RLS but check nothing" wrapper.
- Service-role reads used as the default data-API client, then relying on client-side filters.
