# Plan Feature

## Recipe

- Category: Planning
- Recommended mode: Plan Mode
- Risk level: Medium
- Estimated scope: Small to Large

## Purpose

Create a focused, implementation-ready plan for one feature in an existing Lovable project. The plan must be grounded in the current project, preserve established behavior, expose material uncertainties, and divide the work into safe, verifiable steps without changing the project.

## Use When

- adding a new feature to an existing project
- extending an existing flow
- preparing a database, auth, billing, integration, or UI change
- the feature touches multiple files or systems
- the implementation path is not yet agreed
- acceptance criteria need to be clarified before coding

## Do Not Use When

- the requested change is already trivial, isolated, and fully specified
- the primary task is debugging an unknown failure
- a full project analysis is required first
- implementation has already been approved and only execution remains
- the request combines several unrelated features

## Required Context

Provide when known:

- `[FEATURE]`: Feature or capability to plan
- `[USER]`: Primary actor
- `[USER_GOAL]`: Outcome the actor needs
- `[BUSINESS_REASON]`: Why the feature matters
- `[CURRENT_BEHAVIOR]`: What exists now
- `[DESIRED_BEHAVIOR]`: Expected behavior
- `[ENTRY_POINT]`: Route, page, component, or trigger
- `[ACCEPTANCE_CRITERIA]`: Observable success conditions
- `[CONSTRAINTS]`: Product, technical, security, design, time, or compatibility limits
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[KNOWN_FILES]`: Relevant files or systems, when known
- `[KNOWN_RISKS]`: Existing concerns or fragile areas

## Prompt

```text
You are planning one feature for an existing Lovable project.

Do not implement the feature, edit files, change configuration, install packages, run migrations, or modify external systems. Your deliverable is an evidence-based, implementation-ready plan.

