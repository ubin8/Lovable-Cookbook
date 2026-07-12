# Implement Small Feature

## Recipe

- Category: Implementation
- Recommended mode: Agent Mode
- Risk level: Low to Medium
- Estimated scope: Small

## Purpose

Implement one narrowly scoped feature in an existing Lovable project with minimal surface area.

This recipe is designed for changes that should fit into one focused implementation pass. It emphasizes execution discipline: inspect first, change only what is required, preserve existing contracts, verify the result, and stop when the accepted scope is complete.

## Use When

- the feature is already defined
- acceptance criteria are available
- the affected area is limited and understood
- the change should fit into a small number of files
- no major migration, provider change, or architecture redesign is required
- implementation can be verified in one pass

## Do Not Use When

- the feature is still ambiguous
- multiple unrelated outcomes are bundled together
- the change requires a broad refactor
- the database model or authorization model is still undecided
- a risky migration or rollout sequence is required
- the main task is debugging rather than implementation
- the feature should first be broken into multiple steps

## Inputs

- `[FEATURE]`: Small feature to implement
- `[USER_OUTCOME]`: Observable result for the user
- `[CURRENT_BEHAVIOR]`: Existing behavior
- `[DESIRED_BEHAVIOR]`: Required behavior
- `[ENTRY_POINT]`: Route, page, component, or trigger
- `[ACCEPTANCE_CRITERIA]`: Agreed pass/fail conditions
- `[RELEVANT_AREAS]`: Known files, components, services, routes, or data
- `[PERMISSIONS]`: Roles, ownership, tenant, or visibility rules
- `[CONSTRAINTS]`: Technical, design, compatibility, or product limits
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[VERIFICATION]`: Required checks or user flows

## Prompt

```text
Implement the following small feature in the existing Lovable project.

<feature>
[FEATURE]
</feature>

<outcome>
User outcome: [USER_OUTCOME]
Current behavior: [CURRENT_BEHAVIOR]
Desired behavior: [DESIRED_BEHAVIOR]
Entry point: [ENTRY_POINT]
Acceptance criteria: [ACCEPTANCE_CRITERIA]
</outcome>

