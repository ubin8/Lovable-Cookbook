# Contributing to Lovable Cookbook

Thanks for helping improve the cookbook. Contributions are welcome, but the quality bar is intentionally higher than “this prompt worked once.”

## What belongs here

A strong contribution solves a recurring problem and can be adapted across multiple Lovable projects.

Good candidates include:

- a skill for a repeatable product, UX, engineering, or review workflow;
- a focused prompt recipe with explicit inputs and constraints;
- a practical guide for a common Lovable integration or decision;
- a checklist that catches meaningful mistakes before build or launch;
- a worked example that teaches a reusable method;
- a correction that removes unsafe, misleading, or outdated advice.

## What does not belong here

Avoid contributions that:

- are only useful for one private project;
- restate a request without adding a process;
- depend on hidden context;
- promise guaranteed outcomes;
- encourage implementation before requirements are understood;
- ignore authorization, privacy, destructive actions, or failure states;
- copy proprietary documentation or paid material;
- include credentials, private URLs, personal data, or customer information;
- duplicate an existing artifact without a clear improvement.

## Before you start

For non-trivial additions, open an issue first. Explain:

1. the recurring problem;
2. who experiences it;
3. why an existing artifact does not solve it;
4. the proposed artifact type and scope;
5. how the result can be validated.

Small corrections, broken links, and obvious documentation fixes can go directly to a pull request.

## Contribution workflow

1. Fork the repository.
2. Create a focused branch from `main`.
3. Copy the closest template from `templates/`.
4. Add one coherent contribution.
5. Check links, formatting, examples, and claims.
6. Update the README catalog when adding a top-level artifact.
7. Open a pull request with a clear explanation of the problem and solution.

Example branch names:

```text
skill/ux-reviewer
guide/supabase-auth
fix/broken-link
docs/clarify-security-policy
```

## Required structure

### Skills

Every skill must have a main file:

```text
skills/<skill-name>/SKILL.md
```

A skill may also include supporting files when the workflow is too broad for one readable file:

```text
skills/rls-security-review/
├── SKILL.md
└── references/
    ├── review-workflow.md
    ├── testing.md
    └── severity-and-reporting.md
```

The main `SKILL.md` must remain the authoritative entry point. It should contain the frontmatter, trigger and non-trigger rules, role, core behavior, output contract, hard constraints, and links to every supporting file.

Reference files should:

- cover one clearly named concern;
- be loaded only when relevant to the skill workflow;
- avoid duplicating the main file;
- use relative links;
- remain understandable outside a single private project;
- never hide essential safety boundaries or trigger rules.

Skills should normally include:

- skill name and frontmatter;
- use cases;
- explicit non-use cases;
- role and responsibility;
- required inputs or review-readiness rules;
- process or decision logic;
- required output structure;
- limits and risks;
- validation criteria.

Use lowercase kebab-case:

```text
skills/feature-planner/SKILL.md
skills/rls-security-review/references/review-workflow.md
```

### Guides

Guides should explain a practical workflow rather than paraphrase vendor documentation. Separate stable principles from provider-specific steps and note where details may change.

```text
guides/supabase-auth.md
guides/prompting-existing-projects.md
```

### Checklists

Checklist items must be observable. Avoid vague entries such as “the UX is good.” Prefer concrete checks such as “all destructive actions require confirmation and provide a recovery path where feasible.”

```text
checklists/pre-launch.md
checklists/auth-review.md
```

### Examples

Examples must use fictional or sanitized data. They should show the reasoning and validation process, not only the final prompt.

## Writing standard

Use clear English and concise Markdown.

- Prefer direct language.
- Define unfamiliar terms.
- Separate facts from assumptions.
- State trade-offs instead of hiding them.
- Avoid hype, filler, and unsupported superlatives.
- Do not claim that generated output is automatically secure or production-ready.
- Use relative links for files inside the repository.

## Validation checklist

Before opening a pull request, confirm:

- [ ] The contribution solves a repeatable problem.
- [ ] Use and non-use cases are explicit.
- [ ] Required context and assumptions are listed.
- [ ] The output or outcome is reviewable.
- [ ] Risks, limitations, and edge cases are covered.
- [ ] Supporting references are linked from the main skill and contain no hidden essential rules.
- [ ] Examples contain no secrets or private data.
- [ ] Links and file paths work.
- [ ] The README catalog is updated when necessary.
- [ ] The contribution does not duplicate existing material without improvement.

## Pull request description

A useful pull request explains:

- **Problem:** What recurring problem does this solve?
- **Change:** What was added or changed?
- **Scope:** What is deliberately not covered?
- **Validation:** How was the contribution reviewed or tested?
- **Risks:** What could still be wrong or become outdated?

## Review process

Reviews focus on usefulness, clarity, correctness, boundaries, safety, and maintainability.

A contribution may be rejected even when it is well written if it is too narrow, duplicates existing content, hides important assumptions, or adds more maintenance cost than value.

Maintainers may request substantial changes. That is normal review, not a personal judgment.

## Conduct and security

Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md).

Do not disclose vulnerabilities, exposed credentials, or private data in public issues or pull requests. Follow the [Security Policy](SECURITY.md).
