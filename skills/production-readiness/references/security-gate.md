# Security Gate

Assess security at a production-readiness level. Perform core checks yourself when no specialized review is available; do not defer.

## Inputs to consider
- Current Lovable security findings and their age.
- Open Critical / High findings and whether they were resolved, accepted, or left open.
- RLS and access control on user-facing tables, views, RPCs, storage buckets.
- Which tables/views/RPCs/storage paths are actually reachable by `anon` or unauthenticated clients.
- Privileged keys (service role, provider secret keys): storage location and reachability.
- Client vs backend secret split — nothing sensitive in the client bundle or public API routes.
- Edge functions / public API routes: authentication, signature verification, input validation, safe response bodies.
- Auth/session behavior: sign-in, sign-out, token refresh, session revocation, MFA if in scope.
- Known dependency risks that are actually reachable in production paths.
- Sensitive data in logs, error messages, or client-visible responses.
- Abuse of critical actions: rate limiting, ownership checks, idempotency.

## Handling existing scan results
Classify each existing finding as `Resolved`, `Accepted`, `Open`, or `Not verified`. Confirm the scan corresponds to the reviewed version — a stale scan is not evidence for the current build.

## Default severity → decision mapping
- Open Critical finding: normally **NO-GO**.
- Open High finding: normally **NO-GO** unless there is a narrowly scoped, documented, owned exception with a compensating control.
- Medium/Low findings: usually `Required before launch` or `Post-launch improvement`, depending on risk profile.

## Always check on your own if not covered elsewhere
- Any table/view without RLS that is exposed via the Data API.
- Any endpoint that mutates data without verifying the caller.
- Any webhook endpoint that trusts request bodies without signature verification.
- Any use of a service-role/secret key from a code path reachable from the client or from an unauthenticated public route.

Do not claim a full security audit was performed unless it actually was. State what was verified and what was not.

## Coverage limits
Mark Security as `Partially reviewed` or `Not verifiable` when the schema, grants, RLS policies, edge-function implementation, secret configuration, or reachable production endpoints are unavailable. A clean static code review is not equivalent to verification of deployed authorization and secret configuration.
