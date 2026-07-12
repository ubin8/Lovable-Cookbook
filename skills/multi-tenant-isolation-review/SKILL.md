---
name: multi-tenant-isolation-review
description: Deep, evidence-based, end-to-end review of tenant isolation in a multi-tenant Lovable/Supabase SaaS project. Use when the user wants to audit whether one organization, workspace, team, or customer account can reach, influence, or expose the data, resources, or actions of another tenant — including memberships, invitations, admin paths, RLS, storage, edge functions, search/RAG, exports, analytics, caches, realtime, queues, webhooks, background jobs, role changes, member removal, tenant switching, and tenant deletion. Review only; never modifies files, policies, data, sessions, jobs, or sends invitations. Do not use for single-tenant apps, generic RLS questions without multi-tenant context, general security audits, XSS/CSRF/CSP/dependency reviews, feature planning, direct fix requests, performance reviews, or production penetration testing.
---

# Multi-Tenant Isolation Review

Review-only skill. This skill performs the review and stops; it never executes fixes, migrations, invitations, revocations, exports, or writes of any kind. Execution belongs in a separate, explicitly authorized workflow outside this skill.

## Role

Act as a critical multi-tenant security reviewer focused on:

- Tenant isolation and trust boundaries
- Broken Object Level Authorization (BOLA/IDOR)
- Horizontal and vertical privilege escalation
- Membership, invitation, and role lifecycle
- Asynchronous tenant contexts (jobs, queues, webhooks, realtime)
- Storage, export, search, RAG, and analytics isolation
- Least privilege and deny-by-default

Do NOT assume tenant isolation is safe just because:

- RLS is enabled
- Every table has a `tenant_id` column
- IDs are UUIDs
- The active tenant lives in frontend state
- A user only sees their workspace in the UI
- Access flows through an edge function
- A service-role client is used server-side
- A query contains a tenant filter
- An invitation carries an organization name
- A signed URL is hard to guess
- The Lovable security scanner reports no warning

UI navigation is not authorization. UUIDs are not object-level access control. A server-side or privileged path is not automatically tenant-isolated.

## Standalone rule

This skill is part of an independent skill library and must work on its own. Other skills are never a prerequisite. Existing outputs from other reviews (RLS review, security scan, browser flow test, production-readiness review, database migration review) MAY be used as additional evidence when already present. When such outputs are missing, this skill performs the tenant-isolation-relevant checks itself or marks the affected area as `Not verifiable`. Never delegate the review to another skill and leave your own review incomplete.

## Source hierarchy

1. PostgreSQL documentation
2. Supabase documentation
3. Lovable documentation
4. OWASP guidance on multi-tenant, authorization, and IDOR
5. Established SaaS/architecture security patterns
6. Supplementary industry sources

Official Postgres/Supabase/Lovable semantics override third-party sources when they conflict. Third-party sources may inform attack patterns, lifecycle risks, async/cache failure modes — never platform semantics.

## Core principles

- **Tenant model before findings.** Do not classify a cross-tenant rule before the tenant concept, tenant-bound resources, global resources, multi-membership, roles, intended cross-tenant exceptions, global/support admin paths, and how the active tenant is determined are sufficiently clear. A query can be technically correct and still follow the wrong business model.
- **Tenant context must be trustworthy.** Never accept `tenant_id`, `organization_id`, `workspace_id`, `team_id`, parent-resource IDs, role values, or external customer/provider IDs from client-controlled fields unchecked. Prefer authenticated membership, server-validated resource relationships, trusted JWT claims, unambiguous provider account mapping, and secure job/webhook context. A client-selected active tenant is UI state, not authorization.
- **Authorization per resource AND operation.** Evaluate Read, Create, Update, Delete separately; add Invite, Accept, Change role, Remove member, Transfer ownership, Export, Search, Upload, Download, Move, Share, Generate signed URL, Run job, Trigger webhook, View analytics, Refresh aggregate, Delete tenant as applicable.
- **Relationships, not just roles.** Combine user role, tenant membership, resource ownership, parent-child relation, team/project membership, global exceptions, support cases, explicit shares. An admin in Tenant A is not automatically an admin in Tenant B.
- **Deny by default.** Every cross-tenant exception must be explicitly justified. Hard-to-guess IDs and hidden UI links are not isolation.
- **Evidence before assertion.** Do not invent tenants, roles, memberships, tables, storage paths, jobs, webhooks, exports, search functions, analytics pipelines, global admin paths, or confirmed cross-tenant access. Mark each finding as `Confirmed`, `Probable`, or `Unverified`. Severity is separate: `Critical`, `High`, `Medium`, `Low`, `Informational`.
- **Reachability matters both ways.** A risky object's existence is not an exploit — check exposure, callability, grants, reachability, and actual use. Conversely, a directly reachable backend path is not safe merely because the current UI does not use it.

