# Isolation testing matrix

Use only controlled test identities and test data. Never touch real foreign user data. Never use privileged keys for destructive or real tests. If a test was not actually executed, mark it Not tested or Not verifiable — never as passed.

## Identities to prepare

- User A in Tenant A (member)
- User B in Tenant A (member)
- Admin A in Tenant A
- User C in Tenant B (member)
- Admin B in Tenant B
- Invited but not yet active user
- Removed user (previously member)
- Downgraded user (role reduced)
- Global support / admin, if that role exists
- Privileged background job / service path

## Operations per resource

- Own resource
- Same-tenant resource
- Other-tenant resource
- Manipulated tenant_id
- Manipulated parent-ID
- Direct object-ID (URL, API)
- Direct API call bypassing UI
- Old invitation
- After role revocation
- After tenant switch
- Open session with stale JWT
- Open realtime connection
- Old storage or signed URL
- Export
- Search
- Analytics
- Job
- Webhook

## Recording

For each row: Actor | Tenant | Resource | Operation | Expected | Observed | Result. Result is one of Pass, Fail, Not tested, Not verifiable.

Record how each test resource was created and which tenant owns it. Do not infer ownership from the UI label alone; verify it from controlled fixture setup or authoritative test data.

## Positive and negative

For every tenant-restricted allow case, include a paired cross-tenant deny case. For intentionally public or shared resources (public directories, marketplace items, deliberately cross-tenant projects, global reference data), test the documented sharing boundary instead: permitted fields and operations must be accessible, while private fields and unauthorized mutations remain denied. Absence of the appropriate paired negative tests forbids ISOLATION VERIFIED.
