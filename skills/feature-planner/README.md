# Feature Planner

A structured planning skill for turning an early feature idea into a decision-ready product plan before implementation begins.

> The human-readable documentation lives in this file. The authoritative execution instructions are defined in [`SKILL.md`](SKILL.md).

## Use this skill when

Use `/feature-planner` for meaningful features, larger product changes, or extensions that require product, UX, data, permission, integration, and scope decisions before implementation.

Typical use cases include:

- planning a new feature;
- defining an MVP;
- reviewing whether an idea is worth building;
- clarifying user flows, edge cases, permissions, and failure states;
- comparing a proposed feature with simpler alternatives;
- producing measurable acceptance criteria.

## Do not use it for

- small copy or styling changes;
- direct implementation requests;
- straightforward bug fixes;
- open-ended brainstorming without project context;
- requests for a build or implementation prompt.

## What it produces

The skill produces a Markdown feature plan covering:

1. executive assessment;
2. user problem;
3. scope and MVP boundaries;
4. user experience and states;
5. functional requirements;
6. data and integrations;
7. risks and edge cases;
8. alternatives and trade-offs;
9. acceptance criteria;
10. a clear recommendation and next decision.

## How to use

1. Copy the complete `feature-planner` folder into the environment that supports your skills.
2. Keep the file name `SKILL.md` unchanged.
3. Provide real project context, known constraints, and the intended user outcome.
4. Resolve assumptions and open questions before moving to implementation.
5. Use the resulting plan as input for a separate, bounded implementation workflow.

## Files

| File | Purpose |
| --- | --- |
| [`README.md`](README.md) | Human-readable overview and usage guidance |
| [`SKILL.md`](SKILL.md) | Authoritative skill instructions and output contract |

## Limitations

This skill does not validate market demand, inspect source code, implement changes, or guarantee that a feature is commercially or technically successful. Its output is only as reliable as the supplied project context and evidence.

## Related resources

- [How to use this cookbook](../../guides/how-to-use-this-cookbook.md)
- [Pre-build checklist](../../checklists/pre-build.md)
- [Contributing guide](../../CONTRIBUTING.md)
