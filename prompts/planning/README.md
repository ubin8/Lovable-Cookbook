# Planning Prompts

Recipes for clarifying requirements, defining scope, analyzing existing projects, sequencing work, and preparing implementation-ready plans before code changes.

Use this category when the primary output is a decision, specification, project analysis, feature plan, or staged execution plan rather than completed code.

## Available recipes

| Recipe | Purpose | Works well with |
| --- | --- | --- |
| [`analyze-existing-project.md`](analyze-existing-project.md) | Build a read-only, evidence-based understanding of an existing Lovable project, its architecture, flows, conventions, risks, and unknowns | `plan-feature`, `/feature-planner`, `/safe-database-migration`, `/rls-security-review`, `/multi-tenant-isolation-review`, `/production-readiness` |
| [`plan-feature.md`](plan-feature.md) | Create an implementation-ready plan for one feature, including scope, functional behavior, affected systems, data, permissions, phases, risks, and verification | `analyze-existing-project`, `break-feature-into-steps`, `/feature-planner`, `/safe-database-migration`, `/rls-security-review`, `/multi-tenant-isolation-review` |
| [`break-feature-into-steps.md`](break-feature-into-steps.md) | Turn an approved feature plan into small, ordered, independently verifiable implementation steps with dependency and rollback boundaries | `plan-feature`, `/safe-database-migration`, `/rls-security-review`, `/multi-tenant-isolation-review`, later implementation and testing prompts |

## Recommended sequences

### Existing project with unclear structure

1. [`analyze-existing-project.md`](analyze-existing-project.md)
2. [`plan-feature.md`](plan-feature.md)
3. [`break-feature-into-steps.md`](break-feature-into-steps.md) when the plan is too large for one focused implementation
4. Implement and verify each step separately

### Feature area already understood

1. [`plan-feature.md`](plan-feature.md)
2. [`break-feature-into-steps.md`](break-feature-into-steps.md) only when staged delivery adds value
3. Use the relevant implementation, database, security, testing, integration, performance, or launch resource

### High-risk feature

Use the planning prompts to establish context and sequencing, then apply the focused review skill for the primary risk:

- database or migration risk: [`/safe-database-migration`](../../skills/safe-database-migration/README.md)
- authorization or exposed-data risk: [`/rls-security-review`](../../skills/rls-security-review/README.md)
- tenant-boundary risk: [`/multi-tenant-isolation-review`](../../skills/multi-tenant-isolation-review/README.md)
- release risk: [`/production-readiness`](../../skills/production-readiness/README.md)

The planning recipes do not replace these focused reviews. They produce the evidence, specification, and sequence those reviews need.
