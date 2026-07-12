# Extend Existing Flow

## Recipe

- Category: Implementation
- Recommended mode: Agent Mode
- Risk level: Medium
- Estimated scope: Small to Medium

## Purpose

Extend an existing user flow without breaking its current behavior, contracts, or downstream effects.

This recipe is for adding one new branch, step, state, role, outcome, or integration point to a flow that already works. It emphasizes preserving the original path, understanding the flow end to end, and introducing the extension at the smallest safe boundary.

## Use When

- adding a new step to an existing flow
- adding an alternate path or branch
- extending a form, wizard, checkout, onboarding, approval, booking, or lifecycle flow
- supporting an additional role or state
- adding one new side effect such as notification, export, or provider call
- preserving the original flow is as important as implementing the new behavior

## Do Not Use When

- the flow is fundamentally broken
- the change replaces rather than extends the current behavior
- the state model needs a full redesign
- multiple unrelated flows are involved
- the task requires a broad architecture migration
- the flow is not yet understood
- the extension cannot be delivered without breaking compatibility

## Inputs

- `[FLOW]`: Existing flow to extend
- `[EXTENSION]`: New step, branch, state, role, or outcome
- `[PRIMARY_ACTOR]`: User or system actor
- `[ENTRY_POINT]`: Route, component, event, or trigger
- `[CURRENT_PATH]`: Existing sequence of behavior
- `[NEW_PATH]`: Required extension
- `[PRESERVED_BEHAVIOR]`: Existing behavior that must remain unchanged
- `[ACCEPTANCE_CRITERIA]`: Observable pass/fail conditions
- `[STATE_RULES]`: Relevant statuses and transitions
- `[PERMISSIONS]`: Role, ownership, tenant, or visibility rules
- `[SIDE_EFFECTS]`: Notifications, jobs, billing, exports, or integrations
- `[CONSTRAINTS]`: Technical, product, design, or compatibility limits
- `[OUT_OF_SCOPE]`: Explicit exclusions

## Prompt

