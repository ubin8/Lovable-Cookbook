# Refactor Without Behavior Change

## Recipe

- Category: Implementation
- Recommended mode: Agent Mode
- Risk level: Medium
- Estimated scope: Small to Medium

## Purpose

Improve the internal structure of existing code without changing observable behavior.

This recipe is for disciplined refactoring: preserving user-visible behavior, API contracts, data semantics, permissions, side effects, and test expectations while reducing complexity, duplication, coupling, or unclear ownership.

## Use When

- code is difficult to understand or maintain
- logic is duplicated
- responsibilities are mixed
- a component or function has become too large
- a shared abstraction already exists but is not being reused
- code needs clearer boundaries before a later feature
- types, naming, or structure can be improved without changing behavior

## Do Not Use When

- behavior is expected to change
- acceptance criteria describe a new user outcome
- the refactor requires a new provider, framework, or architecture
- the existing behavior is not understood
- tests are missing and no reliable behavior contract can be established
- the change includes a schema redesign or migration
- the task is actually bug fixing

## Inputs

- `[REFACTOR_TARGET]`: File, component, function, module, or flow to refactor
- `[MOTIVATION]`: Why the refactor is needed
- `[CURRENT_BEHAVIOR]`: Observable behavior that must remain unchanged
- `[KNOWN_PROBLEMS]`: Duplication, complexity, coupling, naming, or structure issues
- `[PRESERVED_CONTRACTS]`: APIs, props, routes, events, data, permissions, or side effects that must not change
- `[RELEVANT_TESTS]`: Existing tests or checks
- `[CONSTRAINTS]`: Technical, architectural, design, or compatibility limits
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[SUCCESS_SIGNAL]`: What structural improvement should be visible after the refactor

## Prompt

```text
Refactor the following existing code without changing observable behavior.

<target>
Refactor target: [REFACTOR_TARGET]
Motivation: [MOTIVATION]
Known structural problems: [KNOWN_PROBLEMS]
Success signal: [SUCCESS_SIGNAL]
</target>

<behavior_contract>
Current behavior that must remain unchanged:
[CURRENT_BEHAVIOR]

Contracts that must remain stable:
[PRESERVED_CONTRACTS]

Relevant tests or verification:
[RELEVANT_TESTS]

Constraints:
[CONSTRAINTS]

Out of scope:
[OUT_OF_SCOPE]
</behavior_contract>

Your task is to improve the internal structure while preserving the complete external contract.

Do not add features, change UX, alter business rules, rename public contracts, change data semantics, or widen permissions.

## Refactor Contract

A refactor is acceptable only when all of the following remain unchanged unless explicitly listed otherwise:

- user-visible output
- routes and navigation
- component props and public interfaces
- API request and response shapes
- emitted events and callbacks
- validation behavior
- loading, empty, success, and error behavior
- authorization and visibility rules
- data written, read, or derived
- side effects, ordering, and retry behavior
- analytics, notifications, and audit behavior
- accessibility and responsive behavior
- SSR and direct-route behavior
- tests that encode intended behavior

Internal implementation may change. External behavior may not.

## Phase 1: Establish the Behavior Baseline

Before editing:

1. Read Workspace Knowledge and Project Knowledge.
2. Inspect the target and its direct callers and dependencies.
3. Identify the observable contract.
4. Identify tests that protect the current behavior.
5. Identify hidden coupling, side effects, shared state, and timing assumptions.
6. Identify whether the target is reused elsewhere.
7. Identify any behavior that is unclear or unverified.

Create a short behavior baseline containing:

- inputs
- outputs
- state transitions
- side effects
- errors
- permissions
- timing-sensitive behavior
- public contracts
- known callers

Do not infer behavior from one file alone.

## Phase 2: Define Refactor Invariants

Write a compact list of invariants before making changes.

Examples:

- the same inputs produce the same outputs
- the same users may perform the same actions
- the same errors appear under the same conditions
- the same data is written in the same transaction boundary
- callbacks fire in the same order
- the same cache keys and invalidation behavior are preserved
- no new network requests are introduced
- no existing request is removed
- no route, prop, event, or schema contract changes

Use project-specific invariants rather than generic statements.

## Phase 3: Choose the Smallest Structural Change

Prefer one focused refactor goal, such as:

- extract pure logic
- split presentation from business logic
- remove duplication
- reuse an existing utility or component
- reduce nested control flow
- centralize one existing rule
- improve type safety
- clarify naming
- isolate provider-specific code
- separate data access from rendering

Do not combine several unrelated refactors.

Avoid introducing:
- speculative abstractions
- generic frameworks
- unnecessary wrapper layers
- new state containers
- new dependencies
- broad file moves
- naming churn unrelated to the target
- public API changes

