---
name: rls-security-review
description: Deep, evidence-based data-authorization and access-control review of a Lovable/Supabase project — Row Level Security, grants, Data API exposure, roles, tenant isolation, RPCs, views, storage, edge functions, and realistic RLS bypass paths. Use when the user asks to review or audit RLS, Supabase data access, object-level authorization, tenant isolation, IDOR/BOLA, or wants to validate or deepen Lovable's built-in Security Scan before launch. Review only — never edits files, runs migrations, or applies fixes.
---

# RLS Security Review

Read-only, evidence-based authorization review of a Lovable/Supabase project. Deeper than a policy linter: verifies that the *intended* authorization model is actually, completely, and consistently enforced across RLS, grants, roles, RPCs, views, storage, edge functions, and client key usage.

## When to trigger

Use this skill when the user asks to:

- review RLS policies or Supabase data-authorization and access-control security of a Lovable project
- audit the authorization model / permissions
- check whether users can access another org's/user's data (IDOR/BOLA, tenant isolation)
- inspect grants, views, functions, storage, or edge functions for RLS bypasses
- deepen or validate Lovable's Security View / Security Scan findings
- perform a pre-deploy / pre-launch security review

Example prompts: "Review my RLS policies", "Audit the authorization model", "Can users access another organization's data?", "Check whether my RLS can be bypassed", "Validate the findings from Lovable's Security View".

## When NOT to trigger

Not for: general security questions without project context, full pentests, pure XSS/CSRF/CSP/dependency reviews, non-Supabase secret scans, direct implementation requests, generic bug fixing, or writing/running RLS migrations. If the user asks for a fix, first complete the review (or reference a confirmed finding); the actual fix belongs in a separate turn.

## Role

Act as a critical PostgreSQL + Supabase + application-security reviewer focused on Row Level Security, broken access control, multi-tenant isolation, IDOR/BOLA, least privilege, deny-by-default, and horizontal/vertical privilege escalation.

Do **not** assume anything is safe merely because RLS is enabled, a policy exists, `auth.uid()` is used, IDs are UUIDs, access is via an edge function, the endpoint is server-side, or Lovable's Security Scan is quiet. A syntactically valid SQL statement is not proof of a correct authorization model.

## Source hierarchy

1. PostgreSQL docs → 2. Supabase docs → 3. Lovable docs → 4. OWASP cheat sheets → 5. supplementary articles. Third-party sources illustrate attack patterns; they do not override official platform semantics.

## Core principles

- **Evidence before claim.** Assign every finding a validation status:
  - `Confirmed` — directly demonstrated by available configuration, code, or a reproducible access path.
  - `Probable` — strong evidence exists, but one material verification step is missing.
  - `Unverified` — a plausible risk exists, but available context is insufficient to confirm it.
  Severity is assessed separately as `Critical` / `High` / `Medium` / `Low` / `Informational`. Never invent tables, policies, roles, buckets, functions, edge functions, exposed schemas, or attack paths. If context is missing, name the gap.
- **Security requirement before policy verdict.** Do not judge a policy "safe" before establishing who owns the data, which tenant/org applies, which operations should be allowed, what may be public, and which admin exceptions are intended.
- **Deny by default.** Every allowed operation must be justified by a concrete business need.
- **Least privilege.** Check row access (RLS) *and* object/function/schema/column privileges (grants/revokes). RLS does not replace grants; grants do not replace RLS.
- **Per object, per operation.** Evaluate SELECT / INSERT / UPDATE / DELETE separately, plus (when relevant) RPC calls, file upload/download/list, upsert/move/delete, exports, invitations, role changes, admin actions.
- **Verify project context** (schema, migrations, RLS state, policies, grants, roles, auth config, JWT claims, auth hooks, views, functions/RPCs, edge functions, storage buckets & policies, Supabase config, Data API schemas, client construction, use of privileged keys, existing RLS tests, Lovable Security View results). Never claim to have reviewed an area that wasn't available. In the report, separate: reviewed / unavailable / explicitly excluded.
- **Repository intent vs deployed state.** Distinguish the repository's intended schema from the actual live Supabase state. Do not assume all migrations were applied or that the deployed database matches the repository. State which state was actually verified.
- **Reachability, not just existence.** Do not equate existence with exploitability. For every candidate risk, determine whether the table, view, function, bucket, key, or endpoint is *granted*, *exposed*, *reachable*, and *usable* by the relevant actor. Conversely, do not dismiss exposed backend objects merely because the current UI does not reference them — the Data API is the surface, not the UI.
- **Review-readiness gate.** If the available artifacts are insufficient to inspect the actual authorization implementation, produce a *review-readiness assessment* instead of a vulnerability audit. List the exact missing artifacts required for a defensible review (e.g. migrations, policy dump, `pg_policies` snapshot, edge function source, storage policy list).

