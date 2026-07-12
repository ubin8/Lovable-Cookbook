# Automatic use recommendations

Lovable skill settings can allow a skill to be used automatically when a request matches its description. This is useful only when the trigger is narrow, predictable, and safer than proceeding without the skill.

Automatic use should not be treated as a quality badge. It is a routing decision.

## Recommended configuration

| Skill | Automatic use | Reason |
| --- | --- | --- |
| [`/feature-planner`](../skills/feature-planner/README.md) | **On, with a narrow trigger** | Useful for explicit planning, specification, or feature-analysis requests. It must not replace direct implementation when the user asks to build something. |
| [`/rls-security-review`](../skills/rls-security-review/README.md) | **Off** | Starts a deep, read-only authorization audit. Automatic activation could turn normal Supabase or policy work into an unnecessarily broad review. |
| [`/production-readiness`](../skills/production-readiness/README.md) | **Off** | Acts as a formal release gate and can return NO-GO or INSUFFICIENT EVIDENCE. It should be started deliberately for a concrete release. |
| [`/safe-database-migration`](../skills/safe-database-migration/README.md) | **On** | Provides a read-only safety preflight before existing schema or production data is changed. This is a narrow, high-risk trigger where automatic protection is valuable. |
| [`/multi-tenant-isolation-review`](../skills/multi-tenant-isolation-review/README.md) | **Off** | Starts a broad cross-tenant audit across authorization, lifecycle, storage, privileged paths, async systems, and derived data. It requires an explicit tenant model and controlled test evidence. |

## Enable Automatic use

### `/safe-database-migration`

Recommended trigger meaning:

> Use automatically when an existing database schema or production data would be changed. Review only; never execute the migration.

Typical matches:

- rename, drop, retype, split, or merge an existing column or table;
- add or tighten `NOT NULL`, unique, check, or foreign-key constraints;
- add or remove indexes, views, functions, RPCs, triggers, or enum values;
- backfill or transform existing rows;
- change fields used by RLS, tenant ownership, realtime, storage, or running application versions.

Typical non-matches:

- greenfield data modeling with no existing data;
- generic SQL questions;
- UI-only work;
- query tuning without a schema change;
- direct migration execution;
- full RLS or production-readiness audits.

This skill acts as a safety belt because seemingly small requests can hide data loss, lock, compatibility, RLS, deployment-order, and recovery risks.

### `/feature-planner`

Enable only when its description remains limited to explicit planning intent.

Good automatic matches:

- “Plan this feature.”
- “Create a technical specification.”
- “Analyze how we should structure feature X.”
- “What must be decided before implementation?”

Do not match:

- “Implement feature X.”
- “Add this button.”
- “Build the checkout page.”
- “Fix this bug.”

When the description is broad enough to match generic feature requests, disable Automatic use.

## Disable Automatic use

### `/rls-security-review`

A full RLS review is much broader than implementing or adjusting one policy. Automatic activation may inspect the entire schema, request tenant-model evidence, create a full audit report, and block direct changes.

Use it deliberately when the user explicitly asks for an RLS, authorization, IDOR/BOLA, tenant-isolation, grant, RPC, view, storage, or edge-function audit.

Migration-specific RLS consequences are already covered by `/safe-database-migration` when a concrete schema or data change is proposed.

### `/production-readiness`

This skill reviews build evidence, critical flows, security, migrations, monitoring, privacy, accessibility, rollback, and launch ownership. It produces a formal release decision and therefore requires a specific version, environment, launch stage, and evidence set.

Do not activate it merely because a user asks to publish a page or make an app “production-ready.” Start it deliberately when a full readiness gate is intended.

### `/multi-tenant-isolation-review`

This skill is intentionally broader than a focused RLS review. It reconstructs the tenant model and reviews memberships, invitations, roles, direct object access, database policies, storage, privileged clients, external integrations, search, RAG, exports, analytics, caches, queues, jobs, webhooks, realtime, offboarding, and tenant deletion.

Automatic activation could turn requests such as “add organization switching,” “change the invitation flow,” or “let admins manage members” into a complete read-only audit instead of the requested implementation.

Start it deliberately when the user explicitly asks to verify tenant isolation, investigate cross-organization access, test BOLA/IDOR boundaries, or audit a multi-tenant SaaS authorization model.

Recommended setting:

> Automatic use: Off

## Library rule

Automatic use is appropriate when a skill:

- protects against a high-impact or irreversible action;
- has a narrow and observable trigger;
- performs a bounded task;
- does not unexpectedly replace the user's requested workflow;
- reduces risk before execution.

Manual use is better when a skill:

- starts a broad audit;
- requires substantial context or many follow-up questions;
- produces a formal approval or rejection decision;
- can block or replace normal implementation;
- is expensive or disruptive when triggered accidentally.

## Description quality

Automatic routing depends heavily on the `description` field in `SKILL.md`.

Descriptions should explicitly include:

- what requests should trigger the skill;
- what nearby requests should not trigger it;
- whether the skill is review-only;
- whether it may execute or modify anything;
- the narrow outcome it produces.

Avoid descriptions such as “helps with database work” or “reviews security.” They are too broad for predictable automatic use.
