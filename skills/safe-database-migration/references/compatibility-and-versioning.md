# Compatibility and versioning

## Classify the change

- **Additive, likely compatible**: new nullable column, new table, new optional index, new additive enum value. Still verify actual app impact.
- **Potentially incompatible**: rename, type change, `NOT NULL`, default change, tightened constraint, RPC signature change, RLS policy change, new FK cascade, changes to NULL/time semantics.
- **Destructive**: dropping columns/tables, overwriting data, removing enum values, lossy casts, merging values, mandatory-without-backfill.

## Expand–Migrate–Contract

Default pattern for incompatible changes:

1. **Expand** — add new structure without breaking existing consumers.
2. **Dual compatibility** — allow old and new reads/writes temporarily. Consider dual read, dual write, trigger-based sync, app-level sync, one-shot backfill. Dual write is not automatically safe; define source of truth and error handling.
3. **Migrate** — deterministic, resumable backfill of existing data.
4. **Validate** — completeness, equality, integrity.
5. **Switch** — controlled cutover of reads and writes.
6. **Contract** — remove old structure only when no dependency remains.

Direct rename or drop is acceptable only when actual compatibility has been demonstrated (no live consumers, no old app versions in flight, no external contracts).

## Concurrent app versions

Rollout must consider the window where old and new app versions run simultaneously. A schema that only works for the new version is a coordination requirement, not just a DDL.

## Generated types as an application contract

Treat generated database types as a versioned application contract.
Determine when types are regenerated, which application version
consumes them, and whether regenerated types would break code before or
after the corresponding schema step.

## Enum additions

An added enum value is database-additive but may still break exhaustive
application logic, generated types, validators, or ordering assumptions.
Verify when the new value becomes usable relative to transaction commit
and application deployment.
