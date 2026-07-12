# Add Error and Empty States

## Recipe

- Category: Implementation
- Recommended mode: Agent Mode
- Risk level: Low to Medium
- Estimated scope: Small to Medium

## Purpose

Add clear, recoverable, and context-aware empty and error states to an existing Lovable feature without changing its core business behavior.

This recipe is specifically for state design and implementation. It focuses on distinguishing true emptiness from loading, permission denial, filtering, partial data, and failure. It also ensures that recovery actions are meaningful, authorized, and consistent with the existing product.

## Use When

- a page or component renders blank content
- errors are swallowed, generic, or only visible in the console
- loading, empty, and error states are conflated
- a list, dashboard, search, table, form, or detail view needs clearer state handling
- the user has no recovery path after failure
- filtered results and truly empty datasets look the same
- partial failures need to remain usable
- retry behavior is missing or unsafe

## Do Not Use When

- the underlying feature is functionally broken and needs debugging first
- the business rule for what counts as “empty” is undefined
- the task requires redesigning the entire feature
- the primary issue is authorization architecture
- the request is only about visual styling
- the data source or contract is still unstable

## Inputs

- `[TARGET]`: Route, page, component, or flow
- `[DATA_SOURCE]`: Query, API, loader, provider, or local state
- `[USER_GOAL]`: What the user is trying to accomplish
- `[CURRENT_STATES]`: Existing loading, empty, and error behavior
- `[EXPECTED_EMPTY_CASES]`: Legitimate empty scenarios
- `[EXPECTED_ERROR_CASES]`: Known failure scenarios
- `[RECOVERY_ACTIONS]`: Valid user recovery options
- `[PERMISSIONS]`: Role, tenant, ownership, or visibility rules
- `[DESIGN_CONSTRAINTS]`: Existing components, tone, layout, or content rules
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[VERIFICATION]`: Required checks or flows

## Prompt

```text
Add robust error and empty states to the following existing Lovable feature.

<target>
Target: [TARGET]
Data source: [DATA_SOURCE]
User goal: [USER_GOAL]
Current behavior: [CURRENT_STATES]
Expected empty cases: [EXPECTED_EMPTY_CASES]
Expected error cases: [EXPECTED_ERROR_CASES]
Valid recovery actions: [RECOVERY_ACTIONS]
Permissions and visibility: [PERMISSIONS]
Design constraints: [DESIGN_CONSTRAINTS]
Out of scope: [OUT_OF_SCOPE]
Required verification: [VERIFICATION]
</target>

Do not redesign the feature, change its business rules, replace the data layer, or add unrelated states.

Your task is to make every relevant state explicit, distinguishable, accessible, and recoverable where appropriate.

## State Model

Before editing, inspect the target and define the actual state model.

At minimum, distinguish where relevant:

- initial loading
- background refresh
- first-time empty
- filtered empty
- search with no matches
- permission-restricted
- not found
- partial data
- partial failure
- recoverable error
- non-recoverable error
- stale data with failed refresh
- success with content

Do not map all non-success outcomes to one generic “Something went wrong” message.

State must be derived from real conditions, not guesswork.

## Classification Rules

Use these distinctions:

### Initial Loading
The user has no usable content yet and the first request is still in progress.

### Background Refresh
Usable content already exists while newer data is loading.

Do not replace existing content with a full-screen loading state during background refresh unless the current product convention requires it.

### First-Time Empty
The source loaded successfully, no records exist, and the user has not created or received content yet.

### Filtered Empty
Records may exist, but the current filters exclude all results.

### Search Empty
The search completed successfully but returned no matches.

### Permission-Restricted
The user is authenticated but cannot view or act on the resource.

Do not present restricted data as if it does not exist when the product needs to communicate access denial.

### Not Found
The requested resource does not exist or is no longer available.

Do not use “not found” to conceal an authorization bug.

### Partial Failure
Some data or actions are available, while another part failed.

Keep usable content visible when safe.

### Recoverable Error
The operation failed, but retry, correction, or navigation can reasonably succeed.

### Non-Recoverable Error
The user cannot resolve the issue in the current context.

Provide a safe next action such as returning, contacting support, or choosing another resource.

## Inspection

Before implementation:

1. Read Workspace Knowledge and Project Knowledge.
2. Inspect the target component, route, loader, query, mutation, and data contract.
3. Identify the current source of truth for status and errors.
4. Identify whether errors are thrown, returned, swallowed, logged, or transformed.
5. Inspect existing shared state components and content patterns.
6. Inspect retry behavior and duplicate-submit protection.
7. Inspect permission checks and not-found handling.
8. Inspect tests covering the target.

Trace the state path:

request starts
→ loading state
→ response or failure
→ state classification
→ visible message
→ available action
→ retry or navigation result

## Implementation Rules

### Use Existing Patterns

- reuse existing alert, empty-state, skeleton, button, icon, card, and layout components
- follow established spacing, typography, and content tone
- preserve existing route, data-fetching, cache, and state-management conventions
- do not add a new state library or global error system for one feature

### Empty States

Each empty state must answer:

- what is missing
- why it may be empty
- what the user can do next
- whether the user has permission to perform that action

Requirements:

- first-time empty states may include a primary creation or setup action
- filtered empty states should offer filter reset or adjustment
- search empty states should preserve the query and suggest a next step
- read-only users must not see creation actions they cannot perform
- empty-state actions must use the same trusted authorization as the underlying operation
- avoid decorative empty states that hide a data or permission bug
- do not claim “No data exists” when the query may have failed

### Error States

Each error state must define:

- what failed
- whether existing data is still usable
- whether retry is safe
- whether the user can correct the input
- whether support or escalation is appropriate

Requirements:

