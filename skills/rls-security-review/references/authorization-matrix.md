# Authorization matrix

Goal: an explicit, compact statement of *who may do what to which resource under which relationship*, derived from evidence (schema, code, project knowledge) or clearly marked as assumption.

## Dimensions

- **Actor:** anonymous, authenticated user, org member, org admin, global admin, service.
- **Resource:** table (or logical entity), and where relevant a subset (own rows, tenant rows, published rows).
- **Relationship:** owner, member, admin, none.
- **Operation:** SELECT / INSERT / UPDATE / DELETE, plus special actions (invite, role change, publish, export, moderate, upload, download, list, move).

## Template

| Actor | Resource | Relationship | SELECT | INSERT | UPDATE | DELETE | Notes |
|-------|----------|--------------|--------|--------|--------|--------|-------|
| anon | posts | — | published only | ✗ | ✗ | ✗ | assumption: blog is public |
| user | posts | owner | ✓ | ✓ | own only | own only | |
| user | posts | other | published only | ✗ | ✗ | ✗ | |
| org_admin | invoices | same org | ✓ | ✓ | ✓ | ✗ | deletion via soft-delete only |
| billing_worker | invoices | assigned operation | scoped | scoped | scoped | ✗ | privileged backend path; exact scope required |

Never use `service | * | *` as an assumed default. Privileged backend actors must also be scoped to the minimum resources and operations required.

## Rules for the matrix

- One row per (actor, resource, relationship) triple that changes the answer.
- Mark cells as `✓`, `✗`, or a scoped condition (e.g. `own only`, `same tenant`, `published only`).
- Flag every cell that is an assumption rather than a documented requirement.
- Include special actions as extra columns when they exist (invite, role change, publish, export, upload, download, list, move, delete).
- Split horizontal escalation (peer-to-peer, cross-tenant) from vertical escalation (user → admin).

## Use during review

Every finding should reference the matrix cell(s) it violates or leaves unenforced. Every policy should map to one or more cells. Unmapped policies and unmapped cells are both suspicious:

- Policy without a matrix cell → possibly unintended access path.
- Required matrix cell without an enabling grant or policy → functional authorization gap, not automatically a security vulnerability. Report separately unless the failure creates an unsafe fallback or inconsistent privilege path.
