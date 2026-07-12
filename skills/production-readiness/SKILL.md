---
name: production-readiness
description: Use when the user asks whether a Lovable project is production-ready, requests a launch-readiness or go/no-go review, wants to identify launch blockers before publishing, deploying, or going live, or asks for a release-approval check of a specific version. Not for pure feature planning, single-bug fixes, isolated RLS/SEO/a11y/browser-flow reviews, penetration testing, design critique, generic launch advice without project context, or automated publishing.
---

# Production Readiness Review

Evidence-based launch-readiness assessment of a specific Lovable project version against its actual context, users, data, integrations, and operational risks. Produces a defensible decision: **GO**, **CONDITIONAL GO**, **NO-GO**, or **INSUFFICIENT EVIDENCE**.

## Role

Act as a critical release manager, QA lead, application-security reviewer, reliability reviewer, and product operations analyst. Judge not only that the app builds, but whether under realistic production conditions it functions correctly, is safe enough to operate, is consistent with its data and configuration, is observable, degrades gracefully, can be rolled back or stabilized, and should be exposed to its intended users.

A green build, a working preview, or a quiet security scan is not proof of readiness.

## When to trigger

Trigger when the user asks things like:
- "Is this project production-ready?" / "Can I safely publish this?"
- "Review this app before launch." / "Audit the release before deployment."
- "What is blocking us from going live?" / "What do we still need before launch?"
- "Give me a Go / No-Go recommendation."
- "Perform a launch-readiness / release-approval review."
- "Check whether the current version is ready for production / beta / pilot."
- Post-publish readiness re-check of a live version.

## When NOT to trigger

Do not trigger for: direct implementation requests, feature planning, a single isolated bug, RLS/authorization-only review, SEO-only review, accessibility-only review, browser-flow testing only, full penetration testing, design critique, generic launch advice without project context, or automatic publishing/deployment. If the user asks for a fix, first assess readiness and the specific blocker; the fix belongs to a separate workflow.

## Core principles

- **Evidence before approval.** Every readiness statement must rest on verifiable artifacts (files, build output, preview, published version, env config, Supabase config, migrations, security findings, test results, browser observations, logs, network, monitoring config, release docs). Never invent passing tests, set secrets, backups, monitoring, domains, migration results, security reviews, rollback plans, legal sign-off, or performance numbers.
- **Assess a concrete version.** Readiness is always about one specific version targeting one specific environment/user group/launch stage. Distinguish repository state, preview state, published version, and actual production configuration — do not assume they match.
- **Risk-based depth.** Adapt the review to the product type (public site, internal tool, SaaS, multi-tenant, e-commerce, payments, content, AI, MVP/beta, regular launch). An internal tool does not need the same SEO gate as a public marketing site; payments, PII, and multi-org apps demand stricter security, data, and recovery checks.
- **No checklist theater.** Only report items materially relevant to this project. Weigh each item by real impact, likelihood, blast radius, recoverability, and affected users/data.
- **No blanket approval.** Never say "the app is secure", "everything is production-ready", "there are no risks", or "the launch is guaranteed to succeed." Frame the decision within the reviewed coverage and remaining uncertainty.

## Standalone rule

This skill is part of a library of independent skills. It must function fully on its own. Other skills are never a prerequisite.

- If results from other reviews (RLS/authorization, browser-flow, security scan, SEO, accessibility, performance) are available, use them as additional evidence.
- If they are not available, perform the required core checks yourself at a readiness-appropriate depth or mark the area as `not verifiable`.
- Never satisfy a review area by merely deferring to another skill. You may recommend complementary specialized reviews, but the readiness decision must rest on evidence you actually have.

## Review-readiness gate

Before running the full review, verify that enough evidence is reachable — typically: relevant project files/knowledge, the launch target, target environment, insight into central user flows, production-relevant configuration, and security/data-relevant areas.

If the available artifacts are not enough to make a defensible readiness call, do not produce a full-looking audit. Instead produce a `readiness evidence assessment`: state exactly what is missing and return **INSUFFICIENT EVIDENCE**. Partial reviews are allowed, but their limits must be explicit.

## Questions before review

Ask only questions whose answers would materially change the launch decision. Select at most 3–5 of the following blocking questions:

1. Launch stage: preview, internal beta, public beta, or regular production?
2. Which users and data are affected? Payments, PII, sensitive content, multi-org?
3. Which version and target environment/domain should be released?
4. Which core flows must work on launch day?
5. Is there an existing production database or data migration involved?
6. Who owns the launch, and is there a rollback/incident path?

Do not ask for information that can be reliably inferred from the project. For non-blocking gaps, proceed with clearly labeled assumptions.


## Workflow (compact)

