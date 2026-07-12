# Search, RAG, exports, analytics

Tenant and permission filters must apply during retrieval, not by hiding results in the frontend.

## Search and autocomplete

- Full-text, semantic, hybrid, autocomplete, previews, counts, search RPCs.
- Tenant filter applied inside the query/RPC/index lookup.
- Global indexes must partition by tenant or filter by membership.
- Count-only endpoints leak existence — treat as data.

## RAG / vector search

- Every vector or embedding row must have an enforceable tenant relationship, either through its own tenant identifier or through a trusted, integrity-protected relation to the source document.
- Retrieval must apply the effective tenant and permission boundary before returning candidate content.
- Document access is re-validated after retrieval (embeddings persist even after ACL changes).
- Deleted/moved documents invalidate stale embeddings.
- Embedding jobs run under the correct tenant context.
- AI prompts and external embedding payloads do not include unrelated tenants' content.
- Logs and vendor telemetry do not carry tenant data unnecessarily.

## Exports

Full flow: authorization at request, tenant-scoped query, async processing, tenant-scoped storage, authorization at download, expiration and deletion.

Check: tenant filter at request AND during generation; privileged export worker uses trusted tenant context; foreign IDs in payload cannot redirect the job; role revocation between request and completion is honored where the design requires it; download authorization is re-checked; signed URL TTL is bounded; delivery is single-tenant; CSV/PDF/ZIP contents, hidden columns, metadata, filenames do not leak; parameter tampering cannot pull another org's export.

## Analytics and materialized views

A materialized view is a stored snapshot — RLS on the source tables does not automatically protect the snapshot. Check:

- Does every stored row or aggregate have an enforceable tenant or sharing relationship?
- Is tenant isolation enforced through a direct tenant key, an integrity-protected relation, a safely partitioned structure, or an authorized access layer?
- If queried directly, does the query enforce the effective tenant and permission boundary?
- Common implementations include a `tenant_id` column plus tenant-filtered queries, per-tenant materialized views, or exposure only via an authorizing function/view.
- Directly reachable via REST/GraphQL/Data API?
- Who may refresh it; under which role/context?
- Removed/deleted rows remain visible until refresh — acceptable?
- Contains foreign aggregates or detail rows?
- Counts/trends of other tenants derivable?

Dashboards, ETL, warehouse, BI: same questions.
