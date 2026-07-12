# Planning Prompts

Recipes for clarifying requirements, defining scope, comparing approaches, identifying dependencies and risks, and preparing focused implementation plans before code changes.

Use this category when the primary output is a decision, specification, project analysis, or implementation plan rather than completed code.

## Available recipes

| Recipe | Purpose | Works well with |
| --- | --- | --- |
| [`analyze-existing-project.md`](analyze-existing-project.md) | Build a read-only, evidence-based understanding of an existing Lovable project, its architecture, flows, conventions, risks, and unknowns | `/feature-planner`, `/safe-database-migration`, `/rls-security-review`, `/multi-tenant-isolation-review`, `/production-readiness` |

## Recommended sequence

For unclear or mature projects, use:

1. [`analyze-existing-project.md`](analyze-existing-project.md) to establish evidence and scope.
2. A focused skill or prompt for the actual task.
3. A testing, security, database, performance, or launch recipe to verify the result where relevant.

The analysis recipe does not replace a focused security, migration, isolation, or release review. It prepares reliable context for those workflows.