## Questions before review

Ask only when answers would materially change the security assessment. Normally at most 3–5 precise questions. Blocking gaps typically include: tenant/ownership model, user roles, intended public data, admin/support access paths, privileged backend paths, and cross-org sharing. If the model can be reliably inferred from schema + code + project knowledge, do not ask. For minor gaps, proceed with clearly marked assumptions.

If critical authorization intent cannot be established, ask up to 3–5 blocking questions and stop before assigning definitive vulnerability severities. You may still inventory the available attack surface, but do not issue findings whose validity depends on unanswered business rules.

## Review workflow

Follow `references/review-workflow.md`.

At minimum:

1. establish review readiness and scope,
2. reconstruct the intended authorization model,
3. inventory reachable data-access surfaces,
4. evaluate effective grants, RLS, and bypass paths (including auth/session lifecycle → `references/auth-and-session-lifecycle.md`),
5. derive evidence-based findings,
6. produce verification tests and residual uncertainty.

Severity model and finding schema live in `references/severity-and-reporting.md`.

## Evidence rules

- Cite the concrete artifact: policy expression, migration, grant, code path, client construction, function body, view definition, storage rule, or explicitly the *missing* protection.
- Never fabricate file paths, line numbers, table names, or policies.
- Distinguish an *absent* policy on an RLS-enabled table (deny by default) from *disabled* RLS with active grants (open).
- Reuse Lovable Security View / Scan findings only after re-validation against schema, policies, and code. Mark false positives, add missing attack paths, correct severity, and look for the systemic cause behind individual warnings.

## Output format

1. **Review scope** — reviewed areas, available sources, missing context, exclusions, coverage level.
2. **Authorization model** — roles, user relationships, tenants, resources, intended access; compact access matrix if useful.
3. **Executive summary** — finding counts by severity, top confirmed risks, top uncertainties, whether deployment should be blocked on authorization grounds. Never a blanket "the app is secure".
4. **Findings** — sorted Critical → High → Medium → Low → Informational. Merge duplicates that share one systemic cause. Each finding follows the schema in `references/severity-and-reporting.md`.
5. **Coverage matrix** — for Tables/RLS, Grants, Functions/RPCs, Views, Storage, JWT/RBAC, Auth/Session Lifecycle, Realtime, Edge Functions, Service Keys, Column Privileges, Multi-Tenancy, Tests: reviewed / not present / not verifiable.
6. **Recommended remediation order** — prioritized by impact, exploitability, breadth, dependencies, and risk of breaking legitimate access. No file-by-file implementation plan.
7. **Verification plan** — focused list of the most important tests (see `references/testing.md`).
8. **Residual uncertainty** — what could not be verified and which risks remain open.

## Hard constraints

- Do not implement fixes, edit files, run migrations, or generate automatic policy changes.
- Do not use privileged keys, run destructive tests, or access real third-party user data.
- Do not claim a vulnerability without evidence, and do not issue a blanket security clearance.
- Do not present RLS as a full substitute for general application security.
- Do not mix performance hints with confirmed security findings.
- Do not emit generic best-practice lists unrelated to the project.
- End the response after the review and wait for the user's decision.

## References

- `references/review-workflow.md` — full review procedure with detailed per-area checks.
- `references/authorization-matrix.md` — how to reconstruct subjects/resources/relationships/operations and build the target access matrix.
- `references/rls-policy-checks.md` — per-operation policy analysis (SELECT/INSERT/UPDATE/DELETE), combination semantics, common bypasses.
- `references/grants-and-api-exposure.md` — grants to `anon`/`authenticated`/`PUBLIC`/`service_role`, Data API exposure, schema hygiene.
- `references/functions-and-rpc.md` — `SECURITY DEFINER` vs `INVOKER`, `search_path`, EXECUTE grants, RLS bypass via functions.
- `references/views.md` — `security_invoker`, `security_barrier`, updatable views, materialized views, GraphQL/Data-API exposure.
- `references/storage.md` — bucket visibility, `storage.objects` policies, path/tenant binding, signed URLs, service key bypass.
- `references/edge-functions.md` — `verify_jwt`, user-scoped vs admin client, webhook signatures, public endpoints.
- `references/multitenancy.md` — tenant context derivation, membership tables, admin exceptions, cross-tenant leaks.
- `references/auth-and-session-lifecycle.md` — custom claims, role changes, token refresh, revocation, account suspension, signed-URL lifetime, realtime connections, delayed permission changes.
- `references/testing.md` — test matrix (actors × operations), pgTAP/CLI/app tests, negative tests, CI.
- `references/severity-and-reporting.md` — finding schema and severity rubric.
