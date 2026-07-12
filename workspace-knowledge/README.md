# Workspace Knowledge

Workspace Knowledge defines engineering rules that apply across every Lovable project in a workspace. It is the shared baseline below explicit platform constraints and above project-specific defaults.

Project Knowledge is intentionally not included here yet. It will be added as a separate repository area because it serves a different scope: Workspace Knowledge applies broadly, while Project Knowledge describes one concrete application, domain, architecture, and product.

## Available resources

| Resource | Purpose |
| --- | --- |
| [`UNIVERSAL.md`](UNIVERSAL.md) | Ready-to-use baseline for a typical Lovable workspace with React, Lovable Cloud or Supabase, PostgreSQL, authentication, storage, integrations, testing, security, and production-quality expectations |
| [`TEMPLATE.md`](TEMPLATE.md) | Configurable version with placeholders for workspace defaults, providers, tools, naming conventions, required checks, and additional restrictions |

## Which version should you use?

Use **`UNIVERSAL.md`** when:

- you want a strong default immediately;
- projects in the workspace may differ in implementation details;
- existing project conventions should remain the source of truth;
- you do not yet have firm workspace-wide provider or tooling choices.

Use **`TEMPLATE.md`** when:

- the workspace has approved libraries, providers, testing tools, or naming rules;
- specific technologies are required or prohibited;
- every project must pass defined quality checks;
- you want to add organization-specific architecture, security, compliance, or release rules.

## Recommended setup

1. Choose either the universal version or the template.
2. For the template, replace every bracketed placeholder with a deliberate value or remove the line.
3. Review the text for conflicts with existing projects.
4. Add the final text to Lovable's Workspace Knowledge setting.
5. Keep project-specific product, domain, schema, route, and integration facts out of Workspace Knowledge; those belong in Project Knowledge.
6. Revisit the rules when the workspace changes its stack, providers, security requirements, or release process.

> [!IMPORTANT]
> Workspace Knowledge should contain durable cross-project rules, not temporary task instructions, one-off implementation requests, secrets, credentials, or assumptions about a specific repository.

## Precedence

Use this practical order when rules overlap:

1. platform and security constraints;
2. explicit connected design-system requirements;
3. Project Knowledge for the current project;
4. established repository conventions and actual configuration;
5. Workspace Knowledge defaults;
6. general preferences.

Workspace defaults should not silently replace a working project architecture. Conflicts should be reported and resolved deliberately.

## What the baseline covers

Both versions address:

- project inspection and focused changes;
- Lovable's React, TanStack Start, and Vite environments;
- architecture and TypeScript standards;
- frontend UX and design-system reuse;
- state, caching, and SSR isolation;
- Lovable Cloud and Supabase backends;
- PostgreSQL migrations and schema safety;
- authentication, authorization, RLS, storage, and secrets;
- tenant and role boundaries;
- external integrations, webhooks, and background work;
- testing and evidence-based verification;
- reliability, performance, documentation, and completion standards.

## Maintenance rule

The universal file should remain broadly applicable and avoid hard-coding organization-specific choices. Add specialized defaults to the template or to your configured Workspace Knowledge instead of narrowing the universal baseline for every user.
