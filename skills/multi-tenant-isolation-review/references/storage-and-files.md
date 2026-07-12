# Storage and files

Tenant isolation must not rely solely on a client-chosen path prefix.

## Surface

- Private and public buckets.
- storage.objects policies keyed on bucket_id, name (path), owner.
- Upload, upsert, list, download, move, copy, rename, delete.
- Signed URLs and their lifetime.
- Service-role access.
- Export files, generated PDFs/CSVs, user uploads, avatars.
- File metadata (filename, custom metadata).

## Check

- If the client supplies an object path or tenant segment, validate it against the authenticated user's membership, the intended bucket, and the target resource. Do not treat the path value itself as trusted tenant context.
- User cannot list, download, move, or copy into another tenant's prefix.
- A bucket or public delivery path permits unauthenticated download of objects that the tenant model classifies as private.
- Signed URLs are generated only after full authorization and remain usable until expiry, including after role revocation.
- Downloadable public URLs never point at private tenant data.
- Export files land under the correct tenant prefix.
- Stale files remain after tenant deletion or membership removal — planned cleanup exists.
- Database backups do not automatically include Storage objects.

## Common failures

- Uploads accept a path from the client that includes tenant segment; server does not validate against membership.
- List operations use a broad prefix and rely on the client to filter.
- Signed URL lifetime exceeds the minimum practical duration for the use case or remains valid beyond the relevant membership, export, or sharing lifecycle.
- Moving an object across prefixes updates the row but leaves prior signed URLs valid.
- Public delivery path used for tenant data classified as private, with "unguessable" filenames as the only defense.