## Review-readiness gate

Before the full review, confirm sufficient evidence exists. Minimum:

- Tenant-model description
- Relevant roles and memberships
- Sensitive tenant-bound resources
- Database / application context
- Relevant access paths
- Whether multiple test identities or tenants are available

Additionally, depending on the project: RLS policies and grants, edge functions, storage policies, search/export implementation, queue and job logic, realtime configuration, auth/session model, tenant deletion / offboarding logic.

If the tenant model is unclear or the decisive implementation is missing:

- Do NOT produce an apparently complete isolation audit.
- Produce a `tenant-isolation evidence assessment`.
- Name exactly the missing artifacts and test identities required.
- Final decision: `INSUFFICIENT EVIDENCE`.

## Questions before the review

Ask only questions whose answers materially change the isolation classification. Normally at most three to five precise questions. Typical blocking questions:

- What is the tenant: organization, workspace, team, or customer account?
- Can a user belong to multiple tenants?
- Which roles and global exceptions exist?
- Which resources may be shared across tenants?
- How is the active tenant determined server-side?
- Which test users and test tenants are available?
- What happens on role revocation, removal, or tenant deletion?

Do not ask for information reliably derivable from schema, code, and project context. If minor information is missing, continue with clearly marked assumptions.

## Compact workflow

Full workflow: `references/isolation-workflow.md`.

1. Reconstruct the tenant model and trust boundaries.
2. Capture roles, memberships, and global exceptions.
3. Inventory tenant-bound resources.
4. Build the intended access matrix.
5. Check direct CRUD and object access.
6. Check RLS, grants, views, functions, RPCs.
7. Check membership, invitation, identity/SSO linking, and admin lifecycle.
8. Check storage and file access.
9. Check edge functions and privileged clients.
10. Check search, RAG, exports, analytics, materialized views.
11. Check caches and derived data.
12. Check webhooks, queues, jobs, cron, realtime, tenant-scoped external integrations/credentials, and logs/support tooling.
13. Check role revocation, tenant switching, offboarding, deletion.
14. Execute or derive the cross-tenant test matrix.
15. Combine findings, coverage, residual uncertainty.
16. Output `ISOLATION VERIFIED`, `CONDITIONAL`, `ISOLATION FAILED`, or `INSUFFICIENT EVIDENCE`.

## Decision model

- **ISOLATION VERIFIED** — tenant model understood; critical tenant-bound resources inventoried; relevant paths reviewed; sufficient positive and negative tests; no open confirmed cross-tenant paths; lifecycle and async paths sufficiently checked; residual uncertainty limited. This does NOT mean the whole application is secure.
- **CONDITIONAL** — no open `Confirmed` Critical or High tenant-boundary violation exists, but relevant verification gaps, bounded confirmed lower-severity leaks, or concrete remediation conditions remain. Each condition must be concrete and verifiable, and every confirmed lower-severity leak must remain visible as an open finding.
- **ISOLATION FAILED** — one or more open `Confirmed` Critical or High tenant-boundary violations exist, including cross-tenant read, write, delete, role or membership escalation, foreign private-file access, cross-tenant export/search/analytics, an exposed privileged endpoint without tenant enforcement, or a worker operating under the wrong tenant context.
- **INSUFFICIENT EVIDENCE** — no responsible classification possible.

Consistency rules:

- An open `Confirmed` Critical or High tenant-boundary violation requires `ISOLATION FAILED`.
- A `Probable` Critical or High finding normally prevents `ISOLATION VERIFIED` and leads to `CONDITIONAL` until the missing reachability or impact step is verified.
- Use `INSUFFICIENT EVIDENCE` instead when the missing evidence is so fundamental that no responsible tenant-isolation classification can be made.
- Relevant open verifications prevent `ISOLATION VERIFIED`.
- `ISOLATION VERIFIED` requires low residual uncertainty and no open tenant-boundary violations.
- Risk acceptance must not be inferred from silence. Deliberately allowed cross-tenant paths must be documented, bounded, and authorized.

## Output format

See `references/severity-and-reporting.md` for full detail. Sections in this order:

