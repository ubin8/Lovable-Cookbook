# Project Knowledge

Project Knowledge defines durable context for one specific Lovable project. It records the product, users, domain model, permissions, tenant boundaries, data, integrations, design rules, testing expectations, and operational constraints that should remain stable across tasks.

Workspace Knowledge still applies unless Project Knowledge defines a more specific rule for the current project.

## Available resource

| Resource | Purpose |
| --- | --- |
| [`TEMPLATE.md`](TEMPLATE.md) | Configurable Project Knowledge template for one concrete Lovable application |

## When to use Project Knowledge

Use Project Knowledge for durable facts and rules such as:

- product purpose, stage, users, and non-goals;
- critical user and business flows;
- domain concepts and business rules;
- roles, permissions, tenant boundaries, and sharing rules;
- backend, authentication, schema, storage, and data sensitivity;
- account lifecycle, billing, integrations, files, jobs, search, AI, and exports;
- design language, terminology, accessibility, security, privacy, testing, and operations.

Do not use it for:

- temporary task instructions;
- one-off implementation details;
- speculative requirements that have not been approved;
- secrets, credentials, private URLs, customer data, or production tokens;
- generic rules that should apply to every project in the workspace.

## Recommended setup

1. Copy [`TEMPLATE.md`](TEMPLATE.md).
2. Replace every placeholder with verified project information.
3. Remove sections and lines that do not apply.
4. Resolve conflicts with the actual repository, connected design system, and Workspace Knowledge.
5. Add the finalized text to Lovable's Project Knowledge setting.
6. Update it when product rules, roles, architecture, data flows, integrations, or operational requirements materially change.

> [!IMPORTANT]
> Project Knowledge should describe the project as it actually exists or has been explicitly approved. It must not silently invent product rules, permissions, compliance guarantees, backup behavior, or security properties.

## Scope and precedence

Use this order when requirements overlap:

1. explicit instruction for the current task;
2. Project Knowledge;
3. connected design-system rules and established project architecture;
4. Workspace Knowledge.

Material conflicts should not be resolved silently. Preserve the safer or less destructive behavior until the conflict is clarified.

## Maintenance rule

Project Knowledge should remain concise enough to review, but complete enough to prevent repeated rediscovery of critical project facts. Remove stale rules instead of accumulating contradictory history.
