# Prompt Recipes

Prompt recipes are focused, reusable instructions for recurring Lovable tasks. Each recipe should solve one clear problem, state the required context and boundaries, and define how the result is verified.

Use [`TEMPLATE.md`](TEMPLATE.md) when creating a new recipe.

## Categories

| Category | Purpose |
| --- | --- |
| [`planning/`](planning/) | Scope features, clarify requirements, compare approaches, analyze existing projects, and prepare safe implementation plans |
| [`implementation/`](implementation/) | Implement focused product and engineering changes inside an existing project |
| [`debugging/`](debugging/) | Reproduce failures, gather evidence, isolate causes, and propose or apply targeted fixes |
| [`database/`](database/) | Work with schemas, migrations, queries, constraints, data integrity, and PostgreSQL behavior |
| [`security/`](security/) | Review or improve authorization, data exposure, secrets, tenant boundaries, and trusted operations |
| [`testing/`](testing/) | Create, improve, or execute checks for critical behavior and failure cases |
| [`design/`](design/) | Improve interfaces, interaction patterns, accessibility, responsiveness, and design-system consistency |
| [`integrations/`](integrations/) | Add or review APIs, OAuth, webhooks, provider accounts, jobs, and external services |
| [`performance/`](performance/) | Measure and address rendering, network, bundle, media, query, and Core Web Vitals problems |
| [`launch/`](launch/) | Prepare releases, verify readiness, review deployment risks, and validate post-launch behavior |

## Available recipes

| Recipe | Category | Purpose | Recommended combinations |
| --- | --- | --- | --- |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | Planning | Build a read-only, evidence-based map of an existing project's architecture, flows, conventions, risks, and unknowns | Use before `plan-feature` or a focused review skill when the project is not sufficiently understood |
| [`plan-feature`](planning/plan-feature.md) | Planning | Produce an implementation-ready plan for one feature, grounded in the current project and mapped to concrete verification | Use after `analyze-existing-project` when needed; follow with `break-feature-into-steps` for large or risky features |
| [`break-feature-into-steps`](planning/break-feature-into-steps.md) | Planning | Convert an approved feature plan into small, ordered, independently verifiable implementation steps | Use after `plan-feature`; combine with focused database, security, tenant, implementation, and testing resources as required |

## Combining prompts and skills

Prompts and skills have different roles:

- A **prompt recipe** handles one focused task or output.
- A **skill** provides a deeper reusable workflow with stronger trigger rules, boundaries, references, and output contracts.
- Workspace Knowledge and Project Knowledge provide persistent context; they are not substitutes for either.

Prefer a sequence instead of combining multiple large instructions into one prompt:

1. Establish context with a planning or analysis recipe.
2. Define the feature or change with the focused planning prompt or skill.
3. Break large plans into separately verifiable steps.
4. Use a focused implementation or review resource for each step.
5. Verify the affected risk areas before launch.

Current useful combinations:

| Start with | Continue with | Why |
| --- | --- | --- |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`plan-feature`](planning/plan-feature.md) | Converts verified project evidence into a concrete feature specification and technical plan |
| [`plan-feature`](planning/plan-feature.md) | [`break-feature-into-steps`](planning/break-feature-into-steps.md) | Converts a broad approved plan into focused implementation units with checkpoints |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/feature-planner`](../skills/feature-planner/README.md) | Supplies evidence and existing-project context to the deeper planning workflow |
| [`plan-feature`](planning/plan-feature.md) | [`/safe-database-migration`](../skills/safe-database-migration/README.md) | Reviews concrete schema and migration implications before implementation |
| [`plan-feature`](planning/plan-feature.md) | [`/rls-security-review`](../skills/rls-security-review/README.md) | Reviews the proposed authorization, RLS, storage, RPC, and privileged-operation design |
| [`plan-feature`](planning/plan-feature.md) | [`/multi-tenant-isolation-review`](../skills/multi-tenant-isolation-review/README.md) | Validates tenant scoping across the proposed feature's full data and operational surface |
| [`break-feature-into-steps`](planning/break-feature-into-steps.md) | [`/safe-database-migration`](../skills/safe-database-migration/README.md) | Refines risky data steps into compatible migration, rollout, validation, and cleanup boundaries |
| [`break-feature-into-steps`](planning/break-feature-into-steps.md) | future implementation and testing prompts | Executes and verifies each bounded step separately instead of sending one broad prompt |
| Completed implementation | [`/production-readiness`](../skills/production-readiness/README.md) | Evaluates the concrete release after implementation and relevant checks are available |

Do not chain resources merely because they are related. Add another prompt or skill only when it produces a distinct artifact, verifies a separate risk, or supplies missing evidence.

## File convention

Store each recipe as a lowercase kebab-case Markdown file in the closest category:

```text
prompts/<category>/<recipe-name>.md
```

Examples:

```text
prompts/planning/analyze-existing-project.md
prompts/planning/plan-feature.md
prompts/planning/break-feature-into-steps.md
prompts/debugging/investigate-failed-form-submit.md
prompts/database/add-column-safely.md
```

Do not create duplicate copies across categories. Choose the category that matches the recipe's primary outcome and link related recipes when useful.

## Quality rules

A prompt recipe should:

- address one clear, repeatable task;
- define use and non-use cases;
- list required context and reusable variables;
- distinguish facts, assumptions, and acceptance criteria;
- preserve existing architecture and behavior outside scope;
- include explicit safety and prohibited-change boundaries;
- require evidence before claiming testing, security, deployment, or verification;
- define the expected output and verification checklist;
- identify genuinely useful skill or prompt combinations without making them dependencies.

Project-specific facts belong in the recipe variables or supplied context, not in the reusable core. Broad operating behavior belongs in Workspace Knowledge or Project Knowledge rather than being repeated in every prompt.
