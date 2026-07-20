---
name: rls-policy-designer
description: Design or refactor PostgreSQL Row Level Security for Lovable and Supabase projects. Use when the user wants a new authorization model, new RLS policies, a simpler replacement for existing policies, or migration-ready policy SQL. The skill models authorization before SQL, refuses to guess critical access rules, validates the proposed change set, and never executes migrations.
---
# RLS Policy Designer

## Purpose

Design the smallest correct Row Level Security system for an existing Lovable or Supabase project. This is an authorization-design workflow, not a generic SQL generator.

The governing sequence is:

```text
Discover evidence
→ model authorization
→ build the authorization matrix
→ resolve access-changing ambiguity
→ choose the policy strategy
→ generate migration-ready SQL
→ validate effective access
→ define tests
→ document rollout and rollback
```

SQL is a consequence of the authorization model. It is never the starting point.

## Trigger

Use this skill when the user asks to:

- create RLS policies for one or more PostgreSQL or Supabase resources;
- design ownership-, membership-, tenant-, role-, sharing-, invitation-, public-, admin-, or service-based access;
- secure storage objects, RPC operations, views, or functions as part of an RLS design;
- simplify, replace, or refactor existing policies;
- remove duplicated or overlapping policies without silently widening access;
- fix inconsistent `USING` and `WITH CHECK` behavior;
- create an authorization model or operation-level authorization matrix;
- generate a policy migration, validation plan, and authorization tests;
- compare current and intended effective access before remediation.

## Non-trigger cases

Do not use this skill for:

- a read-only RLS or authorization audit;
- a general application security audit;
- frontend route guards or UI visibility only;
- non-PostgreSQL access control;
- generic SQL formatting;
- database tuning unrelated to RLS;
- live migration execution;
- incident response, data recovery, secret management, or authentication-provider setup.

Use `/rls-security-review` when the requested output is an independent read-only audit with findings, severity, exploitability, and bypass analysis. Use this skill when the requested output is a new or corrected authorization design and migration-ready SQL.

## Modes

### Design

Use when RLS is missing, incomplete, or must be created for a new authorization model.

```text
Discovery
→ Authorization Model
→ Authorization Matrix
→ Consistency Gate
→ Strategy
→ SQL
→ Validation
→ Tests
→ Rollout Notes
```

### Refactor

Use when policies already exist but overlap, duplicate logic, use unsafe broad conditions, or no longer match intended access.

```text
Discovery
→ Existing Policy Inventory
→ Effective Access Reconstruction
→ Intended Authorization Model
→ Authorization Matrix
→ Difference Analysis
→ Replacement Strategy
→ SQL
→ Validation
→ Regression Tests
→ Rollout Notes
```

Never claim “no behavior change” unless effective access was demonstrated to remain equivalent. When old policies are over-permissive, document the access intentionally removed rather than preserving insecure behavior.

## Core principles

1. **Authorization before SQL.** Establish resources, actors, tenant boundaries, ownership, memberships, roles, public access, admin access, service access, operation permissions, state constraints, and denied cases first.
2. **Deny by default.** No requirement means deny. Every allowed operation needs a concrete reason.
3. **Authentication is not authorization.** `auth.uid()` establishes identity, not ownership, membership, role, or resource access.
4. **Trusted relationships only.** Derive access from trusted foreign keys, membership rows, ownership rows, role assignments, server-controlled claims, and verified resource relationships.
5. **Separate operations.** Evaluate SELECT, INSERT, UPDATE, and DELETE independently, plus RPC, storage, exports, admin actions, service operations, and realtime where relevant.
6. **Preserve explicit scopes.** Do not collapse user, tenant, organization, project, global, and service scopes into a generic member rule.
7. **Simplicity is a security property.** Prefer the smallest correct and reviewable policy system, not merely the fewest SQL lines.
8. **Effective access matters.** Account for all applicable policies, permissive OR composition, restrictive AND composition, grants, owners, bypass roles, functions, views, storage, and service paths.
9. **Correctness before performance.** Establish access first, then review indexes, recursion, repeated membership lookups, helpers, and query plans.
10. **Repository state is not deployed state.** Keep repository, local/test database, observed remote database, and deployed application state separate.
11. **No blanket security claims.** Use `READY FOR REVIEW`, `CONDITIONAL`, or `BLOCKED`.
12. **Explicit uncertainty.** Mark material facts as Confirmed, Inferred, Unresolved, or Not inspected.

