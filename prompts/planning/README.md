# Planning Prompts

Planning recipes turn uncertain work into evidence, behavioral contracts, impact assessments, implementation plans, and safe execution sequences. They do not modify the project.

## Available recipes

| Recipe | Use it to | Useful next resource |
| --- | --- | --- |
| [`analyze-existing-project.md`](analyze-existing-project.md) | Understand an existing project's architecture, flows, conventions, risks, and unknowns | `create-acceptance-criteria`, `plan-feature`, or a focused review skill |
| [`create-acceptance-criteria.md`](create-acceptance-criteria.md) | Convert a request into precise, observable success and failure conditions | `plan-feature`, `implement-small-feature`, or a testing prompt |
| [`assess-change-impact.md`](assess-change-impact.md) | Trace direct, indirect, compatibility, security, data, and operational impact before changing existing behavior | `plan-feature`, `break-feature-into-steps`, or the relevant risk-review skill |
| [`plan-feature.md`](plan-feature.md) | Produce an implementation-ready plan grounded in the current project | `break-feature-into-steps` or a focused implementation prompt |
| [`break-feature-into-steps.md`](break-feature-into-steps.md) | Split an approved feature into small, ordered, independently verifiable steps | Implement each step separately, then test and review |

## Choose the smallest useful workflow

### Vague request

1. [`create-acceptance-criteria.md`](create-acceptance-criteria.md)
2. [`plan-feature.md`](plan-feature.md)
3. [`break-feature-into-steps.md`](break-feature-into-steps.md) only when the plan is too large for one focused implementation

### Existing project or unclear feature area

1. [`analyze-existing-project.md`](analyze-existing-project.md)
2. [`create-acceptance-criteria.md`](create-acceptance-criteria.md) when expected behavior is still ambiguous
3. [`plan-feature.md`](plan-feature.md)
4. [`break-feature-into-steps.md`](break-feature-into-steps.md) when staged delivery adds value

### Change to existing behavior

1. [`assess-change-impact.md`](assess-change-impact.md)
2. [`create-acceptance-criteria.md`](create-acceptance-criteria.md) when the new contract needs clarification
3. [`plan-feature.md`](plan-feature.md) or [`break-feature-into-steps.md`](break-feature-into-steps.md)

## Focused review skills

Planning prompts prepare context but do not replace specialized reviews:

- database or migration risk: [`/safe-database-migration`](../../skills/safe-database-migration/README.md)
- authorization or exposed-data risk: [`/rls-security-review`](../../skills/rls-security-review/README.md)
- tenant-boundary risk: [`/multi-tenant-isolation-review`](../../skills/multi-tenant-isolation-review/README.md)
- release risk: [`/production-readiness`](../../skills/production-readiness/README.md)

Do not run every recipe by default. Add a step only when it produces a distinct artifact, resolves material ambiguity, or reviews a separate risk.
