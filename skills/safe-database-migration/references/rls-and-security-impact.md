# RLS and security impact

Scope: migration-driven changes only. This is not a full authorization audit.

## Inspect

- new tables without RLS enabled
- RLS enablement changes
- new owner / user / role / tenant columns
- authorization fields that could be user-controllable
- policies referencing renamed or removed columns
- effective USING and WITH CHECK conditions after the change
- new or changed views: determine whether underlying-table privileges and RLS are evaluated using the view owner or the invoking user
- inspect `security_invoker`, view ownership, grants, `security_barrier`, updatability, and `WITH CHECK OPTION` where relevant
- functions and triggers running with elevated context (SECURITY DEFINER)
- grants on new objects (Data API requires explicit grants)
- default privileges
- sequence privileges
- realtime exposure of newly added tables/columns
- storage-linked columns
- edge functions relying on old or new structure

## Distinguish

- existing access accidentally blocked by the change
- new access accidentally granted
- the transition window with old + new app versions live
- backfill running under a privileged role
- triggers or functions that bypass RLS

`security_invoker` changes whose privileges and RLS policies are used for
underlying relations. It does not create row-level security on the view
itself.

Never rely on another skill for these migration-critical checks. If the underlying policies or grants are not visible, mark `Not verifiable` for that area.