<feature_context>
Feature: [FEATURE]
Primary actor: [USER]
User goal: [USER_GOAL]
Business reason: [BUSINESS_REASON]
Current behavior: [CURRENT_BEHAVIOR]
Desired behavior: [DESIRED_BEHAVIOR]
Entry point: [ENTRY_POINT]
Acceptance criteria: [ACCEPTANCE_CRITERIA]
Constraints: [CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
Known relevant files or systems: [KNOWN_FILES]
Known risks: [KNOWN_RISKS]
</feature_context>

<objective>
Determine the smallest safe way to implement this feature within the current project. Ground the plan in the existing repository, product rules, data model, permissions, design system, and integrations. The plan must be detailed enough that a later implementation step can execute it without redesigning the feature.
</objective>

<planning_principles>
- Read Workspace Knowledge and Project Knowledge first.
- Inspect the existing project before proposing changes.
- Plan around the actual stack and established conventions.
- Preserve working behavior outside the requested scope.
- Reuse existing components, schemas, services, patterns, and providers where appropriate.
- Prefer one clear feature with testable steps over a broad rewrite.
- Use real product terms and realistic content rather than generic placeholders.
- Separate confirmed facts, evidence-supported inferences, decisions, and unresolved questions.
- Give the reason behind important recommendations.
- Do not add speculative infrastructure or abstractions for hypothetical future needs.
- Do not claim that behavior, security, performance, or compatibility is verified without evidence.
</planning_principles>

<inspection>
Inspect only what is relevant to the feature, including as applicable:

1. Workspace Knowledge and Project Knowledge
2. routes, layouts, components, and user entry points
3. current feature flow and adjacent behavior
4. shared UI components and design-system conventions
5. state management, forms, validation, queries, and cache keys
6. domain models, schema, migrations, constraints, indexes, RLS, and generated types
7. authentication, roles, memberships, ownership, and tenant boundaries
8. server routes, Edge Functions, RPCs, storage, jobs, webhooks, and external integrations
9. existing tests, scripts, error handling, logging, and deployment constraints

Trace the current and proposed flow end to end:

user action
→ UI entry point
→ state and validation
→ data-access layer
→ trusted backend or database boundary
→ resulting state
→ feedback and recovery
</inspection>

<clarification_policy>
Ask focused clarification questions only when a material ambiguity blocks a safe plan, such as:

- the user or business outcome is unclear
- two plausible behaviors would create materially different data models
- authorization or tenant rules are undefined
- a destructive or irreversible decision is required
- a provider, pricing model, or compliance rule must be selected by the user
- acceptance criteria conflict

When enough information exists, proceed. Do not ask about preferences that can be resolved from existing project conventions. Record non-blocking unknowns in the plan instead of stopping.
</clarification_policy>

<requirements_analysis>
Define:

- primary actor and goal
- trigger and entry point
- preconditions
- happy path
- alternate paths
- loading, empty, success, validation, and error states
- authorization and visibility rules
- data created, read, updated, or deleted
- notifications or external side effects
- lifecycle and state transitions
- responsive, accessibility, and content requirements
- observability and recovery requirements

Convert vague requirements into observable acceptance criteria. Do not invent product behavior when a choice is material.
</requirements_analysis>

<change_design>
For every proposed change, specify:

- affected file, module, route, component, database object, function, policy, or integration
- current responsibility
- planned responsibility
- whether it is created, extended, or reused
- dependencies and affected behavior
- authorization and validation boundary
- failure and rollback considerations

Prefer focused edits. Explicitly identify files or systems that should remain unchanged.
</change_design>

<data_and_security>
When the feature touches protected data or privileged actions:

- separate authentication from authorization
- identify user, role, membership, ownership, tenant, and resource checks
- state where each check must be enforced
- treat client-provided identity, owner, tenant, role, price, status, and permission values as untrusted
- review read and write access separately
- identify RLS, storage, service-role, webhook, cache, export, and async-job implications
- account for stale sessions or permissions where relevant

When the feature changes data:

- identify schema and migration changes
- define constraints and indexes where enforceable
- consider existing data and backward compatibility
- prefer additive or staged changes for risky transitions
- define backfill, validation, and cleanup only when required
</data_and_security>

<implementation_phases>
Break the plan into the smallest meaningful phases. Each phase must:

- have one clear purpose
- list concrete changes
- preserve a working project state where practical
- include acceptance checks
- identify dependencies
- avoid bundling unrelated cleanup

Order phases so foundational contracts come before dependent UI or integrations. For risky work, separate schema expansion, compatible application changes, backfill, switching, and cleanup.
</implementation_phases>

<verification_plan>
Define verification before implementation begins.

Include as applicable:

- build, type, lint, and automated tests
- frontend or component tests
- browser tests for critical flows
- allowed and denied authorization cases
- own-resource and cross-tenant cases
- loading, empty, validation, success, error, retry, and duplicate-submit states
- direct-route and SSR behavior
- migration pre-checks and post-checks
- webhook retry and idempotency cases
- mobile and desktop layouts
- console, network, logs, and observable failures

Every acceptance criterion must map to at least one verification step.
</verification_plan>

<boundaries>
- Planning only. Make no project changes.
- Do not install packages, create files, edit code, run migrations, or trigger external side effects.
- Do not broaden the requested feature into a redesign.
- Do not replace the stack or introduce a provider unless the feature requires it and the user approves it.
- Do not include unrelated cleanup.
- Do not expose hidden reasoning or a stream-of-consciousness account.
</boundaries>

<output_format>
Return the plan in this exact structure:

# Feature Plan: [FEATURE]

## 1. Outcome
- User outcome
- Business outcome
- What will be different when complete

## 2. Current State
- Existing flow
- Relevant architecture and conventions
- Evidence from the project
- Gaps or uncertainties

## 3. Scope
### In Scope
- [ITEMS]

### Out of Scope
- [ITEMS]

## 4. Functional Specification
- Actor and permissions
- Preconditions
- Happy path
- Alternate paths
- State transitions
- UI and content states
- Side effects and notifications

## 5. Technical Design
For each affected area:
- Current implementation
- Planned change
- Reuse versus new work
- Dependencies
- Security and data implications

Cover:
- routes and components
- state and validation
- backend and data
- permissions and tenant boundaries
- integrations and async work
- design-system usage

## 6. Data and Migration Plan
- Schema changes
- Constraints and indexes
- RLS or policy changes
- Existing-data handling
- Compatibility and rollback considerations

Write “No data changes” when none are needed.

## 7. Implementation Phases
For each phase include:
- Goal
- Changes
- Dependencies
- Acceptance check
- Risks

## 8. Verification Plan
Map each acceptance criterion to concrete checks.

## 9. Risks and Edge Cases
For each item include:
- Scenario
- Impact
- Mitigation
- Confidence

## 10. Open Questions
Include only material questions that must be answered before implementation.

## 11. Recommended First Step
Name the smallest safe implementation step. Do not execute it.
</output_format>

<quality_bar>
- Lead with the planned outcome.
- Be specific, structured, and selective.
- Reference concrete project evidence.
- Explain important trade-offs without surveying irrelevant options.
- Keep the plan proportional to the feature.
- Do not repeat the same information across sections.
- A later implementation prompt should be able to follow this plan directly.
</quality_bar>
```

## Expected Output

An implementation-ready feature plan that:

1. is grounded in the existing project
2. defines user behavior and scope
3. identifies concrete affected systems
4. covers data, authorization, integrations, and design implications
5. divides work into safe phases
6. maps acceptance criteria to verification
7. makes no project changes

## Verification Checklist

- [ ] Workspace Knowledge and Project Knowledge were reviewed.
- [ ] The current feature area was inspected before planning.
- [ ] The actor, goal, trigger, and acceptance criteria are clear.
- [ ] In-scope and out-of-scope work are explicit.
- [ ] Critical flows and state transitions are defined.
- [ ] Affected files, systems, data, and integrations are identified.
- [ ] Authentication and authorization are separated.
- [ ] Data and migration implications are addressed.
- [ ] The plan uses focused, testable phases.
- [ ] Every acceptance criterion maps to verification.
- [ ] Material unknowns are listed without inventing behavior.
- [ ] No project changes were made.