```text
Extend the following existing flow in the current Lovable project.

<flow>
Name: [FLOW]
Primary actor: [PRIMARY_ACTOR]
Entry point: [ENTRY_POINT]
Current path: [CURRENT_PATH]
Requested extension: [EXTENSION]
New path: [NEW_PATH]
Behavior that must remain unchanged: [PRESERVED_BEHAVIOR]
Acceptance criteria: [ACCEPTANCE_CRITERIA]
</flow>

<rules>
State rules: [STATE_RULES]
Permissions and visibility: [PERMISSIONS]
Side effects: [SIDE_EFFECTS]
Constraints: [CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
</rules>

Your task is to add the extension while preserving the existing flow and its contracts.

Do not rebuild the whole flow, duplicate its logic, or move behavior to a new architecture unless the current design makes a safe extension impossible.

## Flow-First Execution

Work from the flow, not from individual files.

### 1. Reconstruct the Existing Flow

Before editing, trace the current behavior from entry to completion:

actor
→ entry point
→ UI state
→ validation
→ data access
→ trusted authorization boundary
→ state transition
→ side effects
→ user feedback
→ retry or recovery

Identify:

- the current source of truth
- existing branch points
- existing state transitions
- shared components and services
- authorization checks
- side effects and integrations
- failure and rollback behavior
- tests that already protect the flow

Read Workspace Knowledge and Project Knowledge before making changes.

Do not assume the flow from UI alone. Inspect the server, database, policies, jobs, and integrations that complete it.

### 2. Define the Insertion Point

State briefly:

- where the extension enters the flow
- which existing branch or contract it depends on
- which existing path remains unchanged
- which state transition or output is new
- which files and systems are expected to change
- whether the extension is backward-compatible

Choose one insertion point. Avoid scattering the new behavior across unrelated layers.

### 3. Protect the Existing Path

Before adding the new branch:

- identify the original happy path
- identify existing alternate and failure paths
- identify the contracts that callers depend on
- identify fields, statuses, events, and responses that must not change
- identify existing users, roles, or tenants that must see no behavioral difference

Add or preserve regression coverage for the original path before changing shared logic when practical.

Do not change current defaults unless the request explicitly requires it.

### 4. Implement the Extension

Add only the behavior required for the new branch.

#### Branching

- express the new condition explicitly
- avoid implicit behavior based on missing values or UI state
- keep branch selection deterministic
- reject impossible or unauthorized combinations
- do not create hidden fallbacks that bypass business rules

#### State transitions

- define the allowed source state
- define the allowed target state
- define who may trigger the transition
- define required data
- reject repeated or forbidden transitions
- preserve prior history when reopening or revisiting states matters

#### Data

- extend the existing source of truth
- avoid parallel state that can drift
- use additive schema changes when required
- preserve old records and old clients where relevant
- update constraints, generated types, and policies only when the contract changes
- do not trust client-supplied owner, role, tenant, status, price, or permission values

#### Authorization

- enforce the new path at a trusted backend or database boundary
- validate actor, role, membership, ownership, tenant, resource, and current state
- review read and write access separately
- do not treat route access or hidden controls as authorization
- ensure the new path does not broaden access to the old path

#### UI

- preserve the visual and behavioral language of the existing flow
- add the new step or branch using existing components
- keep progress, back, cancel, retry, and completion behavior coherent
- show clear loading, disabled, validation, success, and error states
- prevent duplicate progression or submission
- avoid forcing existing users through the new branch unless required

#### Side effects

- trigger notifications, jobs, billing, exports, or provider calls only after the trusted state change succeeds
- make retryable side effects idempotent
- bind external IDs to the correct actor, tenant, and resource
- define what happens when a side effect fails after the main state change
- do not duplicate existing notifications or jobs

### 5. Verify Both Paths

Verification must cover the original flow and the extension.

#### Original path

- same entry behavior
- same allowed actors
- same state transitions
- same data result
- same side effects
- same user-visible outcome
- same failure behavior

#### New path

- correct entry condition
- correct allowed actor
- correct validation
- correct authorization
- correct state transition
- correct side effects
- correct completion and recovery behavior

#### Boundary cases

Test as applicable:

- actor is not allowed
- resource belongs to another user or tenant
- current state is invalid
- the new action is repeated
- a side effect times out or fails
- the user navigates backward or reloads
- a stale session or stale page submits old state
- old records do not contain new fields
- direct-route or SSR loading
- mobile and desktop layouts

Do not mark the extension complete when only the new branch was tested.

## Compatibility Guard

Stop and report instead of forcing implementation when the extension requires:

- changing the meaning of an existing status
- breaking an existing API or database contract
- changing default behavior for existing users
- weakening authorization
- duplicating the source of truth
- destructive migration
- replacing the provider or backend
- rewriting the full flow
- combining several independent branches

In that case, explain whether the task needs:
- a migration plan
- a feature flag
- a staged rollout
- a separate refactor
- a larger feature breakdown

## Completion Conditions

The extension is complete only when:

- the new branch works
- the original branch still works
- branch conditions are explicit
- state transitions are valid
- authorization is enforced at a trusted boundary
- side effects are safe against retries
- existing users are not unintentionally redirected
- acceptance criteria are verified
- unverified areas are stated

## Final Response Format

# Extended Flow: [FLOW]

## Extension Added
- [NEW STEP, BRANCH, STATE, OR OUTCOME]

## Insertion Point
- [WHERE THE EXTENSION ENTERS THE FLOW]

## Existing Behavior Preserved
- [ORIGINAL PATH OR CONTRACT]

## Files Changed
- `[PATH]`: [PURPOSE]

## State, Data, or Permission Changes
- [CHANGE]
- Write “None” when none were required.

## Verification

### Original Path
- [CHECK]: [PASS / FAIL / NOT RUN]

### New Path
- [CHECK]: [PASS / FAIL / NOT RUN]

### Boundary Cases
- [CHECK]: [PASS / FAIL / NOT RUN]

## Acceptance Criteria
- [CRITERION]: [MET / NOT MET / PARTIALLY MET]

## Remaining Risks
- [ITEM]
- Write “None identified” only when justified.
```

## Expected Output

A focused extension that:

1. adds one clear branch, step, state, role, or side effect
2. preserves the existing path
3. reuses the current source of truth
4. enforces the new behavior at trusted boundaries
5. handles retry, failure, and compatibility concerns
6. verifies both old and new behavior
7. avoids a full-flow rewrite

## Quality Checklist

- [ ] The existing flow was traced end to end.
- [ ] The insertion point is explicit.
- [ ] The original path remains unchanged unless explicitly required.
- [ ] The new branch is deterministic.
- [ ] State transitions are valid and enforced.
- [ ] Authorization is not widened accidentally.
- [ ] Side effects are idempotent where required.
- [ ] Old records and existing callers remain compatible.
- [ ] Both original and new paths were verified.
- [ ] No unrelated redesign or cleanup was included.
