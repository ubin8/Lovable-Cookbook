<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=transparent&height=120&section=header&text=Lovable%20Cookbook&fontSize=52&fontColor=8B5CF6&fontAlignY=55" alt="Lovable Cookbook" />
</p>

<p align="center">
  <strong>Practical skills, knowledge, prompts, workflows, templates, and checklists for building better products with Lovable.</strong>
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

Lovable can accelerate implementation, but it does not replace product judgment, precise requirements, sound architecture, secure data access, testing, or maintenance. This repository turns those recurring tasks into explicit workflows and durable knowledge that can be reused, reviewed, and improved.

This is not a dump of “magic prompts.” Every useful artifact should make its purpose, required context, limits, expected output, and validation method clear.

## What you will find here

| Area | What it contains |
| --- | --- |
| **Skills** | Structured instructions for recurring product, UX, engineering, and review roles |
| **Workspace Knowledge** | Durable engineering rules shared across every project in a Lovable workspace |
| **Project Knowledge** | Project-specific product, domain, architecture, and integration context — coming later |
| **Prompt recipes** | Focused prompts with context, constraints, expected output, and quality checks |
| **Guides** | Practical explanations for common Lovable workflows and decisions |
| **Templates** | Reusable structures for specifications, prompts, decisions, and handoffs |
| **Checklists** | Compact quality gates before implementation, review, and launch |
| **Examples** | Worked examples showing how the pieces fit together in realistic projects |

---

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Choose a [Workspace Knowledge baseline](workspace-knowledge/README.md).
3. Review the [Automatic use recommendations](guides/automatic-use.md).
4. Pick the artifact that matches the actual problem.
5. Replace placeholders with facts from your workspace or project.
6. Remove requirements that do not apply.
7. Review assumptions before implementation.
8. Validate the result with the relevant checklist.

> [!WARNING]
> Copying an artifact without adapting it to your workspace or project is bad practice. A reusable resource provides structure, not project truth.

---

## Catalog

### Available skills

| Skill | Purpose | Recommended Automatic use |
| --- | --- | --- |
| [`/feature-planner`](skills/feature-planner/README.md) | Critically plan a feature or larger product change before implementation | **On**, only with a narrow planning trigger |
| [`/rls-security-review`](skills/rls-security-review/README.md) | Perform a read-only, evidence-based authorization review of Lovable and Supabase projects, including RLS, grants, tenant isolation, RPCs, views, storage, edge functions, and realistic bypass paths | **Off** |
| [`/production-readiness`](skills/production-readiness/README.md) | Assess a specific Lovable release and return an evidence-based GO, CONDITIONAL GO, NO-GO, or INSUFFICIENT EVIDENCE decision | **Off** |
| [`/safe-database-migration`](skills/safe-database-migration/README.md) | Review an existing PostgreSQL, Supabase, or Lovable schema or data change for data safety, compatibility, locking, RLS impact, backfill quality, recovery, and validation before execution | **On** |
| [`/multi-tenant-isolation-review`](skills/multi-tenant-isolation-review/README.md) | Audit whether organizations, workspaces, teams, or customer accounts are isolated across data, roles, storage, privileged paths, search, exports, analytics, caches, realtime, jobs, webhooks, and tenant lifecycle | **Off** |

See [Automatic use recommendations](guides/automatic-use.md) for trigger boundaries and rationale.

Each skill folder contains a human-readable `README.md` and an authoritative `SKILL.md`. Complex skills may also include focused reference modules that must remain with the main skill.

### Workspace Knowledge

| Resource | Purpose |
| --- | --- |
| [Universal Workspace Knowledge](workspace-knowledge/UNIVERSAL.md) | Ready-to-use engineering baseline that adapts to the actual project stack and established conventions |
| [Workspace Knowledge Template](workspace-knowledge/TEMPLATE.md) | Configurable version with placeholders for approved tools, providers, naming conventions, checks, and workspace-specific restrictions |
| [Workspace Knowledge guide](workspace-knowledge/README.md) | Explains scope, precedence, setup, maintenance, and the difference from future Project Knowledge |

Workspace Knowledge applies across projects. Project-specific product behavior, schemas, routes, users, domain rules, and integration facts belong in Project Knowledge, which will be added separately.

