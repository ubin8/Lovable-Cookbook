<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=transparent&height=120&section=header&text=Lovable%20Cookbook&fontSize=52&fontColor=8B5CF6&fontAlignY=55" alt="Lovable Cookbook" />
</p>

<p align="center">
  <strong>Practical skills, prompts, workflows, templates, and checklists for building better products with Lovable.</strong>
  <br />
  Plan clearly · Prompt deliberately · Review critically · Ship responsibly
</p>

<p align="center">
  <a href="https://github.com/ubin8/Lovable-Cookbook/blob/main/LICENSE"><img src="https://img.shields.io/github/license/ubin8/Lovable-Cookbook?style=for-the-badge&labelColor=111111" alt="MIT License" /></a>
  <a href="https://github.com/ubin8/Lovable-Cookbook/stargazers"><img src="https://img.shields.io/github/stars/ubin8/Lovable-Cookbook?style=for-the-badge&labelColor=111111" alt="GitHub stars" /></a>
  <a href="https://github.com/ubin8/Lovable-Cookbook/commits/main"><img src="https://img.shields.io/github/last-commit/ubin8/Lovable-Cookbook?style=for-the-badge&labelColor=111111" alt="Last commit" /></a>
  <a href="https://github.com/ubin8/Lovable-Cookbook/issues"><img src="https://img.shields.io/github/issues/ubin8/Lovable-Cookbook?style=for-the-badge&labelColor=111111" alt="Open issues" /></a>
</p>

> [!IMPORTANT]
> Lovable Cookbook is an independent community project. It is not affiliated with, maintained by, or endorsed by Lovable.

---

## What this repository is

**Lovable Cookbook** is a curated collection of reusable resources for people building applications with [Lovable](https://lovable.dev/).

Lovable can accelerate implementation, but it does not replace product judgment, precise requirements, sound architecture, secure data access, testing, or maintenance. This repository turns those recurring tasks into explicit workflows that can be reused, reviewed, and improved.

This is not a dump of “magic prompts.” Every useful artifact should make its purpose, required context, limits, expected output, and validation method clear.

## What you will find here

| Area | What it contains |
| --- | --- |
| **Skills** | Structured instructions for recurring product, UX, engineering, and review roles |
| **Prompt recipes** | Focused prompts with context, constraints, expected output, and quality checks |
| **Guides** | Practical explanations for common Lovable workflows and decisions |
| **Templates** | Reusable structures for specifications, prompts, decisions, and handoffs |
| **Checklists** | Compact quality gates before implementation, review, and launch |
| **Examples** | Worked examples showing how the pieces fit together in realistic projects |

---

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Pick the artifact that matches the actual problem.
3. Replace placeholders with facts from your project.
4. Remove requirements that do not apply.
5. Review assumptions before implementation.
6. Validate the result with the relevant checklist.

> [!WARNING]
> Copying a recipe without adapting it to your project is bad practice. A reusable workflow provides structure, not project truth.

---

## Catalog

### Available skills

| Skill | Purpose | Status |
| --- | --- | --- |
| [`/feature-planner`](skills/feature-planner/SKILL.md) | Critically plan a new feature or larger product change before implementation | Available |

### Planned skills

| Skill | Purpose |
| --- | --- |
| `/ux-reviewer` | Review hierarchy, flows, states, accessibility, and usability |
| `/bug-investigator` | Structure reproduction, diagnosis, evidence, and fix validation |
| `/data-model-planner` | Design entities, relationships, ownership, constraints, and migrations |
| `/security-reviewer` | Identify practical security and privacy risks before release |
| `/implementation-prompter` | Turn an approved plan into a constrained implementation prompt |

### Guides

- [How to use this cookbook](guides/how-to-use-this-cookbook.md)

### Checklists

- [Pre-build checklist](checklists/pre-build.md)
- [Pre-launch checklist](checklists/pre-launch.md)

### Templates

- [Skill template](templates/skills/SKILL.template.md)

---

## Featured skill: Feature Planner

[`/feature-planner`](skills/feature-planner/SKILL.md) acts as a critical product manager, UX planner, and technical analyst before implementation begins.

It tests a proposal against:

- the concrete user problem;
- target users and current alternatives;
- MVP scope and explicit non-goals;
- user flows, states, permissions, and recovery paths;
- functional requirements and acceptance criteria;
- data ownership, integrations, and migrations;
- product, technical, privacy, security, and maintenance risks;
- simpler alternatives and opportunity costs.

Use it for meaningful features and larger changes. Do not use it for tiny copy edits, isolated styling changes, direct implementation requests, or straightforward bug fixes.

---

## Repository structure

```text
Lovable-Cookbook/
├── skills/                  # Reusable role and workflow instructions
├── templates/               # Templates for cookbook artifacts
├── guides/                  # Practical workflows and explanations
├── examples/                # End-to-end worked examples
├── checklists/              # Review and release quality gates
├── .github/                 # Community and repository configuration
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── SECURITY.md
├── ROADMAP.md
└── LICENSE
```

---

## Quality standard

A contribution should answer all of these questions:

1. **When should it be used?**
2. **When should it not be used?**
3. **Which inputs and assumptions are required?**
4. **Which process should be followed?**
5. **What should the output contain?**
6. **How can the result be validated?**
7. **What are its limits and risks?**

The following do not meet the bar:

- vague instructions such as “make this better”;
- unsupported claims or guaranteed outcomes;
- project-specific prompt dumps with hidden context;
- workflows that ignore permissions, data ownership, or failure states;
- content copied from proprietary sources without permission;
- advice that encourages shipping generated work without review.

---

## Roadmap

Planned areas include:

- product discovery and feature planning;
- UI and UX review;
- Supabase and database design;
- authentication and authorization;
- payments and subscriptions;
- SEO, analytics, and accessibility;
- testing and debugging;
- security and privacy review;
- deployment and release management;
- refactoring and technical debt;
- prompt patterns and anti-patterns.

See [`ROADMAP.md`](ROADMAP.md) for the proposed sequence.

---

## Contributing

Contributions are welcome when they solve a repeatable problem, state their boundaries, and can be reviewed objectively.

Before contributing, read:

- [Contributing guide](CONTRIBUTING.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Security policy](SECURITY.md)

A large number of shallow prompts does not make the repository better. One well-scoped, tested, and clearly documented workflow is more valuable.

## Security

Do not report vulnerabilities or exposed secrets in public issues. Follow the private reporting instructions in [`SECURITY.md`](SECURITY.md).

## License

Lovable Cookbook is released under the [MIT License](LICENSE). You may use, modify, and distribute the material under its terms. Attribution and the full license notice must be preserved where required.

---

<p align="center">
  Built for people who want Lovable to produce maintainable products, not just fast demos.
</p>