# Edge functions

Edge functions are server-side but not automatically authorized. Two independent questions: *is the caller authenticated?* and *does the function act with user or admin privileges?*

## Checks

- **`verify_jwt`** in `config.toml` (or equivalent). `true` requires a valid Supabase JWT; `false` accepts any caller unless the function verifies auth itself.
- **`Authorization` header** propagation — user-scoped Supabase clients need the caller's bearer token to run under the user's RLS.
- **`apikey` header** — publishable/anon key is not authentication; do not treat presence as authorization.
- **Client construction inside the function:**
  - *User-scoped client* (publishable key + user bearer) → operates under the caller's RLS. Correct default for user actions.
  - *Admin/service client* (service role key) → bypasses RLS. Use only after in-function authorization.
- **`auth: 'none'` or public endpoints** require an independent, verifiable caller-authentication mechanism (HMAC signature, provider signature, mTLS, shared secret with constant-time compare) before sensitive operations.
- **Webhook signatures** — verify using the provider's official verification method or an equivalent constant-time comparison over the exact signed payload; reject on mismatch before any side effects.
- **Service-to-service** — validated bearer or signed request; never trust caller-supplied identity headers.
- **Mixed auth modes** — functions that sometimes act as user and sometimes as admin need a clear switch based on verified identity, not on request payload flags.
- **Paths that bypass RLS** — any admin-client operation on user data should be justified and logged.

## Common failures

- Function uses the service key for every operation "for convenience" → RLS effectively off for that surface.
- `verify_jwt = false` and no in-function auth on an endpoint that writes to user data.
- Webhook endpoint under a public path without signature verification.
- Reads `x-user-id` or similar header and trusts it (spoofable).
- User-scoped client instantiated without forwarding the caller's `Authorization` header, so it silently acts as `anon`.
- Admin-client operation gated only by client-side checks in the frontend.

## Review output

Per function: `verify_jwt` state, which client(s) are constructed, whether the caller identity is verified before privileged operations, and every path where the service role acts on user data.
