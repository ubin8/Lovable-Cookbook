# Workspace Engineering Knowledge Template

Applies to all projects unless Project Knowledge, a connected design system, or established project conventions are more specific. Replace placeholders and remove irrelevant lines.

## Workspace Defaults

- Backend: [USE EXISTING / LOVABLE CLOUD / SUPABASE]
- Package manager: [PROJECT DEFAULT / NPM / PNPM / BUN]
- Validation: [PROJECT DEFAULT / LIBRARY]
- Testing: [PROJECT DEFAULT / TOOLS]
- Components/design system: [PROJECT DEFAULT / SYSTEM]
- Additional rules: [LIST]

Do not force preferences that conflict with the actual project. Report material conflicts.

## Working Principles

- Inspect relevant code, dependencies, routes, data models, migrations, integrations, and conventions first.
- Preserve existing behavior unless the task intentionally replaces it.
- Prefer focused changes and reuse existing components, hooks, utilities, schemas, and services.
- Add libraries, providers, frameworks, or abstractions only for a clear benefit.
- State assumptions; never claim testing, deployment, security, or verification without evidence.
- Follow more specific Project Knowledge.

## Lovable Stack

- Treat the repository and project configuration as the source of truth.
- Lovable apps may use TanStack Start with SSR or React + Vite.
- Inspect `package.json`, routes, build files, and rendering model before stack-dependent work.
- Do not migrate stacks unless explicitly requested and separately planned.
- Preserve routing, data loading, styling, deployment, and SSR behavior.
- Use the existing component system and connected design system.
- Prefer the connected Lovable Cloud or Supabase backend.
- Reuse existing PostgreSQL, auth, storage, realtime, and server-function setup.
- Do not add a second backend, database, auth, or storage platform without approval.
- Never expose secrets or privileged operations in browser code.

[Additional stack rules]

## Architecture, Code, and UX

- Follow existing structure and boundaries.
- Separate UI, business logic, validation, data access, and integrations where practical.
- Keep components/functions focused; avoid premature abstraction and mutable global coupling.
- Do not rewrite working architecture merely to match preference.
- Use TypeScript where supported; avoid `any`.
- Follow existing naming, formatting, imports, exports, and file organization.
- Remove dead code, unused imports, mocks, abandoned code, and debug output.
- Do not present placeholders or incomplete handlers as production-ready.
- Reuse shared components and preserve visual/behavioral consistency.
- Critical actions need loading, disabled, success, and error states.
- Prevent duplicate submissions.
- Forms need labels, validation feedback, and usable focus behavior.
- In SSR projects, guard browser-only APIs and prevent cross-request state leaks.
- Client validation never replaces trusted validation or authorization.
- Hidden controls, routes, and frontend filtering are not security controls.
- Do not swallow errors or expose sensitive internals.

[Additional coding, design, accessibility, or content rules]

## Data and Database

- Use established data-access patterns and fetch only required data.
- Handle stale, empty, retry, and failure states deliberately.
- Partition caches by every query or authorization factor that changes results.
- Never share user-, role-, organization-, or tenant-specific cached data across boundaries.
- Treat client-provided owner, tenant, role, price, status, and permission values as untrusted.
- Validate critical state changes at a trusted backend/database boundary.
- Keep privileged operations server-side and use least privilege.
- Use constraints for integrity; protect private files and treat signed URLs as bearer credentials.
- Before schema changes, inspect migrations, data, constraints, indexes, RLS, grants, views, functions, triggers, and dependencies.
- Use versioned migrations and prefer additive, staged changes.
- Avoid destructive changes without analysis.
- A code revert is not a database rollback.
- Do not use `DROP ... CASCADE` as a shortcut.
- Do not claim backup, restore, or production-data verification without evidence.

[Additional backend, data, or migration rules]

## Security

- Treat all client input as untrusted.
- Authentication proves identity; authorization determines allowed actions.
- Enforce authorization at a trusted backend/database boundary.
- Never use UI state, hidden routes, UUIDs, or hard-to-guess IDs as access control.
- Keep secrets and privileged credentials out of client code, logs, URLs, analytics, and user-facing errors.
- Never expose service-role, database, provider, or admin credentials to the browser.
- Validate ownership, membership, tenant, role, and resource relationships.
- Use RLS for exposed private data and review read/write behavior separately.
- Restrict RPCs, functions, storage, privileged clients, and public endpoints.
- Verify webhooks with the provider-supported method.
- Do not log passwords, tokens, full authorization headers, or unnecessary sensitive data.
- Security-sensitive failures fail closed.

[Additional security rules]

## Tenant, Integrations, and Async Work

Apply relevant rules only when these features exist.

- Derive tenant access from trusted membership/resource relationships.
- A client-supplied tenant ID is not proof of membership.
- Scope data, files, search, analytics, exports, caches, integrations, jobs, webhooks, and realtime to the effective tenant.
- Evaluate roles in the correct scope.
- Validate invitations, role changes, removals, ownership transfer, and tenant switching server-side.
- Document intentional global or cross-tenant sharing.
- Bind provider accounts, credentials, OAuth connections, webhook settings, and external IDs to the correct user, resource, or tenant.
- Job payloads, provider IDs, and webhook metadata are not authorization proof.
- Verify external events before applying changes.
- Make retryable operations idempotent and handle duplicate, delayed, missing, and out-of-order events.
- Background jobs need a server-validated resource and tenant context.
- Log enough context for diagnosis without exposing protected data.

[Additional tenant/integration rules]

## Testing and Operations

- Run available build, type, lint, and automated tests after meaningful changes.
- Add/update tests for business rules, validation, permissions, and critical state changes.
- Test allowed and denied behavior for protected features.
- Use browser testing for critical flows and inspect console/network failures where appropriate.
- Use controlled accounts/data; never real third-party user data.
- State untested areas instead of marking them verified.
- Use bounded retries/timeouts; never retry non-idempotent work blindly.
- Avoid uncontrolled polling and unbounded queries, lists, payloads, loops, or jobs.

Required checks: [LIST]

## Documentation and Prohibited Patterns

- Keep Project Knowledge, setup, architecture decisions, environment requirements, and integration assumptions current.
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
- leave debug, mock, test, or bypass behavior enabled in production

[Additional prohibited patterns]

## Completion Standard

1. Confirm requested and affected existing behavior.
2. Run or describe relevant verification.
3. Review security, data, compatibility, SSR, and operational impact.
4. Remove temporary code/debugging.
5. State unverified areas, assumptions, and residual risks.
