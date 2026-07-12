# Workspace Engineering Knowledge

These rules apply to all projects in this workspace unless Project Knowledge, a connected design system, or an established project convention is more specific.

## Working Principles

- Inspect relevant code, routes, dependencies, data models, migrations, integrations, and conventions before changing anything.
- Preserve existing behavior unless the task intentionally replaces it.
- Prefer focused changes and reuse existing components, hooks, utilities, schemas, and services.
- Add libraries, providers, frameworks, or abstractions only for a clear project benefit.
- State important assumptions and never claim testing, deployment, security, or verification without evidence.
- Follow more specific Project Knowledge where applicable.

## Lovable Stack

- Treat the repository and project configuration as the source of truth.
- Lovable apps are React-based and may use TanStack Start with SSR or React + Vite.
- Inspect `package.json`, routes, build files, and rendering model before stack-dependent work.
- Do not migrate between TanStack Start and Vite unless explicitly requested, supported, and separately planned.
- Preserve existing routing, data loading, styling, deployment, and SSR behavior.
- Use the existing styling/component system and connected design system.
- Prefer the connected backend: Lovable Cloud or Supabase.
- Reuse the existing PostgreSQL, authentication, storage, realtime, and server-function setup.
- Do not add a second database, auth system, storage provider, or backend without explicit approval.
- Keep third-party services behind clear integration boundaries.
- Never expose secrets or privileged operations in browser code.

## Architecture and Code

- Follow existing file structure and architectural boundaries.
- Separate UI, business logic, validation, data access, and integrations where practical.
- Keep components and functions focused; prefer explicit data flow.
- Avoid premature abstraction, unnecessary wrappers, and mutable global coupling.
- Keep provider-specific logic behind dedicated adapters or services.
- Do not rewrite working architecture merely to match preference.
- Use TypeScript consistently where supported.
- Avoid `any`; prefer explicit types, narrowing, generics, or `unknown`.
- Do not weaken types to silence errors.
- Follow existing naming, imports, exports, formatting, and file organization.
- Remove dead code, unused imports, abandoned code, mocks, and debug output.
- Do not present placeholders or incomplete handlers as production-ready.
- Comments should explain intent or constraints, not restate code.
- Handle nullable, loading, empty, success, and error states explicitly.
- Do not swallow errors or expose sensitive internals to users.

## Frontend and UX

- Reuse shared components and preserve visual and behavioral consistency.
- Critical actions need loading, disabled, success, and error states.
- Prevent duplicate submissions.
- Forms need labels, validation feedback, and usable focus behavior.
- Support relevant mobile and desktop layouts.
- In SSR projects, guard browser-only APIs and prevent request-specific data from leaking through shared server state or caches.
- Client-side validation improves UX but never replaces trusted validation or authorization.
- Hidden controls, routes, and frontend filtering are not security controls.

## Data and Backend

- Use established data-access patterns and fetch only required data.
- Handle stale, empty, retry, and failure states deliberately.
- Partition cache keys by every query or authorization factor that changes results.
- Never share user-, role-, organization-, or tenant-specific cached data across access boundaries.
- Treat client-provided owner, tenant, role, price, status, and permission values as untrusted.
- Validate critical state transitions at the database, server route, Edge Function, or another trusted boundary.
- Keep privileged operations server-side and use least privilege.
- Use PostgreSQL constraints and trusted validation for integrity.
- Store private files in protected buckets/paths; signed URLs are temporary bearer credentials.
- Keep test and production configuration separate.

## Database Changes

- Inspect schema, migrations, data, constraints, indexes, RLS, grants, views, functions, triggers, and dependent code first.
- Use versioned migrations.
- Avoid destructive changes without data and dependency analysis.
- Check compatibility when old and new app versions may overlap.
- Prefer additive, staged migrations for incompatible changes.
- Separate expansion, compatible deployment, backfill, validation, switch, and cleanup when needed.
- Regenerate and verify generated database types after contract changes.
- A code revert is not a database rollback.
- Do not use `DROP ... CASCADE` as a shortcut.
- Define pre- and post-change checks for risky migrations.
- Do not claim backup, restore, or production-data verification without evidence.

