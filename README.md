<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=transparent&height=120&section=header&text=Lovable%20Cookbook&fontSize=52&fontColor=8B5CF6&fontAlignY=55" alt="Lovable Cookbook" />
</p>

<p align="center">
  <strong>Practical skills, knowledge, workflows, templates, and checklists for building better products with Lovable.</strong>
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

Lovable can accelerate implementation, but it does not replace product judgment, precise requirements, sound architecture, secure data access, testing, or maintenance. This repository turns recurring work into explicit, reusable resources that can be reviewed and improved.

This is not a dump of “magic prompts.” Every artifact should have a clear purpose, required context, boundaries, expected outcome, and validation method.

## What you will find here

| Area | What it contains |
| --- | --- |
| **Skills** | Structured instructions for recurring product, engineering, security, and review workflows |
| **Workspace Knowledge** | Durable engineering rules shared across all projects in a Lovable workspace |
| **Project Knowledge** | Durable product, domain, architecture, permissions, data, integration, design, and operational context for one project |
| **Guides** | Practical explanations for common Lovable workflows and decisions |
| **Templates** | Reusable starting points for structured artifacts |
| **Checklists** | Compact quality gates before implementation and launch |
| **Examples** | Worked examples showing how multiple resources fit together |

---

## Start here

1. Read [How to use this cookbook](guides/how-to-use-this-cookbook.md).
2. Choose a [Workspace Knowledge baseline](workspace-knowledge/README.md).
3. Choose a [Project Knowledge template](project-knowledge/README.md) for the concrete application.
4. Review the [Automatic use recommendations](guides/automatic-use.md).
5. Select the resource that matches the actual problem.
6. Adapt it to the verified workspace or project context.
7. Validate the result before relying on it.

> [!WARNING]
> Copying an artifact without adapting it to the actual workspace or project creates false confidence. A reusable resource provides structure, not project truth.

---

## Catalog

### Available skills

| Skill | Purpose | Recommended Automatic use |
| --- | --- | --- |
| [`/feature-planner`](skills/feature-planner/README.md) | Critically plan a feature or larger product change before implementation | **On**, only with a narrow planning trigger |
| [`/rls-security-review`](skills/rls-security-review/README.md) | Review Supabase authorization, RLS, grants, tenant isolation, RPCs, views, storage, edge functions, and realistic bypass paths | **Off** |
| [`/production-readiness`](skills/production-readiness/README.md) | Assess a concrete release and return an evidence-based launch decision | **Off** |
| [`/safe-database-migration`](skills/safe-database-migration/README.md) | Review an existing schema or data change for safety, compatibility, locking, RLS impact, recovery, and validation | **On** |
| [`/multi-tenant-isolation-review`](skills/multi-tenant-isolation-review/README.md) | Audit tenant isolation across data, roles, storage, privileged paths, search, exports, caches, realtime, jobs, and lifecycle operations | **Off** |

See [Automatic use recommendations](guides/automatic-use.md) for trigger boundaries and rationale.

### Workspace Knowledge

| Resource | Purpose |
| --- | --- |
| [Universal Workspace Knowledge](workspace-knowledge/UNIVERSAL.md) | Ready-to-use engineering baseline that adapts to the real project stack and established conventions |
| [Workspace Knowledge Template](workspace-knowledge/TEMPLATE.md) | Configurable version for approved tools, providers, naming rules, checks, and workspace-specific restrictions |
| [Workspace Knowledge guide](workspace-knowledge/README.md) | Scope, precedence, setup, maintenance, and separation from Project Knowledge |

### Project Knowledge

