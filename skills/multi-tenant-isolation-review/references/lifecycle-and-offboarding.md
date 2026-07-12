# Tenant lifecycle and offboarding

Supabase project deletion and app-tenant deletion are different operations.

## Join

- Invitation, acceptance, role assignment, tenant binding.
- Existing membership handling; multi-tenant users; tenant switch validation server-side.
- Cache invalidation, query refetch, storage/realtime/UI state reset.

## Role change

- Immediate effect vs JWT staleness.
- Open sessions, open realtime connections, running jobs, signed URLs, in-flight exports.
- Downgrade must not leave stale elevated capabilities anywhere derived.

## Member removal

- Data-API access, storage, exports, jobs, webhooks, tokens.
- Personal resources vs jointly created content.
- Whether the removed user retains any signed URL, cached page, or realtime channel.

## Ownership transfer

- Last owner, new owner, parallel changes, old owner's residual rights, audit trail.

## Tenant deletion

- Database rows, storage objects, queue messages, running jobs, webhooks, embeddings, materialized views, analytics snapshots, exports, external provider references, retention, backups.
- Soft-delete vs hard-delete semantics; whether a rehydrated tenant reappears with stale associations.

## Common failures

- An already-issued JWT retains stale role or membership claims until the configured token expiry or an effective server-side revocation check takes effect; privileged endpoints lack such a check.
- Removed member can still fetch via a cached export URL.
- Tenant deletion removes rows but leaves storage objects, embeddings, or webhook subscriptions.
- Ownership transfer atomicity gap: two concurrent updates leave the tenant with zero owners.
