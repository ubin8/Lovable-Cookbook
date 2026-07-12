# Decision and Reporting

## Decision rules

### GO
All of the following:
- No open launch blockers.
- Critical flows sufficiently verified against the reviewed version.
- No unresolved Critical or High security risks in scope.
- Production configuration sufficiently confirmed.
- Data and migration risks are controlled and recoverable.
- Operations and error detection are adequate for the launch stage.
- A rollout, post-launch verification, and recovery approach proportionate to the project's risk exists and is executable.
- Remaining risks are small and transparently listed.

GO does not mean risk-free. State residual risks explicitly.

### CONDITIONAL GO
- No immediate critical blocker, but bounded risks or manual checks remain.
- Each condition is concrete: required action, ownership area, timing, and a verification criterion.
- The launch can be tied to satisfying those conditions.
- Do not invent owner names, dates, or SLAs — use ownership areas ("owner of payments integration", "release owner", "database owner") and let the user assign real names.

### NO-GO
Typical triggers:
- Build fails or core flows fail.
- Open Critical security findings; relevant open High findings without a valid, scoped exception.
- Production secrets/domains misconfigured.
- Service/secret key reachable from client or unauthenticated public route.
- Sandbox payment configuration for a real launch.
- Risky data migration without validation or recovery.
- No realistic way to detect critical failures post-launch.
- No realistic recovery path given the risk profile.
- Reviewed version does not match the version intended for release.
- Critical data or authorization logic is not adequately protected.

### INSUFFICIENT EVIDENCE
Use when essential areas cannot be assessed defensibly (no relevant project implementation, unknown target environment, no visibility into production configuration, critical flows not testable, unclear DB state, missing security-relevant implementation, unclear which version should be released). This does not claim the project is unsafe — it states that a responsible release decision is not possible from what is available.

## Finding classes
`Launch blocker` · `Required before launch` · `Accepted risk` · `Post-launch improvement` · `Not applicable` · `Not verifiable`.

Each finding: title, category, status, affected area, observed evidence, expected production state, concrete impact, recommended action, verification step, effect on the launch decision. Deduplicate by root cause; group systemic causes.

## Decision consistency

- A report with any unresolved `Launch blocker` must be `NO-GO`.
- A report with any unresolved `Required before launch` item cannot be `GO`; it must be at least `CONDITIONAL GO`.
- `GO` may contain only accepted residual risks and post-launch improvements.
- `Accepted risk` requires explicit evidence of deliberate acceptance by an authorized ownership area — never inferred.

## Output format for GO / CONDITIONAL GO / NO-GO (fixed order)

1. **Release context** — product type, launch stage, target environment, target users, risk profile, reviewed version.
2. **Review coverage** — table:

    | Area | Status | Evidence used | Limits |

    Statuses: `Reviewed`, `Partially reviewed`, `Not present`, `Not applicable`, `Not verifiable`. Minimum rows: Version & Environment; Build & Deployment; Functional Flows; Security; Data & Migrations; Performance & Resilience; Observability & Operations; Privacy & Compliance; Accessibility, Mobile & Content; Rollout & Recovery.
3. **Executive summary** — decision, top blockers, top conditions, critical residual uncertainty, overall picture.
4. **Critical flow status** — table:

    | Flow | Result | Evidence | Remaining gap |
5. **Launch blockers** — real blockers only, sorted by impact and dependency.
6. **Required before launch** — non-critical but necessary pre-launch work.
7. **Accepted risks** — only if project context actually shows deliberate acceptance.
8. **Post-launch improvements** — important but not blocking.
9. **Go-live conditions** — only for CONDITIONAL GO; each condition includes action, ownership area, timing, verification.
10. **Launch and smoke-test plan** — final pre-check, publish step, direct smoke tests, monitoring, abort criteria, recovery path. Do not execute.
11. **Residual uncertainty** — unreviewed areas, missing live evidence, external dependencies, missing load/security tests, decision limits.
12. **Final decision** — `GO` / `CONDITIONAL GO` / `NO-GO` with short justification.

## Abbreviated output for INSUFFICIENT EVIDENCE

When the readiness gate fails, do not use the full report structure above. Output only:

1. **Release context known so far**
2. **Evidence available**
3. **Missing decision-critical evidence**
4. **Checks that could not be performed**
5. **Exact verification actions required** (what the user must provide or enable to unblock a real review)
6. **Residual risk** given the current unknowns
7. **Final decision:** `INSUFFICIENT EVIDENCE`

Do not invent launch blockers, accepted risks, passed flows, go-live conditions, or a detailed smoke-test / rollback plan when the required implementation or environment evidence is unavailable. Do not populate the full-report sections with placeholders to make the output look complete.

Stop after the report. Do not publish, deploy, or change files.
