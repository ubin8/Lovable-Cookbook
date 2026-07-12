# RLS policy checks (per operation)

Analyze each operation independently. A policy that reads well for SELECT can be catastrophic for UPDATE.

## SELECT

- Does the policy limit rows to what the actor should see?
- Any `USING (true)` — intentional and safe for this data?
- Are sensitive columns readable even when row access is correct? (see column-level privileges)
- Can a user reference foreign tenant/owner IDs (via joins, RPCs, views) to enumerate data?
- Do multiple permissive policies OR together into broader visibility than intended?
- Does the policy authenticate (`auth.uid() is not null`) but fail to authorize?

## INSERT

- Determine the effective `WITH CHECK` condition for INSERT. If it does not bind ownership, tenant, role, or other security-sensitive fields to trusted context (e.g. `auth.uid()`, a JWT claim, a trigger-populated column), an admitted actor may insert rows with attacker-controlled authorization attributes.
- Can the user freely set `user_id`, `owner_id`, `tenant_id`, `organization_id`, role, status, plan, `is_admin`?
- Is ownership derived server-side (default `auth.uid()`, trigger) or trusted from the client?
- Can a user create rows attributed to another user or tenant?

## UPDATE

- `USING` limits *which existing rows* may be targeted; `WITH CHECK` limits *what the new row state may be*.
- Evaluate the effective combined `USING` and `WITH CHECK` for UPDATE. If `WITH CHECK` is missing or too permissive, an allowed row can be moved into an unallowed state (reassign ownership, elevate role, change tenant, flip status).
- Can the user modify ownership, tenant, role, plan, status, admin flags, moderation state?
- Can an allowed row be pushed into a state that then bypasses further checks?
- Are sensitive columns protected via column privileges or split tables?

## DELETE

- Only the right actor/role may delete.
- No cross-tenant/foreign-object deletion via joins or cascades.
- Deleting membership/ownership rows can be a bypass mechanism — check policies on join tables.
- Cascade effects: does deleting X delete or orphan protected Y?

## Policy combination semantics

- Multiple **permissive** policies for the same command combine with `OR`. Any one that admits the row admits the operation.
- **Restrictive** policies combine with `AND` and must all pass.
- `FOR ALL` policies apply to every operation — verify explicitly for INSERT/UPDATE/DELETE, not just SELECT.
- Policies on role `PUBLIC` apply to `anon` and `authenticated`.
- Watch for policy expressions that call functions reading from other tables — that path may itself be unauthorized or recursive.

## Common bypasses

- INSERT policy without `WITH CHECK`.
- UPDATE policy without `WITH CHECK` — enables ownership/tenant/role rewrite.
- SELECT policy that only checks `auth.uid() is not null`.
- Owner column nullable and controllable by client on INSERT.
- Policy compares against a client-supplied column instead of `auth.uid()` / JWT claim.
- Membership table itself unprotected → user grants themselves membership → gains data access via the "correct" policy.
- Helper function used in policy is `SECURITY DEFINER` and reads/returns more than intended.
- Table owner performs writes and RLS silently doesn't apply. Do not report missing `FORCE ROW LEVEL SECURITY` by itself — first demonstrate that a relevant application, function, worker, or direct database path executes as the table owner and therefore bypasses ordinary RLS. `anon` and `authenticated` are not table owners, so the Data API alone does not motivate this finding.
