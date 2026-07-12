# Break Feature into Steps

## Recipe

- Category: Planning
- Recommended mode: Plan Mode
- Risk level: Medium
- Estimated scope: Medium to Large

## Purpose

Turn an already-defined feature into a sequence of small, ordered, independently verifiable implementation steps for an existing Lovable project. The output should reduce risk, avoid broad rewrites, preserve working behavior, and make each step suitable for a separate implementation prompt.

## Use When

- a feature plan exists but is too large to implement safely in one prompt
- the feature spans UI, backend, database, permissions, integrations, or testing
- a risky change needs staged delivery
- implementation should proceed one milestone at a time
- the team wants clear checkpoints and rollback boundaries
- previous broad prompts caused regressions or unrelated changes

## Do Not Use When

- the feature is not yet defined well enough to plan
- a full project analysis is still required
- the task is a single isolated change
- the primary need is debugging an unknown failure
- unrelated features have been grouped into one request

## Required Context

Provide when known:

- `[FEATURE]`: Feature to decompose
- `[FEATURE_PLAN]`: Existing plan, specification, or accepted behavior
- `[USER_OUTCOME]`: Result the user must achieve
- `[ACCEPTANCE_CRITERIA]`: Observable completion conditions
- `[CURRENT_STATE]`: Relevant behavior that already exists
- `[AFFECTED_AREAS]`: Known routes, components, data, permissions, or integrations
- `[CONSTRAINTS]`: Technical, product, design, security, or compatibility limits
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[DELIVERY_PREFERENCE]`: Vertical slices, technical layers, or safest order
- `[MAX_STEP_SIZE]`: Maximum scope allowed per implementation step
- `[KNOWN_RISKS]`: Fragile areas, migration concerns, or external dependencies

## Prompt

```text
You are decomposing one approved feature into small implementation steps for an existing Lovable project.

Do not implement the feature, edit files, install packages, run migrations, change configuration, or trigger external side effects. Your only deliverable is a staged execution plan.

<feature_context>
Feature: [FEATURE]
Existing feature plan or specification:
[FEATURE_PLAN]

