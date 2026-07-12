# RLS Security Review

A read-only, evidence-based authorization review skill for Lovable projects that use Supabase.

It goes beyond checking whether Row Level Security is enabled. The skill reviews whether the intended authorization model is consistently enforced across policies, grants, roles, RPCs, views, storage, edge functions, session handling, and tenant boundaries.

> The human-readable documentation lives in this file. The authoritative execution instructions are defined in [`SKILL.md`](SKILL.md) and its linked reference modules.

## Use this skill when

Use `/rls-security-review` when you need to:

- audit Supabase Row Level Security policies;
- verify tenant or workspace isolation;
- investigate possible IDOR or BOLA paths;
- inspect Data API exposure and grants;
- review RPCs, views, storage policies, or edge functions for bypasses;
- validate or deepen Lovable's built-in Security Scan;
- perform a pre-launch authorization review.

## What it reviews

| Area | Focus |
| --- | --- |
| Authorization model | Intended roles, ownership, tenant boundaries, and privileged actions |
| RLS policies | Coverage, command separation, predicates, ownership checks, and policy gaps |
| Grants and API exposure | Which roles can reach tables, views, functions, and schemas through the Data API |
| RPCs and functions | `SECURITY DEFINER`, search path, caller validation, privilege escalation, and bypass risk |
| Views | Security behavior, exposed columns, filters, and underlying privilege assumptions |
| Storage | Bucket visibility, object ownership, path-based checks, and policy consistency |
| Edge functions | Authentication, authorization, service-role use, input validation, and tenant context |
| Sessions and auth | Identity lifecycle, stale authorization state, invitation flows, and role changes |
| Testing | Concrete abuse cases, cross-tenant checks, and reproducible validation steps |
| Reporting | Evidence, severity, exploitability, impact, confidence, and remediation direction |

## How to use

1. Copy the complete `rls-security-review` folder, including `references/`.
2. Keep all relative paths unchanged.
3. Provide the repository, migrations, schema, policies, functions, storage configuration, edge functions, and relevant client access paths.
4. Explain the intended role and tenant model. The reviewer cannot reliably infer business authorization rules from SQL alone.
5. Treat the result as a review report, not an automatic fix. The skill is intentionally read-only.

## Files

| File | Purpose |
| --- | --- |
| [`README.md`](README.md) | Human-readable overview and usage guidance |
| [`SKILL.md`](SKILL.md) | Authoritative entry point, trigger rules, constraints, and report contract |
| [`references/review-workflow.md`](references/review-workflow.md) | Review sequence and evidence-gathering process |
| [`references/authorization-matrix.md`](references/authorization-matrix.md) | Roles, resources, actions, ownership, and tenant-boundary mapping |
| [`references/rls-policy-checks.md`](references/rls-policy-checks.md) | Detailed RLS policy review checks |
| [`references/grants-and-api-exposure.md`](references/grants-and-api-exposure.md) | Grants, schemas, Data API reachability, and public exposure |
| [`references/functions-and-rpc.md`](references/functions-and-rpc.md) | Functions, RPCs, `SECURITY DEFINER`, and privilege escalation |
| [`references/views.md`](references/views.md) | View behavior, filters, columns, and access assumptions |
| [`references/storage.md`](references/storage.md) | Buckets, objects, ownership, paths, and storage policies |
| [`references/edge-functions.md`](references/edge-functions.md) | Edge function authentication, authorization, and service-role boundaries |
| [`references/multitenancy.md`](references/multitenancy.md) | Cross-tenant isolation and tenant-context enforcement |
| [`references/auth-and-session-lifecycle.md`](references/auth-and-session-lifecycle.md) | Session, invitation, membership, and role-change lifecycle risks |
| [`references/testing.md`](references/testing.md) | Abuse-case and cross-role validation strategy |
| [`references/severity-and-reporting.md`](references/severity-and-reporting.md) | Severity calibration and finding format |

## Expected output

The skill produces a structured review that separates:

- confirmed findings;
- evidence and affected paths;
- realistic exploit scenarios;
- impact and severity;
- confidence and unresolved uncertainty;
- recommended remediation direction;
- missing evidence that prevents a reliable conclusion.

## Limitations

This skill does not modify policies, run migrations, deploy fixes, or replace penetration testing. It can miss runtime behavior that is not represented in the available repository evidence. A clean report does not prove that the application is secure.

## Related resources

- [How to use this cookbook](../../guides/how-to-use-this-cookbook.md)
- [Pre-launch checklist](../../checklists/pre-launch.md)
- [Security policy](../../SECURITY.md)
- [Contributing guide](../../CONTRIBUTING.md)