| Resource | Purpose |
| --- | --- |
| [General Project Knowledge Template](project-knowledge/TEMPLATE.md) | Neutral configurable context for one concrete Lovable application |
| [SaaS Project Knowledge Template](project-knowledge/SaaS.md) | SaaS-specific context for tenants, memberships, permissions, billing, entitlements, integrations, async work, realtime, and cross-tenant testing |
| [Marketplace Project Knowledge Template](project-knowledge/Marketplace.md) | Marketplace-specific context for buyers, sellers, operators, listings, orders, payments, payouts, inventory, reviews, disputes, moderation, and party boundaries |
| [CRM Project Knowledge Template](project-knowledge/internal-tools/CRM.md) | Internal CRM context for ownership, pipelines, activities, communication, imports, automation, reporting, deduplication, consent, and role boundaries |
| [Project Knowledge guide](project-knowledge/README.md) | Template selection, scope, setup, precedence, maintenance, and separation from Workspace Knowledge |

### Planned skills

| Skill | Purpose |
| --- | --- |
| `/webhook-reliability-review` | Review signature validation, replay protection, idempotency, duplicates, ordering, retries, partial processing, dead-letter handling, safe logging, reconstruction, and reprocessing |
| `/incident-investigator` | Preserve evidence and investigate symptoms, timelines, deployments, logs, system state, hypotheses, impact, and safe containment before proposing repairs |
| `/data-lifecycle-review` | Trace personal and business data from collection through storage, access, transfer, export, retention, account deletion, linked records, and backups without claiming legal certification |
| `/schema-integrity-review` | Review nullability, uniqueness, foreign keys, deletion behavior, orphaned records, status values, timestamps, indexes, and consistency |
| `/state-consistency-review` | Review React state for duplicate sources of truth, stale data, optimistic updates, races, cache invalidation, and loading, error, and form states |
| `/performance-root-cause-analysis` | Measure and isolate bundle, rendering, query, index, request, media, blocking-code, and Core Web Vitals problems with before-and-after evidence |

### Guides

- [How to use this cookbook](guides/how-to-use-this-cookbook.md)
- [Automatic use recommendations](guides/automatic-use.md)
- [Workspace Knowledge guide](workspace-knowledge/README.md)
- [Project Knowledge guide](project-knowledge/README.md)

### Checklists

- [Pre-build checklist](checklists/pre-build.md)
- [Pre-launch checklist](checklists/pre-launch.md)

---

## Repository structure

```text
Lovable-Cookbook/
├── skills/                     # Published Lovable skills
│   └── <skill-name>/
│       ├── README.md           # Human-readable documentation
│       ├── SKILL.md            # Authoritative skill instructions
│       └── references/         # Optional supporting modules
├── workspace-knowledge/        # Cross-project workspace rules
│   ├── README.md
│   ├── UNIVERSAL.md
│   └── TEMPLATE.md
├── project-knowledge/          # General and product-specific project templates
│   ├── README.md
│   ├── TEMPLATE.md
│   ├── SaaS.md
│   ├── Marketplace.md
│   └── internal-tools/         # Templates for internal operational applications
│       └── CRM.md
├── templates/                  # Reusable contribution templates
├── guides/                     # Practical workflows and explanations
├── examples/                   # Worked examples
├── checklists/                 # Build and release quality gates
├── .github/                    # Repository configuration
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── SECURITY.md
└── LICENSE
```

---

## Quality standard

A useful contribution should make clear:

1. when it should and should not be used;
2. which context and assumptions are required;
3. what process or decision logic applies;
4. what the result must contain;
5. how the result can be validated;
6. which limits, risks, and failure cases remain;
7. whether Automatic use is appropriate;
8. whether the content is workspace-wide or project-specific.

The repository does not accept vague prompt dumps, unsupported guarantees, hidden project context, copied proprietary material, unsafe workflows, or claims of verification without evidence.

---

## Contributing

Contributions are welcome when they solve a repeatable problem, state their boundaries, and can be reviewed objectively.

Read the [Contributing guide](CONTRIBUTING.md), [Code of Conduct](CODE_OF_CONDUCT.md), and [Security policy](SECURITY.md) before contributing.

## Security

Do not report vulnerabilities, exposed credentials, or private data in public issues. Follow [`SECURITY.md`](SECURITY.md).

## License

Lovable Cookbook is released under the [MIT License](LICENSE).

---

<p align="center">
  Built for people who want Lovable to produce maintainable products, not just fast demos.
</p>