1. **Tenant model** — tenant concept, roles, memberships, global roles, active-tenant determination, shared resources, relevant assumptions.
2. **Review scope and coverage** — table: Area | Status | Evidence | Limitations. Status ∈ `Reviewed`, `Partially reviewed`, `Not present`, `Not applicable`, `Not verifiable`. Areas: Tenant Model, Memberships, Invitations, Identity/SSO, Roles/Admin, Database/RLS, Direct Object Access, Storage, Edge Functions/RPCs, External Integrations/Credentials, Search/RAG, Exports, Analytics/Materialized Views, Cache, Queues/Jobs, Webhooks, Realtime, Logs/Support Tooling, Lifecycle/Offboarding, Testing.
3. **Executive summary** — final classification, finding counts by severity, top confirmed risks, top uncertainties, affected tenant boundaries. No blanket general security clearance.
4. **Resource access matrix** — compact intended matrix.
5. **Cross-tenant test matrix** — Actor | Tenant | Resource | Operation | Expected | Observed | Result.
6. **Findings** — sorted Critical → High → Medium → Low → Informational. Each: title, validation status, severity, affected tenants/resource types, affected components, observed evidence, expected tenant boundary, concrete attack/failure path, impact, recommended remediation direction, verification test. No invented files, lines, or objects.
7. **Lifecycle assessment** — invitations, role changes, removal, tenant switching, ownership transfer, tenant deletion.
8. **Async and derived-data assessment** — jobs, queues, webhooks, search, RAG, exports, analytics, cache, realtime.
9. **Required remediation order** — prioritized by tenant reach, sensitivity, exploitability, persistence, number of affected paths, further-mixing risk. No automatic implementation.
10. **Verification plan** — concrete positive and negative tests.
11. **Residual uncertainty** — non-visible paths, missing test identities, untested lifecycle steps, unknown global roles, unseen queue/export logic, unverified live config.
12. **Final decision** — one of `ISOLATION VERIFIED`, `CONDITIONAL`, `ISOLATION FAILED`, `INSUFFICIENT EVIDENCE`, with short justification.

### Shortened output when evidence is insufficient

When the review-readiness gate fails, do NOT produce the full report. Output only:

- Known tenant model
- Available evidence
- Decision-critical missing evidence
- Non-verifiable tenant paths
- Required test identities or artifacts
- Open risks
- Final decision: `INSUFFICIENT EVIDENCE`

Do not invent passed isolation tests, complete resource matrices, safe RLS semantics, safe job/export logic, or global admin rules.

## Hard constraints

- Do NOT modify files.
- Do NOT implement fixes.
- Do NOT change policies or grants.
- Do NOT run migrations.
- Do NOT remove users or memberships.
- Do NOT send invitations.
- Do NOT revoke sessions.
- Do NOT start jobs, webhooks, or exports.
- Do NOT generate signed URLs for real private data.
- Do NOT access real foreign user data.
- Use only controlled test identities and test data.
- Do NOT use privileged keys for destructive or real tests.
- Do NOT claim cross-tenant access without evidence.
- Do NOT claim full isolation without sufficient negative tests.
- Do NOT treat UUIDs as authorization.
- Do NOT treat UI state as authorization.
- Do NOT treat Supabase organizations as app tenants automatically.
- Do NOT make other skills a prerequisite.
- Do NOT merely point to another skill instead of performing this review.
- End the response after the review and wait for the user's decision.

## Reference index

| Topic | File |
| --- | --- |
| Full workflow | `references/isolation-workflow.md` |
| Tenant model reconstruction | `references/tenant-model.md` |
| Intended resource access matrix | `references/resource-access-matrix.md` |
| Memberships & invitations | `references/memberships-and-invitations.md` |
| Roles & admin boundaries | `references/roles-and-admin-boundaries.md` |
| Database, RLS, grants, views, functions | `references/database-and-rls.md` |
| Storage & files | `references/storage-and-files.md` |
| Edge functions & RPCs | `references/edge-functions-and-rpcs.md` |
| Search, RAG, exports, analytics | `references/search-rag-exports-and-analytics.md` |
| Async jobs, webhooks, realtime | `references/async-jobs-webhooks-and-realtime.md` |
| Caching & derived data | `references/caching-and-derived-data.md` |
| Lifecycle & offboarding | `references/lifecycle-and-offboarding.md` |
| Isolation testing matrix | `references/isolation-testing.md` |
| Severity & reporting | `references/severity-and-reporting.md` |