<context>
Known relevant areas: [RELEVANT_AREAS]
Permissions and visibility: [PERMISSIONS]
Constraints: [CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
Required verification: [VERIFICATION]
</context>

Your task is to complete this feature with the smallest safe implementation.

Do not redesign the surrounding system, broaden the scope, replace working architecture, or add unrelated cleanup.

## Execution Contract

Work in this order:

### 1. Inspect

Before editing:

- read Workspace Knowledge and Project Knowledge
- inspect the relevant route, component, service, schema, validation, and tests
- identify the current source of truth
- identify the existing pattern to reuse
- confirm the smallest affected surface
- note any material conflict between the request and the current project

Do not produce a long project analysis. Summarize only what is necessary to justify the implementation approach.

### 2. Confirm Scope

State briefly:

- what will change
- what will not change
- which acceptance criteria will be satisfied
- which files or systems are expected to be affected
- whether data, permissions, integrations, or migrations are involved

Proceed without asking questions when the intended behavior is clear from the request and project.

Ask only when a blocking ambiguity would change:
- the user-visible outcome
- authorization
- data ownership
- destructive behavior
- provider behavior
- an irreversible state transition

### 3. Implement

Make focused changes only.

Implementation rules:

- reuse existing components, hooks, utilities, schemas, services, and patterns
- preserve existing routing, rendering, styling, and state conventions
- use the current backend and provider setup
- keep TypeScript strict where supported
- avoid weakening types or validation to make the feature compile
- keep business rules out of presentation-only code
- keep secrets and privileged operations server-side
- enforce protected actions at a trusted backend or database boundary
- preserve relevant SSR behavior
- do not introduce a new package unless the feature cannot be completed safely without it
- do not create a new abstraction for one small use case unless it clearly improves the existing design
- do not modify unrelated files

For UI work:

- use existing shared components and design tokens
- implement relevant loading, empty, success, validation, disabled, and error states
- prevent duplicate submissions
- preserve keyboard and focus behavior
- support the layouts already expected by the project
- keep user-facing copy direct and consistent

For data work:

- use the existing data-access pattern
- fetch and mutate only required data
- preserve the existing source of truth
- validate ownership, role, tenant, resource, status, and other protected values server-side
- review read and write access separately
- scope cache keys to every value that changes the result
- update generated types only when the data contract actually changes

For database work:

- use a versioned migration
- prefer additive changes
- add enforceable constraints where appropriate
- inspect existing data before adding non-null, unique, or restrictive constraints
- do not use destructive shortcuts
- do not treat code rollback as database rollback

For integrations:

- use existing provider boundaries
- keep credentials server-side
- bind external IDs to the correct user, tenant, or resource
- make retryable operations idempotent
- verify provider events before applying changes

### 4. Self-Review

Before verification, inspect the final diff and remove:

- unrelated edits
- dead code
- unused imports
- temporary logs
- mocks or bypasses
- duplicated logic
- vague TODOs
- weakened types
- placeholder behavior presented as complete

Check whether the feature accidentally changed:
- shared components
- route behavior
- permissions
- cache scope
- data contracts
- existing user flows

### 5. Verify

Run the smallest relevant verification set.

At minimum:

- verify every acceptance criterion
- verify the main user flow
- verify affected existing behavior
- inspect relevant console and network failures
- run available build, type, lint, or tests that cover the change

When applicable, also verify:

- allowed and denied access
- own-resource and unauthorized-resource cases
- loading, empty, validation, success, and error states
- duplicate submissions
- mobile and desktop behavior
- direct-route and SSR behavior
- database migration result
- webhook retry or duplicate-event behavior

Do not claim a check passed unless it was actually run or directly verified.

When a check cannot be completed:
- state exactly what was not verified
- explain why
- do not mark the feature fully verified

## Scope Guard

Stop and report instead of continuing when inspection shows that the request:

- requires a breaking schema redesign
- needs a new backend or provider
- changes the authorization model
- affects several independent flows
- requires destructive migration
- conflicts with Project Knowledge
- cannot be safely completed as one small feature

In that case, explain why the task should be replanned or broken into steps. Do not force a partial implementation that creates an unsafe or misleading state.

## Completion Conditions

The task is complete only when:

- the requested user outcome exists
- all MUST acceptance criteria pass
- existing affected behavior still works
- protected behavior is enforced at a trusted boundary
- relevant states are implemented
- no unrelated changes remain
- verification results are reported honestly

## Final Response Format

Return a concise implementation report:

# Implemented: [FEATURE]

## What Changed
- [USER-VISIBLE CHANGE]
- [BEHAVIORAL CHANGE]

## Files Changed
- `[PATH]`: [PURPOSE]

## Data or Permission Changes
- [CHANGE]
- Write “None” when none were required.

## Verification
- [CHECK]: [PASS / FAIL / NOT RUN]
- [CHECK]: [PASS / FAIL / NOT RUN]

## Acceptance Criteria
- [CRITERION]: [MET / NOT MET / PARTIALLY MET]

## Remaining Risks or Limitations
- [ITEM]
- Write “None identified” only when justified.

Do not include a long retrospective or repeat the full prompt.
```

## Expected Output

A small, production-appropriate implementation that:

1. changes only the required behavior
2. follows the current project architecture
3. reuses established patterns
4. preserves existing behavior outside scope
5. enforces data and permission boundaries correctly
6. verifies the accepted outcome
7. reports unverified areas honestly

## Quality Checklist

- [ ] The feature remained small and focused.
- [ ] Existing architecture and conventions were inspected first.
- [ ] No unrelated refactor or cleanup was included.
- [ ] All acceptance criteria were addressed.
- [ ] Protected actions use trusted authorization.
- [ ] Relevant UI and failure states were implemented.
- [ ] The final diff was self-reviewed.
- [ ] Verification results are evidence-based.
- [ ] Unverified areas are explicit.
- [ ] The project remains in a working state.
