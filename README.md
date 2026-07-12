# Lovable Cookbook

A practical, community-driven collection of reusable skills, prompts, workflows, templates, checklists, and examples for building better products with [Lovable](https://lovable.dev/).

> This repository is independent and is not an official Lovable project.

## Why this repository exists

Lovable can generate impressive results quickly, but speed alone does not produce a good product. Weak requirements, vague prompts, missing edge cases, poor data models, and unreviewed security decisions still lead to fragile applications.

The **Lovable Cookbook** turns repeatable product and engineering work into reusable building blocks. The goal is not to collect random “magic prompts”, but to document workflows that are clear, testable, and maintainable.

## What belongs here

- **Skills** — structured instructions for recurring roles or workflows
- **Prompt recipes** — focused prompts with context, constraints, and expected output
- **Project templates** — reusable specifications, decision records, and planning documents
- **Guides** — practical explanations for common Lovable workflows
- **Examples** — realistic end-to-end use cases
- **Checklists** — compact quality gates before shipping

## Repository structure

```text
lovable-cookbook/
├── skills/                  # Reusable role and workflow instructions
├── templates/
│   ├── prompts/             # Prompt recipe templates
│   └── project-docs/        # PRDs, decisions, handoff documents
├── guides/                  # Practical Lovable guides
├── examples/                # Worked examples
├── checklists/              # Review and release checklists
└── .github/                 # Contribution and issue templates
```

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Copy the relevant recipe or skill into your working context.
3. Replace every placeholder with project-specific facts.
4. Review assumptions before asking Lovable to implement anything.
5. Validate the result with the [pre-launch checklist](checklists/pre-launch.md).

## Included first skill

### Feature Planner

[`/feature-planner`](skills/feature-planner/SKILL.md) is a critical planning skill for new features and larger product changes. It forces the idea through user-problem validation, scope definition, UX analysis, technical constraints, risks, and acceptance criteria before implementation begins.

Use it when a request needs product decisions. Do not use it for tiny styling changes, isolated copy edits, or straightforward bug fixes.

## Recipe quality standard

A useful contribution should answer five questions:

1. **When should this be used?**
2. **What inputs are required?**
3. **What process should be followed?**
4. **What should the output contain?**
5. **How can the result be checked?**

Recipes that only say “make this better” or promise guaranteed results are not useful enough for this repository.

## Planned areas

- Product discovery and feature planning
- UI and UX review
- Database and Supabase design
- Authentication and authorization
- Payments and subscriptions
- SEO, analytics, and accessibility
- Testing and debugging
- Security review
- Deployment and release management
- Refactoring and technical debt
- Prompt patterns and anti-patterns

See the [roadmap](ROADMAP.md) for the proposed sequence.

## Contributing

Contributions are welcome, but they should solve a repeatable problem rather than showcase a single lucky prompt. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening an issue or pull request.

## License

Released under the [MIT License](LICENSE).
