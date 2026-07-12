# Project Knowledge

Project Knowledge defines durable context for one specific Lovable project. It records the product, users, domain model, permissions, tenant boundaries, data, integrations, design rules, testing expectations, and operational constraints that should remain stable across tasks.

Workspace Knowledge still applies unless Project Knowledge defines a more specific rule for the current project.

## Available resources

### General and product models

| Resource | Purpose |
| --- | --- |
| [`TEMPLATE.md`](TEMPLATE.md) | General configurable Project Knowledge template for one concrete Lovable application |
| [`SaaS.md`](SaaS.md) | SaaS-specific template with tenant membership, roles, billing, entitlements, integrations, async processing, realtime, and cross-tenant testing |
| [`Marketplace.md`](Marketplace.md) | Marketplace-specific template for buyer, seller, operator, listing, order, payment, payout, review, dispute, inventory, moderation, and party-boundary rules |

### Internal tools

| Resource | Purpose |
| --- | --- |
| [`internal-tools/CRM.md`](internal-tools/CRM.md) | CRM-specific template for record ownership, pipelines, stage transitions, activities, communication, imports, bulk actions, automation, reporting, consent, deduplication, and role boundaries |

Additional internal-tool templates should be added under [`internal-tools/`](internal-tools/) so operational applications remain grouped separately from general product models.

## Which template should you use?

Use **`TEMPLATE.md`** when:

- the product model is still unclear;
- no specific template matches the application;
- you need the broadest neutral starting point.

Use **`SaaS.md`** when:

- the application serves organizations, workspaces, teams, or customer accounts;
- users can belong to one or more tenants;
- roles and permissions differ by tenant or resource;
- billing, plans, subscriptions, trials, or entitlements affect access;
- integrations, jobs, webhooks, realtime, search, exports, or AI features must preserve tenant boundaries.

Use **`Marketplace.md`** when:

- the product connects buyers with sellers, providers, or businesses;
- listings, orders, bookings, purchases, contracts, or fulfillment are central concepts;
- payments, platform fees, refunds, payouts, disputes, reviews, or moderation require explicit rules;
- inventory, capacity, availability, double booking, or overselling must be controlled;
- buyer-, seller-, operator-, and public-data boundaries must remain distinct.

Use **`internal-tools/CRM.md`** when:

- leads, contacts, companies, deals, activities, tasks, notes, or communications are central records;
- access depends on owner, team, tenant, role, or explicit sharing;
- pipelines, stage transitions, forecasts, assignments, imports, merges, or bulk operations require rules;
- email, calendar, telephony, forms, automations, or external CRM data must preserve record and tenant boundaries.

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

1. Choose the general template or the closest specific template.
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
