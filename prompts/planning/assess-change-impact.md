# Assess Change Impact

## Recipe

- Category: Planning
- Recommended mode: Plan Mode
- Risk level: Medium to High
- Estimated scope: Small to Large

## Purpose

Assess the likely impact of a proposed change before implementation. The analysis should identify affected users, flows, files, data, permissions, integrations, tests, deployment behavior, and operational risks without modifying the project.

## Use When

- changing an existing feature or flow
- modifying shared components, schemas, APIs, auth, RLS, billing, storage, or integrations
- preparing a refactor, migration, provider change, or deprecation
- the change may affect multiple routes, roles, tenants, or environments
- rollback or compatibility risk is unclear
- the team needs a go/no-go recommendation before implementation

## Do Not Use When

- the task is a new feature with no existing behavior to preserve
- a complete project analysis is still required
- the change is already implemented and needs regression testing
- the primary task is diagnosing an unknown bug
- the requested work is trivial, isolated, and fully understood

## Required Context

Provide when known:

- `[PROPOSED_CHANGE]`: What may be changed
- `[REASON]`: Why the change is being considered
- `[CURRENT_BEHAVIOR]`: Existing behavior or contract
- `[DESIRED_BEHAVIOR]`: Intended result
- `[KNOWN_SCOPE]`: Routes, files, features, data, or integrations believed to be affected
- `[USERS_OR_ROLES]`: Actors who may be affected
- `[CONSTRAINTS]`: Product, technical, security, design, legal, or operational limits
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[ROLLBACK_EXPECTATION]`: Required reversibility or recovery behavior
- `[DEPLOYMENT_CONTEXT]`: Prototype, MVP, production, staged rollout, or other
- `[KNOWN_RISKS]`: Existing concerns or fragile areas

## Prompt

```text
You are assessing the impact of a proposed change in an existing Lovable project.

Do not implement the change, edit files, install packages, run migrations, change configuration, or trigger external side effects. Your only deliverable is an evidence-based change-impact assessment.

