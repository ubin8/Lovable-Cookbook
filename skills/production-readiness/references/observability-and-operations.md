# Observability and Operations

Post-launch you must be able to detect and respond to problems. Scale expectations to the project.

## Check
- Access to relevant logs (frontend errors, server functions, edge functions, auth events, integration callbacks).
- Client-side error capture is wired and reports meaningful context (route, user context if permitted).
- Server-side / edge-function errors are captured and inspectable.
- Auth failures and integration failures (payments, email, webhooks) are visible.
- Monitoring of critical business actions (signups, payments, key writes) — either automatic or a defined manual check.
- Product analytics / events, if the launch decision or post-launch metrics depend on them.
- Alerting for critical failure classes.
- Cost / usage visibility for metered services (AI, email, storage, DB).
- Ownership: who watches the launch, who is on call.
- Support path for users reporting problems.
- Incident contact and communication channel.
- Known bugs list.
- Manual control processes to compensate for missing automation.
- A runbook or at minimum a documented response path.

## Sizing
The project needs a proportionate way to detect and respond to failures that could materially affect its users, data, payments, or core purpose. Small MVPs and internal tools do not need enterprise monitoring.

Complete absence of a realistic detection and response path is a launch blocker when critical failures could otherwise remain unnoticed or cause meaningful harm. For low-risk static projects (e.g. a portfolio or simple marketing page), a reachable owner contact plus the ability to unpublish or update the site may be sufficient.


## Evidence rules
Distinguish `automatic monitoring`, `manual monitoring`, and `no monitoring`. Do not assume Lovable/host defaults cover a given failure mode without checking.