- preserve safe, usable content during partial failure
- show field-level validation near the relevant input
- show operation-level errors near the affected action or result
- use page-level errors only when the page cannot function
- do not expose stack traces, provider payloads, tokens, internal IDs, or sensitive implementation details
- do not silently swallow errors
- do not convert permission failures into generic network errors
- do not retry non-idempotent operations automatically without safeguards
- preserve user input after recoverable submission failure where practical

### Retry Behavior

Retry must be explicit and safe.

Before adding retry:

- determine whether the operation is idempotent
- prevent concurrent duplicate retries
- disable or show progress while retrying
- preserve existing content where safe
- clear stale error state only when a new attempt begins
- avoid infinite retry loops
- use bounded retry behavior for external calls
- do not retry authorization, validation, or forbidden-state failures as if they were transient

### Content

Use concise, specific copy.

Prefer:
- “No contacts match these filters.”
- “We couldn’t load the latest activity. Existing activity is still shown.”
- “This record is no longer available.”
- “You don’t have permission to export this report.”

Avoid:
- “Oops!”
- “Something went wrong.”
- “No data.”
- “Try again later.”
- “Unknown error.”

Every message should match the actual state.

### Accessibility

- announce meaningful async error and success changes where appropriate
- preserve focus after failed actions
- move focus only when it helps recovery
- ensure retry and recovery controls are keyboard accessible
- do not communicate error state by color alone
- keep headings and landmarks meaningful
- ensure skeletons and spinners do not create misleading screen-reader output

### Security and Privacy

- do not reveal whether protected records exist when the product requires concealment
- do not expose another user’s or tenant’s data in fallback content, cached state, or error details
- keep authorization enforcement at a trusted boundary
- ensure empty-state actions respect role, ownership, tenant, and resource scope
- avoid logging sensitive user input or provider errors unnecessarily

## Partial Failure Patterns

Use the least disruptive safe pattern.

### Existing content + refresh failure
Keep existing content visible and show a non-blocking refresh error.

### Multi-panel page
Fail only the affected panel unless the page contract requires all data.

### Batch action
Report successful and failed items separately.

### Form submission
Preserve entered values and show actionable validation or operation errors.

### Provider dependency failure
Explain which capability is unavailable without misrepresenting the rest of the feature.

Do not turn a local failure into a full-page outage without evidence.

## Self-Review

Before verification, review every state and ask:

- can loading be mistaken for empty?
- can empty be caused by failure?
- can permission denial be mistaken for not found?
- can stale content be mistaken for current content?
- can retry duplicate a mutation?
- can an unauthorized user see an action?
- can a partial failure hide usable content?
- can the message expose sensitive details?
- can the user recover without losing work?
- does the state use the existing design language?

Remove duplicated messages, generic fallbacks, and unreachable branches.

## Verification

Verify every applicable state deliberately.

### Empty-state checks

- first-time empty
- filtered empty
- search empty
- read-only empty
- empty after deletion or archive
- empty with creation permission
- empty without creation permission

### Error-state checks

- initial load failure
- background refresh failure
- validation failure
- mutation failure
- partial failure
- permission denied
- not found
- provider timeout
- retry success
- repeated retry prevention

### UI checks

- loading does not flash incorrectly
- existing content remains visible during background refresh
- focus is preserved after failure
- messages are specific and accurate
- recovery actions are authorized
- mobile and desktop layouts remain usable
- direct-route and SSR behavior work where applicable

Do not claim a state was verified unless it was explicitly reproduced or covered by a relevant test.

## Stop Conditions

Stop and report instead of masking the problem when:

- the data contract cannot distinguish empty from failure
- authorization behavior is inconsistent
- the query returns unauthorized data that is only hidden in the UI
- retry may duplicate irreversible work
- the feature needs a broader error architecture
- the task requires changing business rules
- the underlying failure is unknown and needs debugging first

Do not create polished states on top of an unsafe or incorrect data flow.

## Completion Conditions

The task is complete only when:

- relevant states are explicit and distinguishable
- messages match actual conditions
- recovery actions are safe and authorized
- partial failures preserve usable content where appropriate
- retries cannot create accidental duplicates
- sensitive details are not exposed
- acceptance criteria and required state checks are verified
- unverified states are reported honestly

## Final Response Format

# Added Error and Empty States: [TARGET]

## States Added or Updated
- [STATE]: [BEHAVIOR]
- [STATE]: [BEHAVIOR]

## Recovery Actions
- [ACTION]: [WHEN AVAILABLE]

## Files Changed
- `[PATH]`: [PURPOSE]

## Authorization or Data Changes
- [CHANGE]
- Write “None” when none were required.

## Verification
- [STATE OR FLOW]: [PASS / FAIL / NOT RUN]

## Remaining Gaps
- [ITEM]
- Write “None identified” only when justified.

Do not repeat the entire state model in the final response.
```

## Expected Output

A focused implementation that:

1. distinguishes loading, empty, permission, not-found, partial, and failure states
2. provides safe recovery actions
3. preserves usable content during partial failure
4. prevents unsafe retry behavior
5. follows existing design and data patterns
6. respects authorization and privacy boundaries
7. verifies each relevant state deliberately

## Quality Checklist

- [ ] Empty and error states are not conflated.
- [ ] Filtered empty and true empty are distinct.
- [ ] Permission denial and not-found behavior are intentional.
- [ ] Partial failures preserve usable content where safe.
- [ ] Retry behavior is idempotent or protected.
- [ ] Messages are specific and actionable.
- [ ] Recovery actions respect permissions.
- [ ] Sensitive details are not exposed.
- [ ] Accessibility and focus behavior were considered.
- [ ] Each implemented state was verified.