<change_context>
Proposed change: [PROPOSED_CHANGE]
Reason: [REASON]
Current behavior: [CURRENT_BEHAVIOR]
Desired behavior: [DESIRED_BEHAVIOR]
Known scope: [KNOWN_SCOPE]
Affected users or roles: [USERS_OR_ROLES]
Constraints: [CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
Rollback expectation: [ROLLBACK_EXPECTATION]
Deployment context: [DEPLOYMENT_CONTEXT]
Known risks: [KNOWN_RISKS]
</change_context>

<objective>
Determine what the proposed change could affect, how severe each impact could be, which dependencies or contracts may break, and what safeguards are required before implementation.

Ground the assessment in the current project. Distinguish direct impact, indirect impact, compatibility risk, operational risk, and unresolved uncertainty.
</objective>

<principles>
1. Read Workspace Knowledge and Project Knowledge first.
2. Inspect the current implementation before estimating impact.
3. Trace behavior and dependencies rather than relying on filenames alone.
4. Preserve existing working behavior unless the change explicitly replaces it.
5. Prefer concrete project evidence over generic assumptions.
6. Distinguish confirmed impact, likely impact, possible impact, and unknown impact.
7. Explain the reason behind important conclusions.
8. Do not propose a broad rewrite unless the evidence shows the current design cannot support the change safely.
9. Do not treat frontend visibility as authorization.
10. Do not claim that tests, migrations, security, compatibility, or rollback are verified without evidence.
11. Stop after the assessment. Do not implement anything.
</principles>

<inspection_scope>
Inspect only relevant areas, expanding when dependencies require it:

- Workspace Knowledge and Project Knowledge
- entry points, routes, layouts, components, and shared UI
- feature modules and adjacent flows
- state management, validation, queries, cache keys, and derived state
- domain models and state transitions
- database schema, migrations, constraints, indexes, RLS, functions, triggers, and generated types
- authentication, sessions, roles, memberships, ownership, and tenant boundaries
- storage, files, signed URLs, exports, and retention
- server routes, Edge Functions, RPCs, jobs, queues, webhooks, and external providers
- tests, scripts, build configuration, deployment setup, logging, and observability
</inspection_scope>

<impact_dimensions>
Assess the proposed change across every relevant dimension.

## Product and user impact
- users, roles, tenants, or teams affected
- changed user journeys
- changed permissions or visibility
- changed content, expectations, or commitments
- new loading, empty, validation, success, and error states
- accessibility or responsive behavior

## Functional impact
- affected flows and state transitions
- changed contracts or assumptions
- adjacent features that reuse the same components, data, or services
- existing behavior that must remain unchanged
- failure and recovery behavior

## Architecture impact
- routes, layouts, components, hooks, services, schemas, and utilities
- shared abstractions or provider boundaries
- SSR, client-only code, caching, and request isolation
- duplicated or coupled logic
- new dependency or package implications

## Data impact
- records created, read, updated, migrated, archived, or deleted
- schema, constraints, indexes, generated types, and compatibility
- existing-data quality and backfill requirements
- retention, deletion, audit, and export implications
- rollback limitations

## Security and authorization impact
- authentication and session behavior
- role, membership, ownership, tenant, and resource rules
- RLS, backend authorization, storage access, and service-role usage
- stale permissions, signed URLs, realtime connections, caches, jobs, and exports
- secrets, provider credentials, and sensitive-data exposure

## Integration impact
- API contracts, OAuth bindings, provider IDs, webhook payloads, and retries
- idempotency and duplicate-event behavior
- rate limits, timeouts, provider failure, and reconciliation
- test versus production configuration

## Operational impact
- migrations, deployment order, feature flags, rollout, and rollback
- monitoring, logs, alerts, support procedures, and incident recovery
- background jobs, queues, scheduled work, and partial failures
- customer communication or admin tooling

## Quality impact
- tests that must change or be added
- regression surface
- browser, mobile, desktop, SSR, and direct-route checks
- performance or scalability risks
</impact_dimensions>

<dependency_tracing>
Trace dependencies in both directions.

Downstream:
What consumes the component, function, schema, policy, event, provider response, or data contract being changed?

Upstream:
What creates, validates, authorizes, or supplies the changed value or behavior?

For shared contracts, inspect:
- imports and callers
- route usage
- database dependencies
- generated types
- cache keys
- tests and fixtures
- jobs and webhooks
- exports and analytics
- external provider mappings

Do not assume a change is isolated because only one file appears to require editing.
</dependency_tracing>

<compatibility_analysis>
Determine whether the change is:

- backward-compatible
- backward-compatible only with staged rollout
- temporarily dual-compatible
- breaking
- destructive
- difficult or impossible to roll back

Consider:
- old and new application versions running at the same time
- old records and new records coexisting
- clients or jobs using old contracts
- delayed webhooks or queued messages
- stale sessions, caches, and generated links
- external consumers or exports
- schema rollback versus code rollback

When a staged rollout is required, identify the safe order:
1. expand
2. deploy compatible readers/writers
3. backfill or migrate
4. validate
5. switch behavior
6. remove obsolete paths
</compatibility_analysis>

<risk_scoring>
For each finding assign:

- Likelihood: low, medium, high
- Severity: low, medium, high, critical
- Confidence: low, medium, high
- Detectability: easy, moderate, difficult

Classify the finding as:
- confirmed impact
- likely impact
- possible impact
- compatibility risk
- security risk
- operational risk
- unknown

Do not manufacture numeric precision. Use qualitative ratings supported by evidence.
</risk_scoring>

<clarification_policy>
Ask focused questions only when a material ambiguity prevents a reliable assessment, such as:

- the intended behavior changes a core product contract
- the source of truth is undefined
- authorization or tenant behavior is unclear
- destructive data handling is undecided
- rollout or rollback requirements conflict
- a provider or external contract is unknown

When uncertainty is non-blocking, record it explicitly and continue.
</clarification_policy>

<boundaries>
- Analysis only. Make no changes.
- Do not create files, edit code, install packages, run migrations, or call external services.
- Do not broaden the proposed change into a redesign.
- Do not include unrelated cleanup.
- Do not expose hidden reasoning or a stream-of-consciousness account.
- Do not mark uninspected areas as safe.
</boundaries>

<output_format>
Return the assessment in this exact structure:

# Change Impact Assessment: [PROPOSED_CHANGE]

## 1. Executive Summary
- Change being assessed
- Overall impact level: low, medium, high, or critical
- Go / go with safeguards / no-go recommendation
- Most important reasons
- Confidence

## 2. Current Contract
- Current behavior
- Source of truth
- Relevant users and systems
- Evidence from the project

## 3. Direct Impact
For each directly affected area:
- Area
- Current responsibility
- Expected change
- Evidence
- Impact level
- Required safeguard

## 4. Indirect and Regression Impact
For each adjacent area:
- Dependency
- Why it may be affected
- Failure mode
- Likelihood
- Severity
- Confidence

## 5. Data and Migration Impact
- Schema and contract changes
- Existing-data handling
- Backfill or cleanup needs
- Compatibility between old and new behavior
- Rollback limitations
- Required pre-checks and post-checks

Write “No data impact identified” only when supported by inspection.

## 6. Security and Authorization Impact
- Authentication/session implications
- Role, membership, ownership, tenant, and resource checks
- RLS/backend/storage implications
- Service-role, cache, export, realtime, and job implications
- Sensitive-data or secret exposure risk
- Required negative tests

## 7. Integration and Operational Impact
- Provider and API contracts
- Webhooks, jobs, retries, and idempotency
- Deployment order
- Feature flag or staged rollout needs
- Monitoring and support implications
- Recovery and rollback strategy

## 8. Test and Verification Impact
Map affected behavior to:
- automated tests
- browser flows
- allowed and denied cases
- loading, error, retry, and duplicate-submit states
- migration validation
- mobile, desktop, SSR, and direct-route checks
- observability checks

## 9. Risk Register

| Finding | Type | Likelihood | Severity | Detectability | Confidence | Safeguard |
|---|---|---|---|---|---|---|

## 10. Areas That Should Remain Unchanged
List behaviors, files, contracts, providers, or flows that should not be modified.

## 11. Open Questions
Include only material questions that could change the recommendation or implementation order.

## 12. Recommendation
State:
- go, go with safeguards, or no-go
- required safeguards before implementation
- safest rollout order
- smallest safe next step

Do not implement the change.
</output_format>

<quality_bar>
- Lead with the recommendation and impact level.
- Be specific, structured, and evidence-based.
- Separate direct impact from indirect risk.
- Explain important dependency and compatibility conclusions.
- Avoid vague statements such as “this may affect other areas” without naming them.
- Do not repeat the same finding in multiple sections.
- Keep the assessment proportional to the change.
</quality_bar>
```

## Expected Output

An evidence-based impact assessment that:

1. identifies direct and indirect dependencies
2. evaluates product, architecture, data, security, integration, and operational impact
3. distinguishes confirmed, likely, possible, and unknown impact
4. scores risks qualitatively
5. defines compatibility, rollout, and rollback requirements
6. recommends go, go with safeguards, or no-go
7. makes no project changes

## Verification Checklist

- [ ] Workspace Knowledge and Project Knowledge were reviewed.
- [ ] The current implementation and relevant dependencies were inspected.
- [ ] Direct and indirect impacts are separated.
- [ ] Upstream and downstream dependencies are traced.
- [ ] Data, migration, and rollback implications are addressed.
- [ ] Authentication and authorization are analyzed separately.
- [ ] External integrations and async behavior are considered.
- [ ] Old/new version and old/new data compatibility are considered.
- [ ] Every major risk includes evidence and a safeguard.
- [ ] Uninspected areas are not marked safe.
- [ ] The recommendation is explicit.
- [ ] No project changes were made.
