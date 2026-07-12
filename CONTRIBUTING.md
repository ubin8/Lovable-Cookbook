# Contributing to Lovable Cookbook

Thanks for helping improve the cookbook. Contributions are welcome, but the quality bar is intentionally higher than “this prompt worked once.”

## What belongs here

A strong contribution solves a recurring problem and can be adapted across multiple Lovable projects.

Good candidates include:

- a skill for a repeatable product, engineering, security, or review workflow;
- durable Workspace Knowledge that applies across projects;
- reusable general or product-specific Project Knowledge templates;
- a focused prompt recipe with explicit context, constraints, output, and verification;
- a practical guide for a common Lovable workflow or decision;
- a checklist that catches meaningful mistakes before build or launch;
- a worked example that teaches a reusable method;
- a correction that removes unsafe, misleading, outdated, or inconsistent advice.

## What does not belong here

Avoid contributions that:

- are useful only for one private project unless they are sanitized examples;
- depend on hidden context;
- promise guaranteed outcomes;
- encourage implementation before requirements are understood;
- ignore authorization, privacy, destructive actions, or failure states;
- copy proprietary documentation or paid material;
- include credentials, private URLs, personal data, or customer information;
- duplicate an existing artifact without a clear improvement;
- place project-specific or temporary instructions in Workspace Knowledge;
- present speculative Project Knowledge as verified project fact;
- add vague prompts without triggers, variables, boundaries, or verification.

## Before you start

For non-trivial additions, open an issue first and explain:

1. the recurring problem;
2. who experiences it;
3. why an existing artifact does not solve it;
4. the proposed artifact type and scope;
5. how the result can be validated.

Small corrections, broken links, and obvious documentation fixes can go directly to a pull request.

## Contribution workflow

1. Fork the repository.
2. Create a focused branch from `main`.
3. Copy the closest available template.
4. Add one coherent contribution.
5. Check links, formatting, examples, claims, naming, and category placement.
6. Update the root catalog only when adding or removing a top-level resource.
7. Open a pull request with a clear explanation of the problem and solution.

Example branch names:

```text
skill/webhook-reliability-review
knowledge/workspace-security-rules
knowledge/project-saas-template
knowledge/internal-tool-crm
prompt/database-safe-column-addition
example/project-knowledge-saas
fix/broken-link
docs/clarify-security-policy
```

## Required structure

### Skills

Every published skill uses this structure:

```text
skills/<skill-name>/
├── README.md
├── SKILL.md
└── references/        # optional
```

Responsibilities:

- `README.md` is human-readable documentation covering purpose, usage, files, setup, expected output, limitations, related resources, and recommended Automatic use behavior.
- `SKILL.md` is the authoritative execution file containing frontmatter, trigger rules, role, process, output contract, and hard constraints.
- `references/` contains narrowly scoped supporting modules when the workflow is too broad for one readable skill file.

Use the templates in:

```text
templates/skills/TEMPLATE.md
templates/skills/README.template.md
```

The main `SKILL.md` must remain the authoritative entry point. It must link every required supporting file and must not hide trigger rules or safety constraints in references.

Reference files should cover one clearly named concern, avoid duplicating the main file, use relative links, remain understandable outside a private project, and contain no hidden essential trigger or safety rules.

Use lowercase kebab-case for skill folders and reference files.

### Workspace Knowledge

Workspace Knowledge must remain durable and apply across multiple projects in a workspace.

```text
workspace-knowledge/
├── README.md
├── UNIVERSAL.md
└── TEMPLATE.md
```

Rules:

- `UNIVERSAL.md` must remain broadly useful without organization-specific assumptions.
- `TEMPLATE.md` may expose deliberate placeholders for tools, providers, checks, naming rules, and restrictions.
- Project-specific schemas, routes, product rules, users, tenants, integrations, or temporary tasks do not belong here.
- Secrets, credentials, private URLs, and customer information are prohibited.
- Do not duplicate content that already has an authoritative location.

### Project Knowledge

Project Knowledge defines durable context for one concrete application.

```text
project-knowledge/
├── README.md
├── TEMPLATE.md
├── <ProductType>.md          # optional top-level product template
└── internal-tools/           # internal operational applications
    └── <ToolType>.md
```

Rules:

