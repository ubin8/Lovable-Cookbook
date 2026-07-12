# Contributing

## Contribution principles

A contribution should be reusable, explicit about its limits, and easy to verify. Avoid vague instructions, inflated claims, and recipes that depend on hidden context.

## Before opening a pull request

- Confirm the problem is recurring rather than project-specific.
- State when the recipe should and should not be used.
- List required inputs and assumptions.
- Define the expected output structure.
- Include validation steps or acceptance criteria.
- Remove secrets, personal data, private URLs, and proprietary content.

## Recommended workflow

1. Open an issue describing the problem and proposed artifact.
2. Copy the closest template from `templates/`.
3. Add one focused contribution per pull request.
4. Explain how the contribution was tested.
5. Update the README or index when adding a new top-level artifact.

## Naming

Use lowercase kebab-case for folders and files where practical.

Examples:

- `skills/feature-planner/SKILL.md`
- `guides/supabase-auth.md`
- `checklists/pre-launch.md`

## Quality bar

A contribution may be rejected when it:

- merely restates a user request;
- lacks clear boundaries;
- hides important assumptions;
- encourages implementation before planning;
- makes unsafe security or privacy recommendations;
- duplicates an existing artifact without a meaningful improvement.
