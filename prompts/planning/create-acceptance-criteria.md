# Create Acceptance Criteria

## Recipe

- Category: Planning
- Recommended mode: Plan Mode
- Risk level: Low to Medium
- Estimated scope: Small to Medium

## Purpose

Turn a feature request, product idea, bug fix, or workflow change into precise, observable acceptance criteria that define what “done” means.

This recipe is not for architecture planning or implementation. Its job is to remove ambiguity, expose missing product decisions, define edge cases, and create a testable contract between product intent and implementation.

## Use When

- a request is vague or outcome-focused
- different people may interpret the feature differently
- implementation should not begin before expected behavior is agreed
- a bug fix needs clear regression criteria
- a flow has important permission, state, validation, or failure behavior
- a feature plan needs measurable completion conditions
- QA or browser testing needs a reliable test basis

## Do Not Use When

- the feature itself is not yet understood
- the main decision is architectural rather than behavioral
- the request is only to explore ideas
- implementation details are already fixed but test cases are needed
- the user wants a complete test plan rather than acceptance criteria

## Inputs

- `[REQUEST]`: Original feature, change, or bug-fix request
- `[PRIMARY_ACTOR]`: Main user or role
- `[USER_GOAL]`: Outcome the actor wants
- `[CURRENT_BEHAVIOR]`: What happens now
- `[DESIRED_BEHAVIOR]`: What should happen instead
- `[BUSINESS_RULES]`: Known domain rules
- `[PERMISSIONS]`: Relevant roles, ownership, tenant, or visibility rules
- `[STATE_MODEL]`: Relevant statuses or transitions
- `[ERROR_CASES]`: Known failures or edge cases
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[QUALITY_REQUIREMENTS]`: Accessibility, responsiveness, performance, reliability, or compliance constraints

## Prompt

```text
Create acceptance criteria for the following request.

Do not design the architecture, choose libraries, write implementation steps, or modify the project. Focus only on externally observable behavior, business rules, permissions, state transitions, validation, and completion conditions.

<request>
[REQUEST]
</request>

<context>
Primary actor: [PRIMARY_ACTOR]
User goal: [USER_GOAL]
Current behavior: [CURRENT_BEHAVIOR]
Desired behavior: [DESIRED_BEHAVIOR]
Known business rules: [BUSINESS_RULES]
Permissions and visibility: [PERMISSIONS]
Relevant states or transitions: [STATE_MODEL]
Known errors or edge cases: [ERROR_CASES]
Out of scope: [OUT_OF_SCOPE]
Quality requirements: [QUALITY_REQUIREMENTS]
</context>

Your task is to convert the request into a precise behavioral contract.

First, identify ambiguity. Distinguish between:

- behavior that is explicitly required
- behavior strongly implied by the request
- behavior that remains undecided
- implementation detail that should not appear in acceptance criteria

Do not silently invent product decisions.

Then create acceptance criteria that are:

- observable by a user, tester, or system boundary
- specific enough to pass or fail
- written in product and domain language
- independent of a particular implementation where possible
- complete across success, validation, permission, state, and failure behavior
- limited to the requested scope
- non-duplicative

Use the following rule:

A criterion is valid only when someone can determine whether it passed without asking what the author meant.

Cover only the dimensions that apply.

<behavior_dimensions>

1. Entry and preconditions
   - who may start the flow
   - required state, data, membership, ownership, or prior action
   - unavailable or disabled conditions

2. Primary success behavior
   - action taken
   - resulting state
   - data created or changed
   - user-visible confirmation
   - downstream effect

3. Validation
   - required fields
   - allowed formats or ranges
   - invalid combinations
   - duplicate prevention
   - business-rule validation
   - where the user receives feedback

4. Permissions and visibility
   - who may view, create, edit, approve, delete, export, or trigger the action
   - own-resource, team, tenant, and global boundaries
   - denied behavior
   - information that must remain hidden

5. State transitions
   - allowed source states
   - allowed target states
   - actor allowed to trigger each transition
   - required fields or conditions
   - forbidden transitions
   - repeat-action behavior

6. Failure and recovery
   - validation failure
   - backend failure
   - provider failure
   - timeout or retry
   - partial failure
   - duplicate submission
   - user recovery path

7. Empty and loading states
   - initial loading
   - no results or no data
   - disabled state
   - progress indication for long-running work

8. Side effects
   - notifications
   - emails
   - jobs
   - audit records
   - billing effects
   - inventory changes
   - external integrations
   - cache or realtime updates

9. Data integrity
   - values that must remain unchanged
   - source of truth
   - atomicity expectations
   - duplicate or idempotency behavior
   - retention or deletion effects

10. Quality constraints
   - responsive behavior
   - accessibility
   - performance threshold
   - security-sensitive behavior
   - localization or formatting
   - observability where user impact depends on it

</behavior_dimensions>

<writing_rules>

Write each acceptance criterion as one of these forms:

Preferred default:
- Given [precondition]
- When [action or event]
- Then [observable result]

Use a concise declarative statement when Given/When/Then would add noise.

Good criteria:
- describe one primary behavior
- use exact actors and domain terms
- define the result, not the internal code
- include negative behavior when access or state is restricted
- make hidden assumptions explicit
- separate independent outcomes

Avoid criteria that:
- use vague words such as “works,” “properly,” “fast,” “user-friendly,” or “secure”
- describe implementation files, hooks, tables, libraries, or components unless they are part of the external contract
- combine several unrelated behaviors
- merely restate the request
- require subjective visual judgment without a concrete standard
- claim compliance or security without a measurable condition
- say “handle errors” without defining the expected behavior

Do not convert every sentence into Given/When/Then mechanically. Optimize for clarity.
</writing_rules>

<ambiguity_handling>

For each unresolved product decision:

- explain why it matters
- list the smallest set of viable choices
- show which criteria depend on the answer
- do not choose silently

Mark it as:
- Blocking: implementation cannot safely begin
- Non-blocking: a reasonable default can be documented later

Ask at most the minimum number of questions required to resolve blocking ambiguity.
</ambiguity_handling>

<output_format>

# Acceptance Criteria: [FEATURE OR CHANGE]

## 1. Outcome

- Primary actor:
- User goal:
- Successful outcome:
- Explicitly out of scope:

## 2. Assumptions

List only assumptions required to write the criteria.

For each assumption mark:
- Confirmed
- Inferred
- Unresolved

## 3. Acceptance Criteria

Group criteria by behavior, not by technical layer.

### A. Entry and Preconditions

AC-01  
Given ...  
When ...  
Then ...

### B. Primary Flow

AC-02  
Given ...  
When ...  
Then ...

### C. Validation

AC-03  
...

### D. Permissions and Visibility

AC-04  
...

### E. State Transitions

AC-05  
...

### F. Failure and Recovery

AC-06  
...

### G. Side Effects and Data Integrity

AC-07  
...

### H. Quality Requirements

AC-08  
...

Remove groups that do not apply.

## 4. Negative Acceptance Criteria

List behavior that must not occur.

Use this format:

NAC-01  
Given ...  
When ...  
Then the system must not ...

Include only meaningful forbidden outcomes such as:
- unauthorized access
- cross-tenant visibility
- duplicate creation
- silent data loss
- forbidden state transitions
- incorrect billing or inventory effects
- exposure of sensitive data

## 5. Acceptance Criteria Matrix

| ID | Actor | Scenario | Expected Result | Priority | Source |
|---|---|---|---|---|---|
| AC-01 | [ACTOR] | [SCENARIO] | [RESULT] | MUST / SHOULD / COULD | EXPLICIT / INFERRED |

Every criterion must appear once in the matrix.

## 6. Blocking Questions

For each blocking question include:

- Question
- Why it matters
- Criteria affected
- Available choices

Write “None” when the criteria are implementation-ready.

## 7. Definition of Ready

The feature is ready for implementation only when:

- all MUST criteria are unambiguous
- blocking questions are resolved
- actor and permission boundaries are defined
- relevant state transitions are defined
- success and failure outcomes are observable
- out-of-scope behavior is explicit

## 8. Coverage Check

Report whether the criteria cover:

- primary success path
- alternate path
- validation
- permissions
- forbidden behavior
- state transitions
- loading/empty behavior
- failure/recovery
- side effects
- data integrity
- applicable quality constraints

Mark each as:
- Covered
- Not applicable
- Missing decision

</output_format>

<quality_bar>

Before finalizing, inspect every criterion and remove or rewrite it when:

- two readers could reasonably interpret it differently
- it cannot be tested
- it mixes independent behaviors
- it depends on an unstated product decision
- it describes implementation instead of outcome
- it duplicates another criterion
- it extends beyond the requested scope

The final set should be complete but not bloated. Prefer fewer strong criteria over many weak ones.
</quality_bar>
```

## Expected Output

A testable behavioral contract that:

1. defines the successful user outcome
2. separates explicit requirements from assumptions
3. covers relevant validation, permissions, states, failures, and side effects
4. includes meaningful negative criteria
5. exposes blocking product decisions
6. avoids implementation details
7. is ready to feed into planning, implementation, or testing prompts

## Quality Checklist

- [ ] Every criterion has a clear pass/fail outcome.
- [ ] Actors and permissions are explicit.
- [ ] Product language is used consistently.
- [ ] Independent behaviors are separated.
- [ ] Negative behavior is included where risk exists.
- [ ] State transitions are explicit where applicable.
- [ ] Side effects and data integrity are covered.
- [ ] Vague quality language has been replaced with measurable behavior.
- [ ] Implementation details are excluded.
- [ ] Blocking questions are minimal and specific.
- [ ] Out-of-scope behavior is explicit.
- [ ] No two criteria describe the same outcome.