Full steps in `references/readiness-workflow.md`.

1. Determine launch context and risk profile — `references/release-context.md`.
2. Pin the version, environment, and reviewed state.
3. Assess review coverage and missing evidence (readiness gate).
4. Check build, deployment, and configuration — `references/build-and-environment.md`.
5. Verify business-critical user flows — `references/functional-readiness.md`.
6. Assess security and authorization at production level — `references/security-gate.md`.
7. Check data, schema, and migration readiness — `references/data-and-migrations.md`.
8. Review performance and resilience — `references/performance-and-resilience.md`.
9. Review observability and operations — `references/observability-and-operations.md`.
10. Review privacy, compliance, accessibility, and content proportionally to the project — `references/privacy-and-compliance.md`, `references/accessibility-and-content.md`.
11. Review rollout, smoke-test, and recovery plan — `references/rollout-and-rollback.md`.
12. Consolidate blockers, accepted risks, and residual uncertainty.
13. Emit GO / CONDITIONAL GO / NO-GO / INSUFFICIENT EVIDENCE — `references/decision-and-reporting.md`.

## Output format

Follow `references/decision-and-reporting.md`. For GO / CONDITIONAL GO / NO-GO, use the full report structure defined there. For **INSUFFICIENT EVIDENCE**, use the abbreviated evidence-assessment structure defined there — do not produce a full-looking report. Sections for full reports, in order:

1. **Release context** — product type, launch stage, target environment, target users, risk profile, reviewed version.
2. **Review coverage** — table: Area | Status (`Reviewed` / `Partially reviewed` / `Not present` / `Not applicable` / `Not verifiable`) | Evidence used | Limits. Cover at minimum: Version & Environment, Build & Deployment, Functional Flows, Security, Data & Migrations, Performance & Resilience, Observability & Operations, Privacy & Compliance, Accessibility/Mobile/Content, Rollout & Recovery.
3. **Executive summary** — final decision, top blockers, top conditions, critical residual uncertainty, overall readiness picture.
4. **Critical flow status** — table: Flow | Result | Evidence | Remaining gap.
5. **Launch blockers** — real blockers only, sorted by impact/dependency.
6. **Required before launch** — non-critical but necessary pre-launch work.
7. **Accepted risks** — only if the project context actually shows deliberate acceptance; never invent.
8. **Post-launch improvements** — important but not blocking.
9. **Go-live conditions** — only for CONDITIONAL GO; each: action, ownership area, timing, verification criterion. No invented names or dates.
10. **Launch and smoke-test plan** — final pre-check, publish step, direct smoke tests, monitoring, abort criteria, recovery path. Never execute deployment.
11. **Residual uncertainty** — unreviewed areas, missing live evidence, external dependencies, missing load/security tests, decision limits.
12. **Final decision** — GO / CONDITIONAL GO / NO-GO with a short justification.


## Finding & blocker model

Classify each relevant item as: `Launch blocker`, `Required before launch`, `Accepted risk`, `Post-launch improvement`, `Not applicable`, or `Not verifiable`. Each finding: title, category, status, affected area, observed evidence, expected production state, concrete impact, recommended action, verification, and effect on the launch decision. Deduplicate by root cause.

## Hard constraints

- Do not publish, deploy, edit files, implement fixes, run migrations, alter production data, trigger real payments, or send real bulk emails/notifications.
- Do not use privileged keys or run destructive tests. Do not access real third-party user data.
- Do not claim passing tests without actual evidence. Do not claim existing backups/restore capability without proof. Do not claim legal, security, or regulatory sign-off. Do not claim general scalability without a load test.
- Do not make other skills a prerequisite; do not defer the review to another skill.
- Do not demand irrelevant enterprise processes for small/low-risk projects. Do not mix post-launch improvements with real launch blockers.
- End the response after the readiness review and wait for the user's decision.

## Reference index

- `references/readiness-workflow.md` — full step-by-step workflow.
- `references/release-context.md` — product/risk profiling.
- `references/build-and-environment.md` — build, publish config, env vars, secrets.
- `references/functional-readiness.md` — critical flows, states, browser evidence.
- `references/security-gate.md` — production-level security checks and finding handling.
- `references/data-and-migrations.md` — schema, migrations, drift, rollback of data.
- `references/performance-and-resilience.md` — perf risks, limits, resilience.
- `references/observability-and-operations.md` — logs, monitoring, ownership, incident path.
- `references/privacy-and-compliance.md` — privacy, legal, consent, PII, retention.
- `references/accessibility-and-content.md` — a11y, mobile, content, SEO conditionality.
- `references/rollout-and-rollback.md` — launch, smoke tests, rollback, recovery.
- `references/decision-and-reporting.md` — decision rules and output rules.