## Evidence rules

Never invent table names, columns, keys, indexes, policies, roles, membership states, tenant models, ownership, public behavior, admin semantics, storage buckets, RPCs, views, functions, service clients, JWT claims, or deployed state.

Evidence may come from:

- explicit user statements;
- Workspace Knowledge and Project Knowledge;
- schema files and migrations;
- generated database types;
- application and server code;
- Edge Functions and tests;
- configuration;
- observed database metadata;
- current policies and grants.

When sources conflict, identify the conflict, distinguish intended from observed state, prefer authoritative database evidence when available, and ask only when the unresolved choice changes effective access.

## Clarification gate

Ask no more than 3–5 focused questions per round, and only when the answer changes access. Typical blocking questions include:

- What is the tenant boundary?
- Who owns each protected resource?
- Where is membership stored?
- Which roles exist and what do they mean?
- Which rows are intentionally public?
- Are admins tenant-scoped or global?
- Which operations may trusted services perform?

If authorization intent is insufficient, stop before SQL and return an authorization-readiness report.

## Discovery

Inspect as relevant:

- schemas, tables, columns, keys, constraints, indexes, migrations, generated types;
- existing policies, RLS flags, grants, owners, bypass roles, views, functions, RPCs, triggers, and storage policies;
- application queries and mutations;
- authentication, membership, ownership, role, sharing, invitation, and lifecycle logic;
- service-role clients, Edge Functions, jobs, webhooks, realtime, exports, and admin tools;
- repository state versus observed database and deployed state.

For each protected resource record:

- actor types;
- tenant and ownership relationships;
- membership and role source;
- public behavior;
- admin and support behavior;
- service behavior;
- state restrictions;
- protected fields;
- direct and indirect access surfaces.

## Authorization model

Create one model per resource containing:

- resource and source of truth;
- tenant boundary;
- owners and members;
- roles and role scope;
- public behavior;
- admin/support behavior;
- service behavior;
- relevant lifecycle states;
- protected fields;
- trusted relationships;
- confirmed evidence and unresolved assumptions.

Do not continue to final SQL until all access-changing model gaps are resolved.

## Authorization matrix

Build an operation-level matrix with positive and negative actors.

| Resource | Actor | Relationship | Operation | Condition | Decision | Protected fields | Evidence | Confidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Cover SELECT, INSERT, UPDATE, and DELETE separately. Add RPC, storage, export, invitation, admin, service, and realtime operations where applicable.

Include negative rows such as anonymous, authenticated non-member, cross-tenant member, lower role, suspended or removed member, non-owner, stale session, mismatched service scope, and invalid resource state.

## Consistency gate

Before strategy or SQL, verify:

- every allowed operation has a trusted relationship;
- every denied actor remains denied;
- tenant, ownership, membership, role, and lifecycle rules do not conflict;
- INSERT cannot choose unauthorized tenant, owner, role, status, or public values;
- UPDATE checks both old-row eligibility and new-row validity;
- protected fields cannot be changed through broad row policies;
- public, admin, support, and service exceptions are explicit and bounded;
- matrix rows map to enforceable database or trusted-server controls.

Return `BLOCKED` when the schema cannot express the approved rule safely or when access-changing intent remains unresolved.

## Strategy

Choose the simplest valid enforcement boundary for each rule:

- RLS for row visibility and row mutation scope;
- constraints for enforceable data invariants;
- column privileges, triggers, RPCs, or trusted functions for protected-field changes;
- views for intentional projections, not as assumed RLS inheritance;
- server-only paths for privileged provider or service operations;
- storage policies for object access;
- helper functions only when they materially simplify repeated trusted relationships and do not create recursion or privilege hazards.

Prefer operation-specific policies. Do not use `FOR ALL` merely to reduce policy count. Do not use `USING (true)` without an explicit public-access requirement.

