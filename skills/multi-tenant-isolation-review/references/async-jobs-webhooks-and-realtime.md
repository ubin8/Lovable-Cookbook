# Async jobs, queues, webhooks, cron, realtime

## Queues and background jobs

- Queue message must carry a tenant reference from a trusted source (not blindly the client's input).
- Consumer re-validates the resource against tenant on processing.
- Idempotency keys are tenant-scoped.
- Retry does not run under a different active tenant context.
- Partial processing on retry must not produce cross-tenant writes.
- Role revocation between enqueue and processing is respected where the design requires it.
- Queue name is not a tenant boundary.
- Dead-letter / failure handling does not expose payloads across tenants.
- Global workers with privileged DB access must bound queries by the message's trusted tenant.
- Cron jobs use trusted tenant context per iteration; a single global cron must fan out per tenant safely.

## Webhooks and external IDs

- Verify the webhook with the provider's official signature-verification method, or an equivalent implementation that validates the exact signed payload and avoids timing-sensitive comparison where applicable.
- Replay protection (timestamp/nonce), idempotency.
- Provider account / external customer ID mapped to exactly one app tenant.
- Event type routing; ordering; late/out-of-order events; events for deleted or moved resources.
- A signed event is authenticated, not automatically assigned to the right tenant.
- Globally unique external IDs must be paired with the expected provider account before trusting the mapping.
- Service-role processing must scope writes to the mapped tenant.

## Realtime

Determine which realtime features are used — Postgres Changes, Broadcast, Presence, private channels — before assuming a single authorization model.

Check:

- Topic / channel naming and whether channel names are tenant-scoped.
- JWT and membership check on subscribe. For Postgres Changes, inspect the publication, subscribed tables, subscriber JWT/role, and the effective RLS behavior for each event source.
- Reconnect and token refresh — stale JWT after role revocation.
- Tenant switching keeps or drops existing subscriptions.
- Broadcast payload contents; presence metadata.
- Distinguish whether the connection remains technically open from whether new messages or database changes continue to be authorized and delivered.
- Verify authorization on the existing connection, after token refresh, and after reconnect.

Plan or run: user in Tenant A subscribes to a Tenant B topic; member removed; role downgraded; token stays old; reconnect after tenant switch. Safe normal queries do not prove safe realtime.

## Tenant-scoped integrations and credentials

Check:

- External connection, provider account, OAuth token, webhook configuration, and secret references are bound to the correct tenant.
- A tenant cannot supply or select another tenant's `connection_id`, provider account, customer ID, repository, or credential reference.
- Privileged workers resolve credentials from a server-validated tenant/resource relationship, not from an unchecked job payload.
- Token refresh, reconnect, reauthorization, and provider-account changes preserve the tenant boundary.
- Tenant removal or deletion revokes or disconnects the relevant external integration without affecting other tenants.
- Logs, error payloads, and support tooling do not expose another tenant's provider identifiers or credentials.

## Logs, audit trails, and support tooling

Cross-tenant leaks also arise via stack traces, job dashboards, support views, audit logs, provider responses, and differing error messages that reveal object existence. Review logs, audit trails, error responses, job dashboards, and support tooling for tenant-scoped visibility and metadata leakage.
