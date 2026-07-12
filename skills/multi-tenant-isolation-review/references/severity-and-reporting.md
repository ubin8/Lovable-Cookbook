# Severity and reporting

## Validation status

- Confirmed — the cross-tenant path or improper authorization effect is supported by concrete evidence (executed test, unambiguous policy/query, reproducible response).
- Probable — the unsafe mechanism is evidenced, but a material reachability or impact step is missing.
- Unverified — the risk is plausible but the mechanism, intended model, or execution path is not sufficiently evidenced.

## Severity

- Critical — public/unauthenticated access to highly sensitive data across tenants; freely reachable service-role endpoint with global write; broad global admin escalation; systematic cross-tenant access to all customer data.
- High — confirmed read of foreign tenant data; modify/delete of foreign records; tenant admin administrates another tenant; foreign private files retrievable; export or search contains foreign data; job processes data under wrong tenant; role/membership escalation.
- Medium — bounded cross-tenant information leak; count/metadata leak; stale permission after role revocation; single foreign object reachable; cache leak with limited sensitivity; unclear admin exception with realistic risk.
- Low — small metadata disclosure; defense-in-depth gap; hard-to-reach edge path; non-critical tenant information.
- Informational — missing tests; unclear documentation; hardening recommendation; unused risky path; maintainability issue without confirmed access.

## Finding shape

Each finding contains: title; validation status; severity; affected tenants or resource types; affected components; observed evidence; expected tenant boundary; concrete attack or failure path; impact; recommended remediation direction; verification test. Cite only files, functions, tables, or policies that exist — never invent line numbers.

## Output sections

See SKILL.md "Output format" for the full section list and order.

## Decision consistency (local reference)

- Open Confirmed Critical or High cross-tenant finding forces ISOLATION FAILED.
- Unresolved Confirmed cross-org write/delete/role path forces ISOLATION FAILED.
- Relevant open verifications forbid ISOLATION VERIFIED.
- ISOLATION VERIFIED requires low residual uncertainty and no open tenant-boundary violations.
- Risk acceptance is never inferred from silence; a deliberately allowed cross-tenant path must be documented, bounded, and authorized.

## Shortened output when evidence is insufficient

Do not produce a full report. Output only: known tenant model; available evidence; decision-critical missing evidence; non-verifiable tenant paths; required test identities or artifacts; open risks; final decision INSUFFICIENT EVIDENCE. Do not invent passed tests, complete resource matrices, safe RLS semantics, safe job/export logic, or global admin rules.
