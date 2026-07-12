# Implementation Prompts

Implementation recipes make focused changes inside an existing Lovable project. Use them only after the requested behavior, boundaries, and acceptance criteria are sufficiently clear.

## Available recipes

| Recipe | Use it to | Works well after |
| --- | --- | --- |
| [`implement-small-feature.md`](implement-small-feature.md) | Implement one narrow, understood feature in a focused pass with minimal surface area and evidence-based verification | `create-acceptance-criteria`, `plan-feature`, or one step from `break-feature-into-steps` |

## Recommended workflow

For a small, understood feature:

1. Define observable behavior with [`create-acceptance-criteria`](../planning/create-acceptance-criteria.md) when needed.
2. Use [`implement-small-feature.md`](implement-small-feature.md).
3. Run the relevant testing or focused review resource for affected risks.

For a larger or riskier feature:

1. Use [`plan-feature`](../planning/plan-feature.md).
2. Use [`break-feature-into-steps`](../planning/break-feature-into-steps.md).
3. Implement one step at a time rather than forcing the entire feature through this recipe.

`implement-small-feature` must stop when inspection reveals a breaking redesign, unresolved authorization model, destructive migration, provider replacement, or several independent flows.
