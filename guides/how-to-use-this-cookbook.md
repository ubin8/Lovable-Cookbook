# How to use this cookbook

The artifacts in this repository are starting points, not substitutes for project context or judgment.

## 1. Choose the right artifact

- Use a **skill** for a repeatable role or multi-step workflow.
- Use a **guide** to understand a Lovable workflow or technical decision.
- Use a **template** to create a consistent project document or recipe.
- Use a **checklist** as a quality gate before implementation or release.
- Use an **example** to see how several artifacts work together.

Do not use a complex skill when a direct instruction would be enough. More process is not automatically better.

## 2. Supply real context

Useful context normally includes:

- the target user and their problem;
- the current product state;
- the desired outcome;
- scope and explicit non-goals;
- relevant routes, components, data, and integrations;
- authentication and permission rules;
- data sensitivity;
- constraints such as budget, time, compatibility, or regulation;
- observable acceptance criteria.

Never invent missing facts merely to make a prompt look complete. Mark assumptions explicitly.

## 3. Separate planning from implementation

For significant changes, establish what should be built and why before asking Lovable to implement it.

Combining discovery, design, architecture, implementation, and validation into one vague request creates hidden assumptions and makes failures harder to diagnose.

## 4. Prefer observable requirements

“Make it modern” is subjective. “Use a two-column desktop layout, collapse to one column below 768 px, and keep the primary action visible without horizontal scrolling” can be reviewed.

Requirements should describe behavior, states, constraints, and outcomes rather than relying on taste or undefined quality words.

## 5. Work in bounded changes

Prefer one coherent change at a time:

1. define the outcome;
2. inspect the current state;
3. plan the change;
4. implement a bounded slice;
5. test observable behavior;
6. review security, accessibility, and regressions;
7. document the result.

## 6. Verify generated work

Generated output is not automatically production-ready. Depending on the change, verify:

- happy paths and failure paths;
- loading, empty, and permission states;
- authentication and authorization boundaries;
- database constraints and migrations;
- responsive behavior and accessibility;
- secrets and environment configuration;
- external service failures;
- analytics and error reporting;
- rollback or recovery options.

## 7. Adapt, do not cargo-cult

A recipe should be shortened, expanded, or rejected when it does not fit the project. Copying every section without understanding it creates ceremony, not quality.

## Recommended first workflow

For a non-trivial feature:

1. Run [`/feature-planner`](../skills/feature-planner/SKILL.md).
2. Resolve its assumptions and choose the MVP scope.
3. Apply the [pre-build checklist](../checklists/pre-build.md).
4. Ask Lovable to implement one bounded increment.
5. Test and review the result.
6. Apply the [pre-launch checklist](../checklists/pre-launch.md) before release.