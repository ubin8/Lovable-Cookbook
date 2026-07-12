# Readiness Workflow

Run these steps in order. Stop early with `INSUFFICIENT EVIDENCE` if the readiness gate fails.

## 1. Establish launch context and risk profile
See `release-context.md`. Determine product type, target users, launch stage, public vs internal, data criticality, business-critical actions, expected usage, integrations, payments, multi-tenant/role model, and known regulatory or contractual constraints. Classify risk as low / medium / high / especially critical and justify it. Risk profile drives depth, not the duty to check relevant risks.

## 2. Pin version and target environment
Identify what is actually being reviewed: repo state, preview state, published snapshot, target production configuration. Note branch/commit if visible, target domain/environment, separation of prod vs test, and whether the user might release the wrong version. Explicitly document any divergence between repo, preview, and production.

## 3. Readiness gate — coverage and missing evidence
Enumerate what evidence is available for each review area and what is missing. If essentials are absent, produce a readiness evidence assessment and return `INSUFFICIENT EVIDENCE` instead of a full-looking audit.

## 4. Build, deployment, and configuration
See `build-and-environment.md`. Verify successful build; no blocking runtime errors; publish config, domain, HTTPS, redirects; no dev/preview/localhost URLs, test webhooks, sandbox payment config, or demo/seed dependencies leaking to prod; correct production env vars; no secrets in client-public vars; no missing required env; no debug/dev flags; no unshipped critical changes. Distinguish confirmed config, code-inferred config, and dashboard values that are not verifiable from here.

## 5. Business-critical user flows
See `functional-readiness.md`. First identify the flows that must work on launch day. Verify happy path plus required validation, error, empty, cancel, reload, back nav, double submit, missing permission, expired session, slow/failed requests, and mobile rendering — proportional to project. Use browser testing when available. Never mark a flow passed unless it was actually executed. Status per flow: Passed / Failed / Partially verified / Not tested / Not applicable.

## 6. Security and authorization
See `security-gate.md`. Assess at production level: current Lovable security findings, open critical/high findings, age/validity of prior scan results, RLS and access control, exposed tables/views/RPCs/storage, privileged keys, client vs backend secrets, unprotected edge functions, public endpoints, auth/session flaws, known dependency risks, sensitive logs/error messages, and abuse of critical actions. Perform core checks yourself when no specialized review is available. Do not treat an existing scan as a blanket approval.

## 7. Data, schema, and migrations
See `data-and-migrations.md`. Verify data model/constraints; needed migrations; applied vs intended DB state; drift between repo and live DB; backward compatibility; effect on existing prod data; nullable/default changes; data transformations; FKs/unique constraints; test/seed data; deletion, retention, export, backup, restore; rollback implications. Distinguish code, schema, and data rollback. Do not claim backup/restore capability without evidence.

## 8. Performance and resilience
See `performance-and-resilience.md`. Look for obvious production risks: critical pages/actions respond acceptably; no request loops; no unbounded lists/queries; pagination/limits under growth; no oversize payloads; no repeated expensive calls; no blocking third-party dependencies without error handling; timeouts, retries, idempotency for critical actions; rate limits/abuse protection where relevant; upload sizes; cost risks; expected usage vs platform limits. Distinguish observed, statically inferred, and untested.

## 9. Observability and operations
See `observability-and-operations.md`. Check access to relevant logs; error capture; edge-function/auth/integration failures; monitoring of critical business actions; analytics/product events; alerting; cost/usage visibility; ownership; support path; incident contact; known bugs; manual controls; runbook or at least a clear response path. Small MVPs do not need enterprise monitoring but must have a realistic way to detect and react to critical failures.

## 10. Privacy, compliance, accessibility, and content (adaptive)
See `privacy-and-compliance.md` and `accessibility-and-content.md`. Apply only what the project actually requires given users, region, data, and public vs internal exposure. Never claim legal or regulatory sign-off. SEO is a gate only if public discoverability is actually a goal.

## 11. Rollout, smoke tests, and recovery
See `rollout-and-rollback.md`. Check launch owner, target window, release process, post-publish smoke tests, rollback/unpublish plan, data consequences of a rollback, feature flags/gradual rollout when relevant, pilot group, post-launch metrics, abort criteria, communication path, incident owner, known risks and workarounds. A rollback plan must be concrete enough to execute under time pressure.

## 12. Consolidate and decide
Merge blockers, accepted risks, and residual uncertainty. Deduplicate by root cause. Emit the output per `decision-and-reporting.md` with a clear GO / CONDITIONAL GO / NO-GO / INSUFFICIENT EVIDENCE and short justification. Stop and wait for the user.