## Security

- Treat all client input as untrusted.
- Authentication proves identity; authorization determines allowed actions.
- Enforce authorization at a trusted backend or database boundary.
- Never use UI state, hidden routes, UUIDs, or hard-to-guess IDs as access control.
- Keep secrets and privileged credentials out of client bundles, source, logs, screenshots, analytics, URLs, and user-facing errors.
- Never expose service-role, database, provider, or admin credentials to the browser.
- Client-exposed environment variables must not contain secrets.
- Validate ownership, membership, tenant, role, and resource relationships for protected actions.
- Use RLS for exposed private data in Lovable Cloud or Supabase and review read/write behavior separately.
- Restrict RPCs, functions, storage operations, privileged clients, and public endpoints explicitly.
- Verify webhooks with the provider-supported method.
- Do not log passwords, tokens, full authorization headers, or unnecessary sensitive data.
- Security-sensitive failures fail closed.
- Automated scans supplement, not replace, project-specific review.

## Tenant and Role Boundaries

Apply when a project has organizations, workspaces, teams, or accounts.

- Derive tenant access from trusted membership or resource relationships.
- A client-supplied tenant ID is not proof of membership.
- Scope data, files, search, analytics, exports, caches, integrations, jobs, webhooks, and realtime to the effective tenant.
- Evaluate roles in the correct tenant/resource scope.
- Validate invitations, role changes, removals, ownership transfer, and tenant switching server-side.
- Consider stale sessions, claims, caches, signed URLs, realtime connections, and queued jobs after access changes.
- Document every intentional global or cross-tenant sharing rule.

## Integrations and Async Work

- Bind provider accounts, credentials, OAuth connections, webhook settings, and external IDs to the correct user, resource, or tenant.
- Job payloads, queue messages, provider IDs, and webhook metadata are not authorization proof.
- Verify external events before applying changes.
- Make retryable operations idempotent and handle duplicate, delayed, missing, and out-of-order events.
- Background jobs must use an explicit, server-validated resource and tenant context.
- Avoid partial processing that cannot be resumed or retried safely.
- Log enough context for diagnosis without exposing protected data.

## Testing, Reliability, and Performance

- Run available build, type, lint, and automated tests after meaningful changes.
- Add or update tests for business rules, validation, permissions, and critical state changes.
- Test allowed and denied behavior for authorization-sensitive features.
- Use browser testing for critical flows when appropriate; inspect console and network failures.
- Test relevant loading, empty, validation, error, retry, duplicate-submit, direct-route, and SSR behavior.
- Use controlled accounts and data; never use real third-party user data.
- State untested areas instead of marking them verified.
- Use bounded retries and timeouts; never retry non-idempotent work blindly.
- Avoid uncontrolled polling, infinite loops, and unbounded queries, lists, payloads, or jobs.
- Use limits/pagination for growing datasets and consider indexes for common filters, joins, and sorts.
- Do not claim scalability without evidence.

## Documentation and Prohibited Patterns

- Keep Project Knowledge, setup, architecture decisions, environment requirements, and integration assumptions current.
- Update generated types, schemas, tests, and relevant docs after contract changes.
- Record assumptions and unresolved risks.
- Do not store temporary tasks in Workspace Knowledge.

Do not:

- migrate frontend/backend stacks without approval
- create parallel auth, database, storage, or permission systems without a clear requirement
- expose secrets or privileged credentials
- use client state as authorization
- weaken validation, RLS, permissions, or constraints to make a feature work
- perform destructive changes without impact analysis
- ignore build, type, test, console, or network failures
- claim verification without evidence
- use broad rewrites where focused changes suffice
- leave debug, mock, test, or bypass behavior enabled in production

## Completion Standard

Before declaring work complete:

1. Confirm the requested behavior and affected existing behavior.
2. Run or describe relevant verification.
3. Review security, data, compatibility, SSR, and operational impact.
4. Remove temporary code and debugging output.
5. State unverified areas, assumptions, and residual risks.