- `TEMPLATE.md` is the neutral starting point.
- Product-specific templates such as `SaaS.md` and `Marketplace.md` belong at the top level.
- Internal operational applications such as CRM, admin panels, inventory systems, or support tools belong under `project-knowledge/internal-tools/`.
- Use concise PascalCase filenames for specific templates.
- A specific template must remain reusable across multiple projects of that type and must not contain one company's private conventions.
- Remove irrelevant sections rather than leaving unresolved placeholders in a finished configuration.
- Do not include temporary task instructions, active incident notes, short-lived priorities, secrets, or unsupported claims.
- Workspace-wide defaults belong in Workspace Knowledge; only more specific project rules belong in Project Knowledge.

### Prompt recipes

Prompt recipes use this structure:

```text
prompts/
├── README.md
├── TEMPLATE.md
├── planning/
├── implementation/
├── debugging/
├── database/
├── security/
├── testing/
├── design/
├── integrations/
├── performance/
└── launch/
```

Store each recipe as:

```text
prompts/<category>/<lowercase-kebab-case-name>.md
```

Use [`prompts/TEMPLATE.md`](prompts/TEMPLATE.md) as the starting point.

Rules:

- Choose the category that matches the recipe's primary outcome.
- Do not duplicate the same recipe across categories.
- Keep each recipe focused on one clear, repeatable task.
- Include purpose, use and non-use cases, required context, variables, the reusable prompt, expected output, and verification.
- Put project-specific facts in variables or supplied context, not in the reusable core.
- Do not repeat broad Workspace Knowledge or Project Knowledge unless the recipe needs a specific task-level constraint.
- High-risk recipes must require appropriate planning, evidence, review, and safe boundaries.
- Prompts must not claim testing, security, deployment, or verification without evidence.
- Use `integrations/` as the folder name even though the template category value is `INTEGRATION`.

Category boundary examples:

- A schema migration belongs in `database/`, even when it includes security checks.
- A pure authorization or data-exposure review belongs in `security/`.
- A bug investigation belongs in `debugging/`; a regression-test recipe belongs in `testing/`.
- A release-readiness or deployment-validation recipe belongs in `launch/`.

### Guides

Guides should explain a practical workflow rather than paraphrase vendor documentation. Separate stable principles from provider-specific steps and note where details may change.

### Checklists

Checklist items must be observable. Avoid vague entries such as “the UX is good.” Prefer concrete checks such as “all destructive actions require confirmation and provide a recovery path where feasible.”

### Examples

Examples must use fictional or sanitized data. They should show the process and validation, not only the final prompt.

## Writing standard

Use clear English and concise Markdown.

- Prefer direct language.
- Define unfamiliar terms.
- Separate facts from assumptions.
- State trade-offs instead of hiding them.
- Avoid hype, filler, and unsupported superlatives.
- Do not claim that generated output is automatically secure or production-ready.
- Use relative links for files inside the repository.
- Keep naming and capitalization consistent with existing repository conventions.

## Validation checklist

Before opening a pull request, confirm:

- [ ] The contribution solves a repeatable problem.
- [ ] File names, paths, and category placement follow repository conventions.
- [ ] Links and relative references work.
- [ ] Use and non-use cases are explicit where applicable.
- [ ] Required context and assumptions are listed.
- [ ] The output or outcome is reviewable.
- [ ] Risks, limitations, and edge cases are covered.
- [ ] Supporting references contain no hidden essential rules.
- [ ] Workspace Knowledge contains no project-specific or temporary instructions.
- [ ] Project Knowledge contains no temporary tasks, secrets, unsupported claims, or private conventions.
- [ ] Product-specific Project Knowledge remains reusable beyond one private application.
- [ ] Internal-tool templates are grouped under `project-knowledge/internal-tools/`.
- [ ] Prompt recipes use `prompts/TEMPLATE.md`, remain focused, and include verification.
- [ ] Examples contain no secrets or private data.
- [ ] The root catalog is updated only when necessary.
- [ ] The contribution does not duplicate existing material without improvement.

## Pull request description

A useful pull request explains:

- **Problem:** What recurring problem does this solve?
- **Change:** What was added or changed?
- **Scope:** What is deliberately not covered?
- **Validation:** How was the contribution reviewed or tested?
- **Risks:** What could still be wrong or become outdated?

## Review process

Reviews focus on usefulness, clarity, correctness, boundaries, safety, consistency, and maintainability.

A contribution may be rejected even when it is well written if it is too narrow, duplicates existing content, hides assumptions, breaks repository structure, or adds more maintenance cost than value.

Maintainers may request substantial changes. That is normal review, not a personal judgment.

## Conduct and security

Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md).

Do not disclose vulnerabilities, exposed credentials, or private data in public issues or pull requests. Follow the [Security Policy](SECURITY.md).