## SQL generation

Generate migration-ready SQL only after the authorization gates pass.

The migration must:

- use exact known object names and schema qualification;
- enable RLS intentionally;
- target roles explicitly;
- define `USING` and `WITH CHECK` intentionally;
- account for grants and policy composition;
- preserve safe replacement ordering;
- avoid broad old and new permissive policies coexisting unintentionally;
- include supporting helpers, constraints, or indexes only when justified;
- handle protected columns and state transitions;
- identify transaction and concurrent-index limitations;
- separate code rollback from database rollback.

Never execute the migration or claim it was applied.

## Validation

Validate the proposed design and affected change set, not an unrelated full-project audit.

At minimum inspect:

- RLS enabled and FORCE RLS decisions;
- table owner and bypass roles;
- all applicable policies and their effective composition;
- SELECT, INSERT, UPDATE old-row, UPDATE new-row, and DELETE semantics;
- schema, table, sequence, function, and column grants;
- helper ownership, security mode, search path, recursion, and execute grants;
- views, RPCs, storage, Edge Functions, service clients, realtime, exports, jobs, and webhooks affected by the change;
- JWT claim source, freshness, revocation, and stale-session behavior;
- replacement order, drift, transaction limits, compatibility, rollback, and performance support.

Do not declare the system secure. Report evidence, failures, unverified areas, and residual risk.

## Tests

Define positive and negative authorization tests. Include where relevant:

- anonymous and authenticated actors;
- owner and non-owner;
- same-tenant and cross-tenant users;
- active, suspended, and removed memberships;
- lower roles, tenant admins, global admins, support, and trusted services;
- SELECT, INSERT, UPDATE, DELETE;
- unauthorized tenant, owner, role, status, public flag, provider ID, and protected-field mutations;
- valid and invalid state transitions;
- direct Data API, RPC, view, storage, realtime, export, job, webhook, and service paths;
- stale sessions and revoked access;
- refactor regression differences.

Compilation or policy creation alone is not authorization validation.

## Output when blocked

```text
# RLS Authorization Readiness
## Mode and Scope
## Confirmed Model
## Blocking Unknowns
## Evidence
## Unsafe Assumptions Avoided
## Minimum Required Input
## Decision
BLOCKED — INSUFFICIENT AUTHORIZATION INTENT
```

Do not include speculative policy SQL.

## Full output

```text
# RLS Policy Design
## 1. Mode and Scope
## 2. Environment Evidence
## 3. Authorization Model
## 4. Authorization Matrix
## 5. Consistency Result
## 6. Strategy
## 7. Existing Policy Assessment
## 8. Migration SQL
## 9. Policy Explanation
## 10. Complexity and Performance Review
## 11. Validation
## 12. Test Plan
## 13. Rollout and Rollback
## 14. Residual Uncertainty
## Decision
READY FOR REVIEW / CONDITIONAL / BLOCKED
```

`READY FOR REVIEW` means the proposed change set is complete enough for human review. `CONDITIONAL` means only explicit non-access-changing environment or operational conditions remain. `BLOCKED` means the design cannot safely proceed.

## Hard constraints

- Never guess tenant, ownership, membership, role, public, admin, support, or service semantics.
- Never treat authentication, UI visibility, route protection, CORS, generated types, or UUID secrecy as authorization.
- Never trust client-supplied user, owner, tenant, role, price, status, approval, or permission values.
- Never omit INSERT `WITH CHECK`, UPDATE `USING`, or UPDATE `WITH CHECK` analysis.
- Never assume UPDATE policies protect sensitive columns.
- Never ignore permissive/restrictive composition, grants, owners, `BYPASSRLS`, views, functions, RPCs, storage, or service paths.
- Never expose service-role credentials or use service role as a substitute for user authorization.
- Never assume storage bucket privacy replaces storage policies.
- Never generate speculative object names.
- Never drop policies without replacement and effective-access analysis.
- Never use migration guards to conceal unknown drift.
- Never claim equivalence without comparing effective access.
- Never execute migrations or issue blanket security certification.
- Always include validation, tests, rollout notes, and unresolved uncertainty.