If the safest improvement is smaller than the requested refactor, choose the smaller change.

## Phase 4: Refactor in Behavior-Preserving Steps

Apply changes incrementally.

For each change:

- preserve the current contract
- keep the project compilable
- avoid touching unrelated files
- reuse established patterns
- keep side effects at the same trusted boundary
- preserve transaction and authorization boundaries
- preserve error propagation
- preserve async ordering where behavior depends on it
- preserve cache and invalidation behavior
- preserve generated or provider-controlled contracts

When extracting logic:

- keep pure logic pure
- pass dependencies explicitly
- avoid hidden global state
- preserve null, empty, and error semantics
- do not move trusted server logic into the client

When simplifying conditions:

- preserve evaluation order
- preserve short-circuit behavior
- preserve fallback behavior
- preserve distinction between missing, null, empty, false, and zero

When changing types:

- strengthen types without changing runtime behavior
- do not silence errors with `any`
- do not alter serialized shapes
- do not remove defensive checks unless evidence proves they are unreachable

When consolidating duplicated logic:

- verify the copies are behaviorally equivalent
- preserve caller-specific differences
- do not create a shared abstraction that hides meaningful variation

## Phase 5: Diff Review

Before verification, review the diff specifically for accidental behavior change.

Check for:

- changed conditions
- changed defaults
- changed return values
- changed error messages
- changed request order
- changed transaction boundaries
- changed effect timing
- changed dependency arrays
- changed cache keys
- changed permission checks
- changed route or prop contracts
- changed public exports
- changed analytics or logging
- removed edge-case handling
- new network requests
- altered loading or empty states

Remove unrelated formatting or cleanup that obscures the refactor.

## Phase 6: Regression Verification

Verify behavior before and after the refactor.

At minimum:

- run existing tests covering the target
- run build, type, and lint checks where available
- verify direct callers
- verify the main user flow
- verify known edge cases
- inspect console and network behavior

When relevant, also verify:

- allowed and denied access
- loading, empty, success, and error states
- duplicate submissions
- async ordering
- cache invalidation
- SSR and direct-route loading
- mobile and desktop behavior
- provider calls and side effects
- database writes and transaction behavior

When tests are missing:

- add focused characterization tests before or alongside the refactor when practical
- test current behavior, not the desired cleanup
- avoid inventing new behavior through the test

Do not claim behavior was preserved if only compilation succeeded.

## Stop Conditions

Stop and report instead of continuing when:

- current behavior cannot be established reliably
- the refactor requires changing a public contract
- the target hides a real bug that must be fixed separately
- the change requires a schema migration
- authorization must change
- side effects cannot be preserved safely
- callers depend on inconsistent behavior
- the task has grown beyond one focused refactor

In that case, explain what must be separated into:
- bug fix
- migration
- feature change
- compatibility plan
- broader refactor

Do not mix those changes into this refactor.

## Completion Conditions

The refactor is complete only when:

- the structural problem is improved
- observable behavior is preserved
- public contracts remain stable
- permissions and data semantics remain unchanged
- no unrelated changes remain
- relevant regression checks pass
- unverified areas are stated explicitly

## Final Response Format

# Refactored: [REFACTOR_TARGET]

## Structural Improvement
- [WHAT WAS IMPROVED]

## Behavior Preserved
- [CONTRACT OR FLOW]
- [CONTRACT OR FLOW]

## Files Changed
- `[PATH]`: [PURPOSE]

## Public Contract Changes
- None

If any public contract changed, the task was not a pure refactor and must be reported as such.

## Verification
- [CHECK]: [PASS / FAIL / NOT RUN]
- [REGRESSION FLOW]: [PASS / FAIL / NOT RUN]

## Characterization Coverage
- [EXISTING OR ADDED TEST]: [WHAT IT PROTECTS]

## Remaining Risks or Unverified Areas
- [ITEM]
- Write “None identified” only when justified.

Do not include a long retrospective or propose unrelated cleanup.
```

## Expected Output

A behavior-preserving refactor that:

1. improves one structural problem
2. preserves all observable contracts
3. avoids unrelated cleanup
4. keeps authorization and data semantics unchanged
5. uses focused regression verification
6. reports any uncertainty honestly
7. stops when the task is no longer a pure refactor

## Quality Checklist

- [ ] A behavior baseline was established before editing.
- [ ] Refactor invariants were explicit.
- [ ] One focused structural goal was chosen.
- [ ] Public contracts remained unchanged.
- [ ] Authorization and data semantics remained unchanged.
- [ ] Async ordering and side effects were preserved.
- [ ] The final diff was checked for accidental behavior changes.
- [ ] Regression verification covered more than compilation.
- [ ] Missing tests were handled with characterization tests where practical.
- [ ] No bug fix, feature change, or migration was mixed into the refactor.
