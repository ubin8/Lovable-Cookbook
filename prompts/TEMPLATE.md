# Prompt Recipe Template

Use this template for reusable Lovable prompt recipes. Replace placeholders and remove irrelevant sections.

## Recipe

- Name: [RECIPE NAME]
- Category: [PLANNING / IMPLEMENTATION / DEBUGGING / DATABASE / SECURITY / TESTING / DESIGN / INTEGRATION / PERFORMANCE / LAUNCH]
- Recommended mode: [PLAN MODE / AGENT MODE / EITHER]
- Risk level: [LOW / MEDIUM / HIGH]
- Estimated scope: [SMALL / MEDIUM / LARGE]

## Purpose

[Describe the exact outcome this recipe should produce.]

## Use When

- [TRIGGER OR SITUATION]
- [TRIGGER OR SITUATION]
- [TRIGGER OR SITUATION]

## Do Not Use When

- [CASE WHERE ANOTHER RECIPE OR MANUAL REVIEW IS BETTER]
- [CASE WHERE THE SCOPE IS TOO BROAD OR TOO RISKY]

## Required Context

Provide or verify:

- [RELEVANT FILES, ROUTES, COMPONENTS, OR FEATURES]
- [CURRENT BEHAVIOR]
- [DESIRED BEHAVIOR]
- [CONSTRAINTS]
- [KNOWN ERRORS OR EVIDENCE]
- [ACCEPTANCE CRITERIA]

## Variables

- `[VARIABLE]`: [MEANING]
- `[VARIABLE]`: [MEANING]
- `[VARIABLE]`: [MEANING]

## Prompt

```text
You are working inside an existing Lovable project.

Task:
[DESCRIBE THE REQUESTED OUTCOME]

Context:
- Project area: [FEATURE / ROUTE / COMPONENT / FLOW]
- Current behavior: [CURRENT BEHAVIOR]
- Desired behavior: [DESIRED BEHAVIOR]
- Relevant constraints: [CONSTRAINTS]
- Acceptance criteria: [ACCEPTANCE CRITERIA]

Instructions:
1. Inspect the relevant existing files, dependencies, routes, components, data models, migrations, integrations, and conventions before making decisions.
2. Reuse existing components, utilities, schemas, services, and patterns where appropriate.
3. Preserve existing behavior outside the requested scope.
4. Do not introduce new libraries, providers, frameworks, or abstractions unless clearly necessary.
5. Follow Workspace Knowledge, Project Knowledge, the connected design system, and established project conventions.
6. State important assumptions when context cannot be verified.
7. Do not claim testing, security, deployment, or verification without evidence.

Specific requirements:
- [REQUIREMENT]
- [REQUIREMENT]
- [REQUIREMENT]

Do not:
- [PROHIBITED CHANGE]
- [PROHIBITED CHANGE]
- [PROHIBITED CHANGE]

Before implementation:
- Summarize the relevant existing structure.
- Identify affected files, data, permissions, integrations, and risks.
- Propose the smallest safe approach.
- Ask for clarification only when a material ambiguity blocks safe progress.

Implementation:
- Make focused changes.
- Handle relevant loading, empty, success, validation, and error states.
- Enforce authorization and critical validation at a trusted boundary.
- Preserve type safety and established architecture.
- Remove temporary code, mocks, bypasses, and debug output.

Verification:
- Run or describe the relevant build, type, lint, test, browser, database, or security checks.
- Verify the requested behavior and affected existing behavior.
- Test both allowed and denied behavior for protected features.
- State any unverified areas, assumptions, or residual risks.

Final response:
- Summarize what changed.
- List the affected files.
- Report the checks performed and their results.
- Clearly state unresolved issues or limitations.
```

## Expected Output

The response should include:

1. [ANALYSIS OR PLAN]
2. [IMPLEMENTATION OR RECOMMENDATION]
3. [AFFECTED FILES OR SYSTEMS]
4. [VERIFICATION RESULTS]
5. [ASSUMPTIONS, RISKS, OR LIMITATIONS]

## Verification Checklist

- [ ] The requested outcome is addressed.
- [ ] Existing architecture and conventions were inspected.
- [ ] Changes remain within the requested scope.
- [ ] Relevant authorization and data boundaries were reviewed.
- [ ] Loading, empty, success, validation, and error states were considered.
- [ ] Existing and new behavior were verified where possible.
- [ ] No verification claim is made without evidence.
- [ ] Remaining assumptions and risks are stated.

## Adaptation Notes

- Remove sections that do not apply.
- Keep the prompt focused on one clear task.
- Split large requests into separate recipes or phases.
- Put project-specific facts in variables or context, not in the reusable core.
- Prefer explicit acceptance criteria over vague quality language.
- For high-risk changes, require planning and review before implementation.

## Example Variables

- `[FEATURE]`: The feature or flow being changed.
- `[CURRENT_BEHAVIOR]`: What the project does now.
- `[DESIRED_BEHAVIOR]`: The expected result.
- `[CONSTRAINTS]`: Technical, product, design, security, or compatibility limits.
- `[ACCEPTANCE_CRITERIA]`: Observable conditions that define success.
- `[RELEVANT_FILES]`: Known files, routes, components, functions, or migrations.
