# Lovable Cookbook

A practical, community-driven collection of reusable **skills, prompt recipes, workflows, templates, checklists, and examples** for building better products with [Lovable](https://lovable.dev/).

> [!IMPORTANT]
> This is an independent community project. It is not affiliated with or endorsed by Lovable.

## The idea

Lovable makes implementation fast. It does not remove the need for product judgment, clear requirements, sound data models, security decisions, testing, or maintenance.

The **Lovable Cookbook** turns repeatable product and engineering work into explicit, reusable building blocks. It is deliberately not a dump of “magic prompts.” Every artifact should explain when it applies, what context it needs, what it produces, where it can fail, and how its result can be checked.

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Choose a skill, guide, template, or checklist from the catalog below.
3. Replace placeholders with facts from your own project.
4. Review assumptions before asking Lovable to implement anything.
5. Validate the result with the relevant checklist.

## Catalog

### Skills

| Skill | Purpose | Status |
| --- | --- | --- |
| [`/feature-planner`](skills/feature-planner/SKILL.md) | Critically plan a new feature or larger product change before implementation | Available |
| `/ux-reviewer` | Review flows, states, hierarchy, accessibility, and usability | Planned |
| `/bug-investigator` | Structure reproduction, diagnosis, evidence, and fix validation | Planned |
| `/data-model-planner` | Design entities, relationships, ownership, constraints, and migrations | Planned |
| `/security-reviewer` | Identify practical security and privacy risks before release | Planned |

### Guides

- [How to use this cookbook](guides/how-to-use-this-cookbook.md)

### Checklists

- [Pre-build checklist](checklists/pre-build.md)
- [Pre-launch checklist](checklists/pre-launch.md)

### Templates

- [Skill template](templates/skills/SKILL.template.md)

## Repository structure

```text
Lovable-Cookbook/
├── skills/                  # Reusable role and workflow instructions
├── templates/               # Templates for new cookbook artifacts
├── guides/                  # Practical Lovable workflows and explanations
├── examples/                # End-to-end worked examples
├── checklists/              # Compact quality gates
└── .github/                 # Contribution and issue templates
```

## Included skill: Feature Planner

[`/feature-planner`](skills/feature-planner/SKILL.md) acts as a critical product manager, UX planner, and technical analyst. It is intended for features and larger changes that require decisions before implementation.

It forces the proposal through:

- user-problem validation;
- scope and MVP definition;
- UX states and recovery paths;
- functional requirements;
- data, permissions, and integrations;
- risks, edge cases, and trade-offs;
- measurable acceptance criteria;
- a clear proceed, revise, validate, or reject recommendation.

Do not use it for tiny copy or styling changes, direct implementation requests, or straightforward bug fixes.

## Quality standard

A contribution is useful only when it answers these questions:

1. **When should it be used?**
2. **When should it not be used?**
3. **Which inputs and assumptions are required?**
4. **Which process should be followed?**
5. **What should the output contain?**
6. **How can the result be validated?**
7. **What are the limits and risks?**

Vague instructions such as “make this better,” unsupported guarantees, and project-specific prompt dumps do not meet the bar.

## Planned areas

- Product discovery and feature planning
- UI and UX review
- Supabase and database design
- Authentication and authorization
- Payments and subscriptions
- SEO, analytics, and accessibility
- Testing and debugging
- Security and privacy review
- Deployment and release management
- Refactoring and technical debt
- Prompt patterns and anti-patterns

See the [roadmap](ROADMAP.md) for the proposed sequence.

## Contributing

Contributions are welcome when they solve a repeatable problem and are explicit about their limits. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening an issue or pull request.

## License

Released under the [MIT License](LICENSE).