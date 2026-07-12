# Implementation Prompts

Implementation recipes make focused changes inside an existing Lovable project. Use them only after the requested behavior, boundaries, and acceptance criteria are sufficiently clear.

## Available recipes

| Recipe | Use it to | Works well after |
| --- | --- | --- |
| [`implement-small-feature.md`](implement-small-feature.md) | Implement one narrow, understood feature in a focused pass with minimal surface area | `create-acceptance-criteria`, `plan-feature`, or one step from `break-feature-into-steps` |
| [`extend-existing-flow.md`](extend-existing-flow.md) | Add one branch, step, state, role, outcome, or side effect while preserving the original flow | `analyze-existing-project`, `assess-change-impact`, `create-acceptance-criteria`, or `plan-feature` |
| [`refactor-without-behavior-change.md`](refactor-without-behavior-change.md) | Improve internal structure while preserving observable behavior and public contracts | `analyze-existing-project`, characterization tests, or `assess-change-impact` for shared code |
| [`add-error-and-empty-states.md`](add-error-and-empty-states.md) | Add explicit loading, empty, permission, partial-failure, error, and recovery states to an existing feature | `create-acceptance-criteria`, an existing stable data contract, and relevant testing prompts |

## Which recipe should you use?

Use **`implement-small-feature`** when the request adds one small, clearly defined outcome and the affected area is limited.

Use **`extend-existing-flow`** when a working flow needs one additional branch or step and preserving the existing path is central to the task.

Use **`refactor-without-behavior-change`** when the desired result is structural improvement only. Do not use it for bug fixes, changed UX, migrations, or new behavior.

Use **`add-error-and-empty-states`** when the underlying feature and data contract already work, but user-visible states are missing, conflated, generic, or unrecoverable.

## Recommended workflows

### Small feature

1. Define observable behavior with [`create-acceptance-criteria`](../planning/create-acceptance-criteria.md) when needed.
2. Use [`implement-small-feature.md`](implement-small-feature.md).
3. Run the relevant testing or focused review resource.

### Extend a working flow

1. Use [`assess-change-impact`](../planning/assess-change-impact.md) when shared contracts or downstream effects are unclear.
2. Define the new branch with [`create-acceptance-criteria`](../planning/create-acceptance-criteria.md).
3. Use [`extend-existing-flow.md`](extend-existing-flow.md).
4. Verify both the original and new paths.

### Behavior-preserving refactor

1. Establish the current behavior and callers, using [`analyze-existing-project`](../planning/analyze-existing-project.md) when necessary.
2. Add or identify characterization coverage.
3. Use [`refactor-without-behavior-change.md`](refactor-without-behavior-change.md).
4. Run regression verification beyond compilation.

### Improve feature states

1. Confirm the data contract can distinguish loading, empty, denied, not-found, partial, and failed states.
2. Use [`add-error-and-empty-states.md`](add-error-and-empty-states.md).
3. Verify each applicable state deliberately, including safe retry and permission-sensitive actions.

## Stop and replan when

An implementation recipe should stop when inspection reveals a breaking redesign, unresolved authorization or state model, destructive migration, provider replacement, unstable data contract, several independent flows, or behavior that cannot be verified reliably.
