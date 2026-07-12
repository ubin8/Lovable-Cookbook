# Workspace Knowledge

Workspace Knowledge defines engineering rules that apply across every Lovable project in a workspace. It provides a durable cross-project baseline, while [Project Knowledge](../project-knowledge/README.md) describes one concrete application's product, domain, architecture, permissions, and integrations.

## Available resources

| Resource | Purpose |
| --- | --- |
| [`UNIVERSAL.md`](UNIVERSAL.md) | Ready-to-use baseline for a typical Lovable workspace with React, Lovable Cloud or Supabase, PostgreSQL, authentication, storage, integrations, testing, security, and production-quality expectations |
| [`TEMPLATE.md`](TEMPLATE.md) | Configurable version with placeholders for workspace defaults, tools, required checks, and additional restrictions |

## Which version should you use?

Use **`UNIVERSAL.md`** when:

- you want a strong default immediately;
- projects in the workspace may differ in implementation details;
- existing project conventions should remain the source of truth;
- you do not yet have firm workspace-wide provider or tooling choices.

Use **`TEMPLATE.md`** when:

- the workspace has approved tools, providers, testing requirements, or component systems;
- specific technologies are required or prohibited;
- every project must pass defined quality checks;
- you want to add organization-specific architecture, security, or delivery rules.

## Recommended setup

1. Choose either the universal version or the template.
2. For the template, replace every bracketed placeholder with a deliberate value or remove the line.
3. Review the text for conflicts with existing projects.
4. Add the final text to Lovable's Workspace Knowledge setting.
5. Keep project-specific product, domain, schema, route, role, and integration facts in [Project Knowledge](../project-knowledge/README.md).
6. Revisit the rules when the workspace changes its stack, providers, security requirements, or delivery process.

> [!IMPORTANT]
> Workspace Knowledge should contain durable cross-project rules, not temporary task instructions, one-off implementation requests, secrets, credentials, or assumptions about a specific repository.

## Precedence

Workspace Knowledge applies unless a more specific source defines the current requirement. In practice, consider:

1. explicit task instructions and platform safety constraints;
2. Project Knowledge for the current project;
3. connected design-system requirements and established project conventions;
4. Workspace Knowledge defaults.

Workspace defaults should not silently replace a working project architecture. Material conflicts should be reported and resolved deliberately.

## What the baseline covers

Both versions address:

- project inspection and focused changes;
- Lovable's React, TanStack Start, and Vite environments;
- architecture, TypeScript, frontend UX, state, caching, and SSR;
- Lovable Cloud, Supabase, PostgreSQL, migrations, authentication, RLS, storage, and secrets;
- tenant and role boundaries;
- external integrations, webhooks, and background work;
- testing, reliability, performance, documentation, and completion standards.

## Maintenance rule

The universal file should remain broadly applicable and avoid hard-coding organization-specific choices. Add specialized defaults to the template or to your configured Workspace Knowledge instead of narrowing the universal baseline for every user.
