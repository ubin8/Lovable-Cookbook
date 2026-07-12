# Prompt Recipes

Prompt recipes are focused, reusable instructions for recurring Lovable tasks. Each recipe owns one clear output, states its required context and boundaries, and defines how the result is verified.

Use [`TEMPLATE.md`](TEMPLATE.md) when creating a new recipe.

## Browse by category

| Category | Primary outcome |
| --- | --- |
| [`planning/`](planning/) | Analysis, acceptance criteria, impact assessment, feature plans, and execution sequencing |
| [`implementation/`](implementation/) | Focused changes inside an existing project |
| [`debugging/`](debugging/) | Reproduction, evidence gathering, root-cause isolation, and targeted fixes |
| [`database/`](database/) | Schemas, migrations, queries, constraints, and data integrity |
| [`security/`](security/) | Authorization, data exposure, secrets, tenant boundaries, and trusted operations |
| [`testing/`](testing/) | Automated, browser, regression, and failure-path verification |
| [`design/`](design/) | UI, UX, accessibility, responsiveness, and design-system consistency |
| [`integrations/`](integrations/) | APIs, OAuth, webhooks, provider accounts, jobs, and external services |
| [`performance/`](performance/) | Rendering, network, bundle, media, query, and Core Web Vitals analysis |
| [`launch/`](launch/) | Release readiness, deployment risk, rollout, and post-launch validation |

## Available recipes

### Planning

- [`analyze-existing-project`](planning/analyze-existing-project.md) — map an existing project's architecture, flows, conventions, risks, and unknowns.
- [`create-acceptance-criteria`](planning/create-acceptance-criteria.md) — turn a request into a precise, testable behavioral contract.
- [`assess-change-impact`](planning/assess-change-impact.md) — evaluate direct, indirect, compatibility, security, data, and operational impact before implementation.
- [`plan-feature`](planning/plan-feature.md) — create an implementation-ready plan for one feature.
- [`break-feature-into-steps`](planning/break-feature-into-steps.md) — split an approved plan into small, ordered, independently verifiable steps.

### Implementation

- [`implement-small-feature`](implementation/implement-small-feature.md) — implement one narrow, understood feature with minimal surface area.

## Recommended sequences

Use only the steps that add distinct value.

### Small, clear feature

`create-acceptance-criteria` → `implement-small-feature`

### Existing project or unclear feature area

`analyze-existing-project` → `create-acceptance-criteria` when needed → `plan-feature` → `break-feature-into-steps` when needed → implementation

### Change to existing behavior

`assess-change-impact` → `create-acceptance-criteria` when the new contract is unclear → `plan-feature` or `break-feature-into-steps` → implementation

## Combining prompts and skills

A prompt recipe produces one focused artifact or change. A skill provides a deeper reusable workflow with stronger triggers, boundaries, references, and output contracts. Neither replaces Workspace Knowledge or Project Knowledge.

Useful combinations:

| Prompt | Skill | Use together when |
| --- | --- | --- |
| `analyze-existing-project` or `plan-feature` | [`/feature-planner`](../skills/feature-planner/README.md) | deeper feature planning needs verified project evidence |
| `assess-change-impact`, `plan-feature`, or `break-feature-into-steps` | [`/safe-database-migration`](../skills/safe-database-migration/README.md) | schema, data, rollout, compatibility, or rollback risk exists |
| planning or implementation prompt | [`/rls-security-review`](../skills/rls-security-review/README.md) | authorization, RLS, storage, RPC, or privileged operations are affected |
| planning or implementation prompt | [`/multi-tenant-isolation-review`](../skills/multi-tenant-isolation-review/README.md) | tenant boundaries span data, files, search, exports, jobs, realtime, or integrations |
| completed implementation | [`/production-readiness`](../skills/production-readiness/README.md) | a concrete release is ready for evidence-based launch assessment |

Do not chain resources merely because they are related. Add another prompt or skill only when it resolves material ambiguity, produces a distinct artifact, or reviews a separate risk.

## File convention

Store each recipe as a lowercase kebab-case Markdown file in the closest category:

```text
prompts/<category>/<recipe-name>.md
```

Choose the category by the recipe's primary outcome. Do not create duplicate copies across categories.

## Quality rules

Every recipe should define when it applies, required context, reusable variables, explicit boundaries, expected output, and evidence-based verification. Project facts belong in supplied context; durable cross-task behavior belongs in Workspace Knowledge or Project Knowledge.
