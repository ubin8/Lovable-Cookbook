# Isolation review workflow

Execute in order. Skip a step only when the area is Not present or Not applicable, and record that in the coverage table.

## 1. Reconstruct the tenant model

Document: tenant term, tenant-ID column, membership table, user-tenant relationship, multi-membership, roles, owner model, global roles, support access, shared resources, parent-child structure, active-tenant selection, tenant lifecycle. Classify each resource as global / tenant-bound / user-bound / shared-across-tenants / derived / temporary / externally stored. Distinguish explicitly: Supabase platform organization, App tenant, Lovable workspace, in-app customer organization. Never conflate.

## 2. Trust boundaries

Identify everywhere tenant context originates or changes: login, JWT, custom claims, membership query, route, URL params, client state, API payload, edge function, RPC, job, queue message, webhook, provider mapping, storage path, search filter, cache key. For each: source, trustworthiness, validation, manipulation risk, fallback, error behavior, logging.

## 3. Resource access matrix

Build the intended matrix (see resource-access-matrix.md). At minimum: normal user, tenant admin, owner, removed user, invited user, user of a different tenant, global support/admin (if any), background job / service path.

## 4. Direct object access

Test-logically: own resource; same-tenant resource; other-tenant resource; nonexistent resource; foreign parent; manipulated tenant-ID; own object-ID with foreign parent-ID; foreign object-ID with own tenant-ID. Cover URL IDs, query params, form/API payloads, parent IDs, guessable/public IDs, UUIDs, composite keys, direct REST/GraphQL/RPC/frontend requests. UUIDs are defense-in-depth only.

## 5. Database, RLS, grants

See database-and-rls.md. Cover tenant-bound tables, RLS enablement, USING/WITH CHECK per operation, membership joins, parent-child consistency, manipulable tenant columns, permissive vs restrictive policies, grants to anon/authenticated/PUBLIC, default privileges, views, materialized views, functions, RPCs, triggers, service-role paths.

## 6. Memberships, invitations, identity, and SSO

See memberships-and-invitations.md. Authentication invitation is not tenant-membership authorization.

## 7. Roles and admin boundaries

See roles-and-admin-boundaries.md. Supabase dashboard roles are not app-tenant roles.

## 8. Storage and files

See storage-and-files.md. Signed URLs are bearer credentials until expiry.

## 9. Edge functions and RPCs

See edge-functions-and-rpcs.md. Server-side is not authorized.

## 10. Search, RAG, exports, analytics

See search-rag-exports-and-analytics.md. Tenant/permission filters must apply during retrieval, not by hiding results in the frontend.

## 11. Caches and derived data

See caching-and-derived-data.md. A tenant filter in the origin query does not protect a wrongly shared cache.

## 12. Webhooks, queues, jobs, cron, realtime

See async-jobs-webhooks-and-realtime.md. Do not assume all realtime features share authorization logic — determine which are used first.

## 13. Lifecycle and offboarding

See lifecycle-and-offboarding.md. Supabase project deletion is not app-tenant deletion.

## 14. Cross-tenant test matrix

See isolation-testing.md. Use only controlled test identities and test data. Never touch real foreign user data. Untested is Not tested or Not verifiable, never passed.

## 15. Consolidate

Combine findings, coverage, residual uncertainty. Apply the decision consistency rules from SKILL.md.

## 16. Output

Follow the output format in SKILL.md and severity-and-reporting.md. End the response after the review and wait for the user's decision.
