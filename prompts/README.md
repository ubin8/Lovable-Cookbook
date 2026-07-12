# Prompt Recipes

Prompt recipes are focused, reusable instructions for recurring Lovable tasks. Each recipe should solve one clear problem, state the required context and boundaries, and define how the result is verified.

Use [`TEMPLATE.md`](TEMPLATE.md) when creating a new recipe.

## Categories

| Category | Purpose |
| --- | --- |
| [`planning/`](planning/) | Scope features, clarify requirements, compare approaches, and prepare safe implementation plans |
| [`implementation/`](implementation/) | Implement focused product and engineering changes inside an existing project |
| [`debugging/`](debugging/) | Reproduce failures, gather evidence, isolate causes, and propose or apply targeted fixes |
| [`database/`](database/) | Work with schemas, migrations, queries, constraints, data integrity, and PostgreSQL behavior |
| [`security/`](security/) | Review or improve authorization, data exposure, secrets, tenant boundaries, and trusted operations |
| [`testing/`](testing/) | Create, improve, or execute checks for critical behavior and failure cases |
| [`design/`](design/) | Improve interfaces, interaction patterns, accessibility, responsiveness, and design-system consistency |
| [`integrations/`](integrations/) | Add or review APIs, OAuth, webhooks, provider accounts, jobs, and external services |
| [`performance/`](performance/) | Measure and address rendering, network, bundle, media, query, and Core Web Vitals problems |
| [`launch/`](launch/) | Prepare releases, verify readiness, review deployment risks, and validate post-launch behavior |

## File convention

Store each recipe as a lowercase kebab-case Markdown file in the closest category:

```text
prompts/<category>/<recipe-name>.md
```

Examples:

```text
prompts/planning/plan-existing-feature.md
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
- define the expected output and verification checklist.

Project-specific facts belong in the recipe variables or supplied context, not in the reusable core. Broad operating behavior belongs in Workspace Knowledge or Project Knowledge rather than being repeated in every prompt.
