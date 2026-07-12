# Build, Deployment, and Environment

Goal: confirm the exact version intended for release actually builds, is configured for production, and does not carry dev/test artifacts.

## Build and runtime
- Build succeeds without errors or blocking warnings for the version under review.
- No blocking runtime errors in preview for critical routes.
- Route tree, root layout, and required entry files are intact.
- Placeholder pages, template boilerplate, and demo content are removed from launch-critical routes.

## Deployment & publish configuration
- Correct target domain / custom domain / HTTPS.
- Redirects (root, www, trailing slash, legacy paths) match the launch plan.
- No `localhost`, preview, or `*.lovableproject.dev` URLs in production code paths (webhook targets, OAuth callbacks, share URLs, canonical/OG URLs).
- Preview URL not used as production identity.
- The version actually intended for launch is the version being reviewed. If preview and published snapshots differ, call it out.

## Environment variables & secrets
- Required production variables are present and set to production values.
- No secrets in client-public variables (e.g. `VITE_*` / `import.meta.env` exposes only publishable/anon values).
- Service-role / secret keys never referenced from the client bundle or public routes.
- No debug/dev/verbose flags left enabled.
- Third-party keys (payments, email, AI) are production keys, not test/sandbox — unless the launch stage is deliberately a sandbox pilot.
- OAuth apps configured for the production domain (redirect URIs, allowed origins).

## Integrations
- Payment provider mode matches launch stage (live vs test); webhooks point to the production endpoint.
- Email/SMS provider is production; sender identity/domain verified.
- Webhook secrets configured; endpoints verify signatures (see `security-gate.md`).
- External APIs use production endpoints with correct quotas.

## Environment evidence rules

Project files can confirm that an environment variable is required and how it is used, but they normally cannot confirm the deployed secret value. Classify production environment values as `Confirmed` only when they are visible through the relevant deployment configuration, provider configuration, or safely observable production behavior.

Never request or print secret values. Verification should establish presence, environment, and intended provider/account without exposing the credential itself.

## Evidence classes
Mark each item as one of:
- Confirmed from an authoritative configuration source or safely observed deployed behavior.
- Inferred from code (config value not directly visible, but code path implies it).
- Not verifiable from here (dashboard, DNS, provider console) — record explicitly and, if launch-relevant, add as `Required before launch` with a verification step for the owner.
