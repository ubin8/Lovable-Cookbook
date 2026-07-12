# Data profiling

Read-only checks only. Aggregate results; no raw sensitive values.

Do not include duplicate keys or other raw column values in the report
unless they are demonstrably non-sensitive and necessary for remediation.
Prefer aggregate counts. Use pseudonymized identifiers only when
necessary for remediation, and do not assume that an unsalted hash
makes sensitive values anonymous. For the normal report, aggregates
almost always suffice.

## What to measure (per change)

- affected row count and table/index size
- NULL count on target column(s)
- duplicate count for a proposed UNIQUE
- orphan count for a proposed FOREIGN KEY
- rows violating a proposed CHECK
- values that will not deterministically cast to the target type
- unknown or legacy enum values
- empty-string vs NULL
- min/max/length distributions
- invalid dates/timestamps and time-zone anomalies
- distribution and rare edge cases
- rows without owner/tenant when relevant
- inconsistent mapping tables
- large JSON blobs
- large tables and bloated indexes

## Example preflight queries

Provide concrete, safe queries. Examples:

```sql
-- NULLs before NOT NULL
SELECT count(*) FROM public.t WHERE col IS NULL;

-- Duplicates before UNIQUE (aggregate only; does not emit raw key values)
WITH duplicate_groups AS (
  SELECT count(*) AS group_size
  FROM public.t
  WHERE col IS NOT NULL
  GROUP BY col
  HAVING count(*) > 1
)
SELECT
  count(*) AS duplicate_group_count,
  coalesce(sum(group_size), 0) AS affected_row_count
FROM duplicate_groups;

-- Orphans before FK
SELECT count(*) FROM public.child c
LEFT JOIN public.parent p ON p.id = c.parent_id
WHERE c.parent_id IS NOT NULL AND p.id IS NULL;

-- Non-castable values before type change
SELECT count(*) FROM public.t
WHERE col !~ '^-?[0-9]+$';  -- adapt per target type
```

## Gates

A migration is not safe just because new data would be valid. Existing data must satisfy the target, or a bounded cleanup / backfill plan must precede it.