### Planned skills

| Skill | Purpose |
| --- | --- |
| `/webhook-reliability-review` | Review webhook integrations for signature validation, replay protection, idempotency, duplicate and out-of-order events, retries, timeouts, partial processing, dead-letter handling, secret-safe logging, state reconstruction, and manual reprocessing |
| `/incident-investigator` | Investigate incidents without changing the system first by collecting symptoms, affected users, timelines, deployments, logs, network and database evidence, reproduction steps, competing hypotheses, impact, and safe containment measures before proposing a repair |
| `/data-lifecycle-review` | Trace personal and business data from collection through storage, access, third-party transfers, export, retention, account deletion, linked records, and backups; identifies technical gaps but does not provide legal or GDPR certification |
| `/schema-integrity-review` | Review nullability, unique constraints, foreign keys, deletion behavior, orphaned records, status values, timestamps, indexes, and data consistency |
| `/state-consistency-review` | Review complex React state management for duplicate sources of truth, stale data, optimistic updates, race conditions, cache invalidation, loading and error states, and form-state consistency |
| `/performance-root-cause-analysis` | Measure and isolate performance causes across bundle size, unnecessary renders and requests, slow queries, missing indexes, large media, blocking code, and Core Web Vitals, with before-and-after evidence |

### Guides

- [How to use this cookbook](guides/how-to-use-this-cookbook.md)
- [Automatic use recommendations](guides/automatic-use.md)
- [Workspace Knowledge guide](workspace-knowledge/README.md)

### Checklists

- [Pre-build checklist](checklists/pre-build.md)
- [Pre-launch checklist](checklists/pre-launch.md)

### Templates

- [Skill template](templates/skills/SKILL.template.md)
- [Skill README template](templates/skills/README.template.md)
- [Workspace Knowledge template](workspace-knowledge/TEMPLATE.md)

---

## Repository structure

```text
Lovable-Cookbook/
├── skills/
│   ├── feature-planner/
│   │   ├── README.md           # Human-readable documentation
│   │   └── SKILL.md            # Authoritative execution instructions
│   ├── rls-security-review/
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── references/         # Supporting authorization review modules
│   ├── production-readiness/
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── references/         # Supporting launch-readiness modules
│   ├── safe-database-migration/
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── references/         # Supporting migration-safety modules
│   └── multi-tenant-isolation-review/
│       ├── README.md
│       ├── SKILL.md
│       └── references/         # Supporting tenant-isolation modules
├── workspace-knowledge/
│   ├── README.md               # Scope, setup, precedence, and maintenance
│   ├── UNIVERSAL.md            # Ready-to-use cross-project baseline
│   └── TEMPLATE.md             # Configurable workspace baseline
├── templates/                  # Templates for cookbook artifacts
├── guides/                     # Practical workflows and explanations
├── examples/                   # End-to-end worked examples
├── checklists/                 # Review and release quality gates
├── .github/                    # Community and repository configuration
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── SECURITY.md
└── LICENSE
```

Every published skill should include both files:

- `README.md` explains the skill to people, including use cases, files, setup, expected output, limitations, and recommended Automatic use behavior.
- `SKILL.md` remains the authoritative instruction set used to execute the skill.

Workspace Knowledge has a different structure: the universal file is ready to use, while the template exposes deliberate configuration points. Both must remain cross-project, durable, and free of secrets or temporary task instructions.

A skill may also include narrowly scoped reference files. Supporting files should reduce duplication and keep the main skill readable; they should not hide essential trigger rules or hard constraints.

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
8. **Should Automatic use be enabled, and why?**
9. **Is the content workspace-wide or project-specific?**

The following do not meet the bar:

- vague instructions such as “make this better”;
- unsupported claims or guaranteed outcomes;
- project-specific prompt dumps with hidden context;
- workflows that ignore permissions, data ownership, or failure states;
- content copied from proprietary sources without permission;
- advice that encourages shipping generated work without review;
- broad Automatic use triggers that can unexpectedly replace implementation with an audit or planning workflow;
- temporary task instructions, secrets, or project-specific facts placed in Workspace Knowledge.

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
