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
| [`analyze-existing-project`](planning/analyze-existing-project.md) | Planning | Build a read-only, evidence-based map of an existing project's architecture, flows, conventions, risks, and unknowns | Use before `/feature-planner`, `/safe-database-migration`, `/rls-security-review`, `/multi-tenant-isolation-review`, or `/production-readiness` when the existing project is not yet sufficiently understood |

## Combining prompts and skills

Prompts and skills have different roles:

- A **prompt recipe** handles one focused task or output.
- A **skill** provides a deeper reusable workflow with stronger trigger rules, boundaries, references, and output contracts.
- Workspace Knowledge and Project Knowledge provide persistent context; they are not substitutes for either.

Prefer a sequence instead of combining multiple large instructions into one prompt:

1. Establish context with a planning or analysis recipe.
2. Use the focused skill or prompt that owns the main task.
3. Use a verification-oriented recipe or review skill for the affected risk area.

Current useful combinations:

| Start with | Continue with | Why |
| --- | --- | --- |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/feature-planner`](../skills/feature-planner/README.md) | Converts verified project evidence into a scoped feature plan |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/safe-database-migration`](../skills/safe-database-migration/README.md) | Establishes schema, dependency, data, and migration context before the focused migration review |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/rls-security-review`](../skills/rls-security-review/README.md) | Maps auth, data, storage, functions, and trusted boundaries before the authorization audit |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/multi-tenant-isolation-review`](../skills/multi-tenant-isolation-review/README.md) | Identifies tenant resources and flows before the end-to-end isolation review |
| [`analyze-existing-project`](planning/analyze-existing-project.md) | [`/production-readiness`](../skills/production-readiness/README.md) | Clarifies release scope, architecture, critical flows, and operational dependencies before launch assessment |

Do not chain resources merely because they are related. Add another prompt or skill only when it produces a distinct artifact, verifies a separate risk, or supplies missing evidence.

## File convention

Store each recipe as a lowercase kebab-case Markdown file in the closest category:

```text
prompts/<category>/<recipe-name>.md
```

Examples:

```text
prompts/planning/analyze-existing-project.md
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