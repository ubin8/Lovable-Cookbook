<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=transparent&height=120&section=header&text=Lovable%20Cookbook&fontSize=52&fontColor=8B5CF6&fontAlignY=55" alt="Lovable Cookbook" />
</p>

<p align="center">
  <strong>Practical skills, knowledge, prompts, guides, and checklists for building better products with Lovable.</strong>
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

## What this repository is

Lovable Cookbook is a curated library of reusable resources for people building applications with [Lovable](https://lovable.dev/). It turns recurring product, engineering, security, and delivery work into explicit artifacts that can be reviewed, adapted, and improved.

This is not a collection of “magic prompts.” Every resource should define its purpose, required context, boundaries, expected result, and validation method.

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Choose a [Workspace Knowledge baseline](workspace-knowledge/README.md).
3. Add the closest [Project Knowledge template](project-knowledge/README.md).
4. Select a [prompt recipe](prompts/README.md) or [skill](skills/) for the current task.
5. Adapt it to the verified project context and validate the result.

> [!WARNING]
> Reusable resources provide structure, not project truth. Do not copy them without adapting them to the actual repository, product rules, data model, permissions, and integrations.

## Browse the cookbook

| Area | Start here | What it provides |
| --- | --- | --- |
| **Skills** | [`skills/`](skills/) | Deep reusable workflows for planning, database safety, authorization design and review, tenant isolation, and release review |
| **Prompt recipes** | [`prompts/README.md`](prompts/README.md) | Focused prompts organized by planning, implementation, debugging, database, security, testing, design, integrations, performance, and launch |
| **Workspace Knowledge** | [`workspace-knowledge/README.md`](workspace-knowledge/README.md) | Durable engineering defaults shared across projects |
| **Project Knowledge** | [`project-knowledge/README.md`](project-knowledge/README.md) | General and product-specific context templates for one application |
| **Guides** | [`guides/`](guides/) | Practical usage and workflow guidance |
| **Checklists** | [`checklists/`](checklists/) | Compact build and launch quality gates |
| **Examples** | [`examples/`](examples/) | Sanitized examples showing how resources fit together |

## Common workflow

```text
Workspace Knowledge
→ Project Knowledge
→ Planning or analysis prompt
→ Focused implementation prompt or skill
→ Testing and specialist review
→ Production readiness
```

Use only the resources that add distinct value. The [Prompt Recipe catalog](prompts/README.md) documents useful prompt and skill combinations without making them dependencies.

## Featured resources

### Prompt recipes

- [`analyze-existing-project`](prompts/planning/analyze-existing-project.md)
- [`create-acceptance-criteria`](prompts/planning/create-acceptance-criteria.md)
- [`assess-change-impact`](prompts/planning/assess-change-impact.md)
- [`plan-feature`](prompts/planning/plan-feature.md)
- [`break-feature-into-steps`](prompts/planning/break-feature-into-steps.md)
- [`implement-small-feature`](prompts/implementation/implement-small-feature.md)

### Skills

- [`/feature-planner`](skills/feature-planner/README.md)
- [`/safe-database-migration`](skills/safe-database-migration/README.md)
- [`/rls-policy-designer`](skills/rls-policy-designer/README.md)
- [`/rls-security-review`](skills/rls-security-review/README.md)
- [`/multi-tenant-isolation-review`](skills/multi-tenant-isolation-review/README.md)
- [`/production-readiness`](skills/production-readiness/README.md)

See [Automatic use recommendations](guides/automatic-use.md) before enabling skills automatically.

## Repository map

```text
Lovable-Cookbook/
├── skills/
├── prompts/
├── workspace-knowledge/
├── project-knowledge/
├── guides/
├── checklists/
├── examples/
├── templates/
└── .github/
```

## Quality standard

A useful contribution must solve a repeatable problem, state when it applies, require enough context, preserve important boundaries, produce a reviewable result, and avoid claims of testing, security, deployment, or readiness without evidence.

Read [`CONTRIBUTING.md`](CONTRIBUTING.md), [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md), and [`SECURITY.md`](SECURITY.md) before contributing.

## License

Lovable Cookbook is released under the [MIT License](LICENSE).

---

<p align="center">
  Built for people who want Lovable to produce maintainable products, not just fast demos.
</p>
