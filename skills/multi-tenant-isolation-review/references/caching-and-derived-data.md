# Caching and derived data

A tenant filter in the origin query does not protect a wrongly shared cache.

## Layers to consider

- Client cache (React Query, browser).
- Query cache (server memoization).
- Edge cache / CDN.
- Server cache (in-memory, Redis).
- Search cache and analytics cache.
- Export cache.
- AI/RAG cache.

## Partitioning

Every cache for tenant-bound data must include every authorization discriminator that can change the result — typically some combination of tenant, user, role, permission variant, query parameters, data version, locale, feature flags. Do not add user or role mechanically when the cached result is intentionally identical for all members of the tenant.

## Check

- Tenant A populates cache, Tenant B receives result.
- Role revocation does not invalidate cache.
- Tenant switching shows stale data.
- Global search cache serves cross-tenant results.
- Global analytics aggregates carry cross-tenant counts.
- Signed URLs or export URLs sit in a shared cache.
- Cache keys omit a discriminator (missing user, role, or tenant).
- Invalidation on delete/move/role change is missing.

## Derived data more generally

Any denormalized copy — search indexes, embeddings, aggregates, materialized views, ETL outputs, warehouse tables, analytics dashboards — inherits none of the source's RLS by itself. Each needs an explicit tenant boundary and refresh/invalidate discipline.
