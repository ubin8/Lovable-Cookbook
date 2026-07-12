# Severity and finding schema

## Finding schema

Every finding contains:

- **Title** — short, concrete.
- **Validation status** — one of:
  - `Confirmed` — the unauthorized behavior or complete exploit path is demonstrated from available evidence.
  - `Probable` — the insecure control is demonstrated, but one material reachability or impact step remains unverified.
  - `Unverified` — the risk is plausible, but either the control, the intended authorization rule, or the relevant execution path cannot yet be established.
- **Severity** — `Critical` | `High` | `Medium` | `Low` | `Informational`.

An `Unverified` finding must default to at most `Medium` severity. Assign higher severity only when the potential impact is extreme *and* the missing verification step is named explicitly in the finding.
- **Affected components** — table, policy, function, view, bucket, edge function, role, schema; file + location when available.
- **Observed configuration** — only documented facts.
- **Expected security model** — what access restriction is required; mark assumptions.
- **Attack path** — actor → path → action → resource → outcome. No vague "could be insecure".
- **Impact** — confidentiality / integrity / availability, cross-tenant reach, escalation potential, data sensitivity.
- **Evidence** — policy expression, migration, grant, code path, client construction, function body, view definition, storage rule, or explicitly named missing protection. No invented paths or line numbers.
- **Remediation direction** — safe direction, without executing changes (tighter `USING`, correct `WITH CHECK`, immutable ownership column, revoke grant, restrict function EXECUTE, add `security_invoker`, use user-scoped client, verify webhook signature, bind storage path to tenant, etc.).
- **Verification** — one concrete positive or negative test that both confirms the finding and later validates the fix.

## Severity rubric

Rate on actual exploitability and impact. Do not inflate severity because a best practice is violated.

**Critical** — only when demonstrably or near-certainly exploitable with outsized impact:

- Public exposure of a service or secret key.
- Unauthenticated access to highly sensitive cross-tenant data.
- Broad RLS bypass with write across the system.
- Freely reachable privileged endpoint with critical admin actions.

**High**

- Cross-tenant read or write.
- Role or admin escalation.
- Foreign records modifiable or deletable.
- Privileged function callable without sufficient access control.
- Private files of other users retrievable.

**Medium**

- Bounded unauthorized access.
- Manipulable non-critical ownership/status fields.
- Inconsistent policies with limited impact.
- Overbroad grants without a fully confirmed exploit chain.
- Stale claims with a relevant privilege window.

**Low**

- Minor information leaks.
- Defense-in-depth gaps with limited exposure.
- Insecure defaults without a currently reachable attack path.
- Minor auditability gaps.

**Informational**

- Maintainability improvements.
- Test coverage gaps.
- Policy performance without a security impact.
- Hardening suggestions.
- Unused but unnecessary grants.

## Reporting hygiene

- Merge duplicate findings that share a systemic cause; list all affected objects under one entry.
- Do not repeat generic best-practice advice unrelated to observed evidence.
- Keep performance observations in a separate section, not mixed with security findings, unless they enable a realistic availability attack.
- Never issue a blanket "the app is secure" statement. Always name residual uncertainty explicitly.
