# Production Readiness

`/production-readiness` performs an evidence-based launch-readiness review of a specific Lovable project version and returns a defensible **GO**, **CONDITIONAL GO**, **NO-GO**, or **INSUFFICIENT EVIDENCE** decision.

It evaluates more than whether the project builds. The review considers critical user flows, security, data and migrations, environment configuration, performance, resilience, observability, privacy, accessibility, rollout, rollback, and operational ownership in the context of the actual release.

> [!IMPORTANT]
> This is a review-only skill. It does not publish, deploy, edit files, run migrations, modify production data, or implement fixes.

## Use this skill when

Use `/production-readiness` when you need to answer questions such as:

- Is this project ready for production, beta, or an internal pilot?
- What is blocking the current version from going live?
- Can this specific release be approved?
- Which launch conditions must be satisfied first?
- Is the available evidence sufficient for a reliable release decision?
- Does a recently published version remain safe enough to operate?

## Do not use it for

- feature planning;
- direct implementation or deployment;
- a single isolated bug;
- RLS- or authorization-only reviews;
- SEO-only, accessibility-only, or browser-flow-only reviews;
- penetration testing;
- general design critique;
- generic launch advice without a concrete project and release target.

Use [`/rls-security-review`](../rls-security-review/README.md) when the main question is specifically whether Supabase authorization, tenant isolation, grants, RPCs, views, storage, or edge functions enforce the intended access model.

## What it reviews

The review adapts to the product, users, data, launch stage, and risk profile. Relevant areas include:

| Area | Examples |
| --- | --- |
| Release context | Target version, environment, users, launch stage, risk profile |
| Build and environment | Build evidence, deployment configuration, environment variables, secrets |
| Functional readiness | Business-critical flows, failure states, permissions, browser evidence |
| Security | Authentication, authorization, exposed surfaces, production security findings |
| Data and migrations | Schema state, migrations, drift, rollback and recovery risks |
| Performance and resilience | Bottlenecks, limits, external failures, graceful degradation |
| Operations | Logging, monitoring, alerts, ownership, incident response |
| Privacy and compliance | PII, consent, retention, legal and regulatory dependencies |
| Accessibility and content | Mobile behavior, accessibility, content quality, conditional SEO checks |
| Rollout and recovery | Smoke tests, abort criteria, rollback, stabilization path |

## Expected result

The skill produces one of four decisions:

- **GO** — no unresolved launch blocker was found within the reviewed coverage.
- **CONDITIONAL GO** — launch may proceed only after explicit, verifiable conditions are satisfied.
- **NO-GO** — one or more unresolved risks make launch unjustifiable.
- **INSUFFICIENT EVIDENCE** — the available project or release evidence is too weak for a defensible decision.

A full report includes review coverage, critical-flow status, launch blockers, required pre-launch work, accepted risks, go-live conditions, smoke-test and recovery plans, and residual uncertainty.

## Required context

Useful inputs include:

- the exact project version or commit under review;
- the target environment or domain;
- the intended launch stage;
- target users and affected data;
- business-critical launch-day flows;
- build and test results;
- production-relevant environment configuration;
- database and migration state;
- existing security findings;
- monitoring, ownership, rollout, and rollback information.

The skill must not invent missing evidence. When critical information is unavailable, it reports the gap instead of presenting a false full audit.

## Files

| File | Purpose |
| --- | --- |
| [`SKILL.md`](SKILL.md) | Authoritative trigger rules, workflow, output contract, decision logic, and hard constraints |
| [`references/readiness-workflow.md`](references/readiness-workflow.md) | Full review sequence and evidence-handling workflow |
| [`references/release-context.md`](references/release-context.md) | Product, launch-stage, user, data, and risk profiling |
| [`references/build-and-environment.md`](references/build-and-environment.md) | Build, deployment, environment, configuration, and secret checks |
| [`references/functional-readiness.md`](references/functional-readiness.md) | Critical flows, states, and observable functional evidence |
| [`references/security-gate.md`](references/security-gate.md) | Production-level security review and blocker handling |
| [`references/data-and-migrations.md`](references/data-and-migrations.md) | Schema, migrations, drift, data safety, and recovery |
| [`references/performance-and-resilience.md`](references/performance-and-resilience.md) | Performance risk, limits, resilience, and degradation |
| [`references/observability-and-operations.md`](references/observability-and-operations.md) | Logs, monitoring, alerts, ownership, and incident readiness |
| [`references/privacy-and-compliance.md`](references/privacy-and-compliance.md) | Privacy, PII, consent, retention, and compliance dependencies |
| [`references/accessibility-and-content.md`](references/accessibility-and-content.md) | Accessibility, mobile, content, and context-dependent SEO checks |
| [`references/rollout-and-rollback.md`](references/rollout-and-rollback.md) | Launch sequence, smoke tests, abort criteria, rollback, and recovery |
| [`references/decision-and-reporting.md`](references/decision-and-reporting.md) | GO/NO-GO rules, severity model, and report structure |

## Installation

Copy the complete folder without changing its internal paths:

```text
production-readiness/
├── README.md
├── SKILL.md
└── references/
    ├── accessibility-and-content.md
    ├── build-and-environment.md
    ├── data-and-migrations.md
    ├── decision-and-reporting.md
    ├── functional-readiness.md
    ├── observability-and-operations.md
    ├── performance-and-resilience.md
    ├── privacy-and-compliance.md
    ├── readiness-workflow.md
    ├── release-context.md
    ├── rollout-and-rollback.md
    └── security-gate.md
```

Do not copy only `SKILL.md`. The reference modules are part of the skill and contain required review and reporting rules.

## Limitations

- A readiness review is only as reliable as the evidence available for the exact version and environment.
- A green build or successful preview does not prove operational, data, security, or recovery readiness.
- The review cannot guarantee a risk-free launch.
- Missing live tests, load tests, restore tests, third-party evidence, or production configuration must remain explicit uncertainty.
- The skill does not replace legal, regulatory, penetration-testing, or organization-specific approval where those are required.

## Related resources

- [`/feature-planner`](../feature-planner/README.md) — plan significant product changes before implementation.
- [`/rls-security-review`](../rls-security-review/README.md) — perform a deeper Supabase authorization and tenant-isolation review.
- [Pre-launch checklist](../../checklists/pre-launch.md) — compact release quality gate.
- [How to use this cookbook](../../guides/how-to-use-this-cookbook.md) — guidance for selecting and applying cookbook artifacts.
