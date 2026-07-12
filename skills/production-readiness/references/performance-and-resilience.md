# Performance and Resilience

Look for realistic production risks, not micro-optimizations.

## Check
- Critical pages and actions respond acceptably under normal use.
- No obvious request loops, redundant re-fetches, or waterfall chains on critical routes.
- No unbounded queries or lists that will grow without pagination or limits.
- Payload sizes are reasonable (images, JSON responses, exports).
- No repeated expensive calls where caching or memoization is obviously appropriate.
- Third-party dependencies (payment, email, AI, external APIs) have timeouts and error handling; failure of one dependency does not crash unrelated flows.
- Retry behavior does not amplify failures (no unbounded retry storms).
- Idempotency for critical actions (payments, notifications, external writes).
- Rate limiting / abuse protection where the action can be abused (auth, contact forms, expensive AI calls, uploads).
- Upload size / MIME restrictions.
- Cost risks: unbounded AI calls, unbounded storage growth, unbounded outbound email.
- Expected usage vs known platform limits (edge function timeouts, request size, concurrency). Only use platform limits that are visible in current project configuration or verified documentation available in context. If limits cannot be confirmed, state that capacity against platform limits is `Not verifiable` rather than guessing.

## Evidence rules
Distinguish:
- **Observed** — you actually measured or watched the behavior.
- **Statically inferred** — code implies the risk; not measured.
- **Untested at scale** — nobody has load-tested this.

Do not claim general scalability without a load test. Only flag performance items as launch blockers when they meaningfully threaten core function or operational stability at the expected load.
