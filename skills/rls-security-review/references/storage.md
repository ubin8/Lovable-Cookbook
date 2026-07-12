# Storage

Supabase Storage object access is primarily enforced through RLS policies on `storage.objects`. Review `storage.buckets` separately when bucket metadata or bucket-management operations are exposed. Public buckets bypass RLS for object *downloads* via public URLs.

## Per-bucket checks

- **Public vs private.** Public buckets serve any object by URL without authentication. Only truly public assets belong there.
- **`bucket_id`** in every policy — policies without a bucket filter apply to all buckets and are almost always wrong.
- **Path/prefix binding.** Tenant/user isolation is typically enforced by encoding `user_id` or `tenant_id` as the first folder segment and checking it in the policy (e.g. `(storage.foldername(name))[1] = auth.uid()::text`).
- **`owner_id`** on objects — set on upload; verify it is used where intended.
- **Listing vs direct fetch.** Distinguish private authenticated object reads from public-bucket URLs. For private buckets, verify the SELECT policy used for object access. For public buckets, assume anyone with the object URL can retrieve the object; RLS still matters for operations such as listing, upload, update, move, and delete.
- **Per-operation policies:** SELECT (list/read), INSERT (upload), UPDATE (upsert/replace), DELETE, plus `move` and `copy` (which need SELECT + INSERT + DELETE combinations).
- **Signed URLs.** Treat as temporary bearer credentials, not as proof of the current user's identity. Generate only after authorization, use the shortest practical TTL, and account for the fact that they remain usable until expiry even if the user's permissions are revoked.
- **Service key usage.** Uploading/serving via the service role bypasses storage RLS entirely — check where this happens and why.

## Common failures

- Policy checks `owner = auth.uid()` but not `bucket_id` → cross-bucket access.
- Public bucket used for user-uploaded private files.
- Upload policy allows any authenticated user to write into any path (no folder-prefix check) → tenant impersonation on subsequent reads.
- Upsert flows require the correct combination of `SELECT`, `INSERT`, and `UPDATE` policies. Test whether error behavior reveals the existence or metadata of objects the caller is not authorized to inspect.
- Signed URLs generated with long TTLs and shared broadly.
- Edge function serves files via service key regardless of caller authorization.
- Ambiguous or inconsistent path parsing when policies derive tenant or owner identity from folder segments. Test empty segments, unexpected nesting, renamed objects, move/copy operations, Unicode/normalization differences, and alternate valid object names that may bypass a naive prefix rule.

## Review output

Per bucket: visibility (public/private), policies per operation, path-binding scheme, whether service-role paths exist, and whether all sensitive operations (upload/list/download/delete/move) match the authorization matrix.