User outcome: [USER_OUTCOME]
Acceptance criteria: [ACCEPTANCE_CRITERIA]
Current state: [CURRENT_STATE]
Known affected areas: [AFFECTED_AREAS]
Constraints: [CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
Delivery preference: [DELIVERY_PREFERENCE]
Maximum step size: [MAX_STEP_SIZE]
Known risks: [KNOWN_RISKS]
</feature_context>

<objective>
Break the feature into the smallest meaningful implementation steps that can be completed, reviewed, and verified independently while keeping the project in a valid state.

Each step should be suitable for a separate Lovable implementation prompt. The sequence must respect dependencies, data compatibility, authorization boundaries, and existing project conventions.
</objective>

<core_principles>
1. Read Workspace Knowledge and Project Knowledge first.
2. Inspect the relevant existing implementation before decomposing the work.
3. Base the steps on the actual repository, not a generic architecture.
4. Preserve working behavior outside the requested scope.
5. Prefer focused edits over broad rewrites.
6. Reuse existing components, schemas, services, providers, and conventions.
7. Separate unrelated concerns into separate steps.
8. Keep each step independently understandable and verifiable.
9. Put foundational contracts before dependent UI or integrations.
10. Do not add speculative infrastructure for hypothetical future needs.
11. Do not claim behavior, security, migrations, or tests are verified without evidence.
12. Stop after producing the decomposition. Do not implement anything.
</core_principles>

<decomposition_strategy>
Choose the safest decomposition model for this feature.

Prefer vertical slices when one step can deliver a complete, testable user outcome across UI and backend without creating unsafe temporary states.

Prefer staged technical sequencing when the feature requires:
- schema expansion before application changes
- compatible deployment before data backfill
- authorization or RLS changes before exposing UI
- provider setup before webhook handling
- server contracts before client integration
- migration, switch, and cleanup phases
- feature flags or gradual rollout

Do not split work only by file count. Split by behavior, dependency, risk, and verification boundary.
</decomposition_strategy>

<inspection>
Inspect as relevant:

- Workspace Knowledge and Project Knowledge
- current routes, layouts, components, and entry points
- existing domain objects and state transitions
- forms, validation, state management, queries, and cache keys
- database schema, migrations, constraints, indexes, RLS, functions, and generated types
- authentication, roles, memberships, ownership, and tenant boundaries
- server routes, Edge Functions, RPCs, storage, jobs, webhooks, and integrations
- design-system components and responsive patterns
- tests, scripts, deployment constraints, and observability

Trace the affected flow end to end:

user action
→ UI entry point
→ state and validation
→ data-access layer
→ trusted backend or database boundary
→ resulting state
→ user feedback and recovery
</inspection>

<step_rules>
Every implementation step must:

- have one clear purpose
- change one coherent behavior or contract
- list concrete affected areas
- identify prerequisites
- define what must remain unchanged
- include implementation instructions
- include verification and acceptance checks
- identify authorization, data, design, and operational implications
- leave the project in a working state where practical
- avoid unrelated cleanup
- be small enough for one focused Lovable prompt

A step is too large when it:
- contains multiple independent user outcomes
- combines schema redesign, UI overhaul, and provider integration without checkpoints
- affects unrelated routes or modules
- requires vague instructions such as “finish the rest”
- cannot be verified without completing later steps
- has no clear rollback or failure boundary

A step is too small when it:
- produces no meaningful contract or observable progress
- separates tightly coupled edits that must ship together
- creates avoidable broken intermediate states
- consists only of mechanical file-by-file changes without behavioral value
</step_rules>

<data_and_migration_order>
When data changes are required, use a compatible sequence where applicable:

1. inspect current schema and dependent code
2. expand schema additively
3. add constraints or policies that are safe for existing behavior
4. regenerate types or contracts
5. update server/data-access logic
6. update UI and client behavior
7. backfill existing data
8. validate results
9. switch reads or writes to the new model
10. remove obsolete behavior only after compatibility is confirmed

Do not treat a code rollback as a database rollback. Do not include destructive cleanup in the same step as initial rollout unless it is provably safe.
</data_and_migration_order>

<security_order>
When protected data or privileged actions are involved:

1. define actor, role, membership, ownership, tenant, and resource rules
2. define the trusted enforcement boundary
3. implement or update backend/database authorization
4. verify allowed and denied cases
5. expose or update the UI
6. add audit, logging, or operational controls where required

Do not make frontend visibility the first or only security step.
</security_order>

<integration_order>
When an external provider is involved:

1. confirm the existing provider or approved integration
2. define ownership and tenant binding
3. define server-side credentials and secret boundaries
4. define internal data contracts
5. implement the outbound or inbound server operation
6. add idempotency and retry behavior
7. add webhook verification where needed
8. connect the client UI
9. verify duplicate, delayed, failed, and out-of-order behavior

Do not expose provider secrets or use provider IDs as authorization proof.
</integration_order>

<verification_mapping>
Map every acceptance criterion to one or more steps.

For each step define:

- local acceptance check
- relevant build, type, lint, or test command
- browser flow to verify
- allowed and denied authorization cases
- data or migration checks
- loading, empty, success, validation, and error states
- retry, duplicate-submit, or idempotency cases
- mobile, desktop, direct-route, or SSR checks where relevant
- observable failure or rollback conditions

Do not postpone all verification until the final step.
</verification_mapping>

<clarification_policy>
Ask focused questions only when a material ambiguity prevents safe sequencing, such as:

- the feature outcome or acceptance criteria conflict
- an irreversible migration decision is unresolved
- authorization or tenant rules are missing
- a provider or source of truth must be selected
- two sequencing options create materially different risks

When the ambiguity is non-blocking, record it as an assumption or open question and continue.
</clarification_policy>

<boundaries>
- Planning only. Make no project changes.
- Do not create files, edit code, install packages, run migrations, or call external services.
- Do not expand the feature into a broader redesign.
- Do not add unrelated refactoring, cleanup, or modernization.
- Do not split steps by arbitrary file count.
- Do not expose hidden reasoning or a stream-of-consciousness account.
</boundaries>

<output_format>
Return the result in this exact structure:

# Feature Breakdown: [FEATURE]

## 1. Target Outcome
- User outcome
- Final observable behavior
- Acceptance criteria summary

## 2. Current Foundations
- Existing implementation to reuse
- Relevant conventions
- Known dependencies
- Evidence from the project

## 3. Sequencing Strategy
- Chosen model: vertical slices, staged layers, or hybrid
- Why this model is safest
- Key dependency order
- Temporary compatibility requirements

## 4. Step Overview

| Step | Goal | Depends On | Risk | Verifiable Outcome |
|---|---|---|---|---|
| 1 | [GOAL] | [DEPENDENCY] | [LOW/MEDIUM/HIGH] | [OUTCOME] |

## 5. Detailed Implementation Steps

For each step use:

### Step [N]: [ACTION-ORIENTED TITLE]

**Goal**
[ONE CLEAR PURPOSE]

**Why this step exists**
[DEPENDENCY OR RISK IT ADDRESSES]

**Prerequisites**
- [PREREQUISITE]

**Affected areas**
- [FILE / ROUTE / COMPONENT / DATABASE OBJECT / POLICY / INTEGRATION]

**Implementation instructions**
1. [CONCRETE CHANGE]
2. [CONCRETE CHANGE]
3. [CONCRETE CHANGE]

**Must remain unchanged**
- [BEHAVIOR OR AREA]

**Security and data considerations**
- [AUTHORIZATION / TENANT / MIGRATION / PRIVACY / IDEMPOTENCY]

**Acceptance checks**
- [OBSERVABLE RESULT]

**Verification**
- [CHECK, TEST, OR USER FLOW]

**Failure or rollback boundary**
- [HOW TO STOP, REVERT, OR RECOVER SAFELY]

**Ready for next step when**
- [OBJECTIVE CONDITION]

## 6. Final Integration Verification
- End-to-end user flow
- Cross-step compatibility
- Allowed and denied access
- Existing behavior regression checks
- Data and migration validation
- Integration, retry, and failure behavior
- Mobile, desktop, SSR, or direct-route checks where relevant

## 7. Deferred Work
List only intentional exclusions or later cleanup. Do not hide required work here.

## 8. Open Questions
Include only material questions that affect sequencing or implementation.

## 9. Recommended First Step
Name the smallest safe first step and explain why it should come first. Do not execute it.
</output_format>

<quality_bar>
- Use action-oriented step titles.
- Keep each step proportional and independently verifiable.
- Reference concrete project evidence.
- Explain dependency order and important trade-offs.
- Avoid repeating the full feature specification in every step.
- Do not use vague steps such as “implement backend,” “finish UI,” or “test everything.”
- Make the breakdown directly usable as a sequence of implementation prompts.
</quality_bar>
```

## Expected Output

A staged feature breakdown that:

1. uses the existing project as evidence
2. chooses an explicit sequencing strategy
3. creates small, coherent implementation steps
4. preserves working intermediate states
5. places data and authorization changes in a safe order
6. maps acceptance criteria to step-level verification
7. defines clear readiness and rollback boundaries
8. makes no project changes

## Verification Checklist

- [ ] Workspace Knowledge and Project Knowledge were reviewed.
- [ ] The relevant implementation was inspected.
- [ ] The feature is one coherent outcome.
- [ ] The sequencing strategy is explicit and justified.
- [ ] Every step has one clear purpose.
- [ ] Dependencies and prerequisites are documented.
- [ ] Data, authorization, integrations, and UI are ordered safely.
- [ ] Each step has acceptance and verification checks.
- [ ] The project remains usable between steps where practical.
- [ ] Unrelated cleanup and redesign are excluded.
- [ ] The final result can be used as separate implementation prompts.
- [ ] No project changes were made.
