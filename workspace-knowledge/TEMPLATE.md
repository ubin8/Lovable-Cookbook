# Workspace Engineering Knowledge

These rules apply to every project in this workspace unless Project Knowledge,
an explicitly connected design system, or an established project convention
defines a more specific requirement.

## 1. Workspace-Specific Configuration

Approved defaults and preferences:

- Preferred backend: [LOVABLE CLOUD / SUPABASE / USE EXISTING]
- Preferred package manager: [PROJECT DEFAULT / NPM / PNPM / BUN]
- Preferred validation library: [PROJECT DEFAULT / LIBRARY]
- Preferred testing tools: [PROJECT DEFAULT / TOOLS]
- Preferred component system: [PROJECT DEFAULT / DESIGN SYSTEM]
- Preferred external providers: [LIST]
- Prohibited libraries or providers: [LIST]
- Required quality checks: [LIST]
- Additional workspace rules: [LIST]

Do not apply a configured preference when it conflicts with the actual project
stack, Project Knowledge, or an explicitly connected design system. Report the
conflict instead of silently replacing the existing architecture.

## 2. General Working Principles

- Understand the existing project before changing it.
- Inspect relevant files, dependencies, routes, components, data models,
  integrations, migrations, and conventions first.
- Preserve established behavior unless the requested change intentionally
  replaces it.
- Prefer focused changes over broad rewrites.
- Reuse existing components, utilities, hooks, schemas, services, and patterns
  before creating duplicates.
- Do not introduce a library, provider, framework, or abstraction without a
  clear project benefit.
- State assumptions when important context cannot be verified.
- Never claim that something was tested, secured, deployed, or verified without
  evidence.
- Follow Project Knowledge when it defines more specific product, architecture,
  security, design, or domain requirements.

## 3. Lovable Platform and Technology Stack

- Treat the actual repository and project configuration as the source of truth.
- Lovable applications are React-based.
- A project may use:
  - TanStack Start with React and server-side rendering, or
  - React + Vite.
- Inspect `package.json`, project configuration, routes, and build files before
  making stack-dependent decisions.
- Do not migrate between TanStack Start and React + Vite unless explicitly
  requested, technically supported, and separately planned.
- Preserve the project's existing routing, rendering, data-loading, styling,
  and deployment model.
- Use the project's existing styling system. Tailwind CSS and shadcn/ui may be
  present, but do not install, remove, or replace them merely as a preference.
- Respect connected design systems and their component instructions.
- Prefer the backend already connected to the project:
  - Lovable Cloud, or
  - Supabase.
- Reuse the existing database, authentication, storage, realtime, and
  server-side function setup.
- Do not introduce a second database, authentication platform, storage system,
  or backend without a clear requirement and explicit approval.
- External APIs, MCP-backed services, and third-party integrations may
  supplement the backend, but must be isolated behind clear integration
  boundaries.
- Do not expose backend credentials or privileged operations in browser code.

[Add additional stack restrictions here.]

## 4. Architecture

- Follow the existing file structure and architectural boundaries.
- Keep UI, business logic, validation, data access, and external integrations
  separated where practical.
- Keep components and functions focused on one clear responsibility.
- Prefer explicit data flow and predictable state transitions.
- Avoid premature abstraction and unnecessary wrapper layers.
- Avoid hidden coupling through mutable global state.
- Keep provider-specific logic behind a dedicated integration boundary.
- Do not replace working architecture solely to match personal preference.
- Use server-side execution only where it provides a real requirement such as
  secure credentials, trusted authorization, SSR, protected data access, or
  backend processing.
- Do not move trusted server logic into the browser.

[Add workspace-specific architecture rules here.]

## 5. Coding Standards

- Use TypeScript consistently where supported by the project.
- Avoid `any`; prefer explicit types, generics, narrowing, or `unknown`.
- Do not weaken types merely to silence compiler errors.
- Use descriptive names that reflect domain meaning.
- Follow existing naming, import, export, formatting, and file-organization
  conventions.
- Prefer small, readable functions over deeply nested logic.
- Remove dead code, unused imports, abandoned implementations, and temporary
  debug output.
- Do not present placeholders, mocks, or incomplete handlers as finished
  production behavior.
- Comments should explain intent, constraints, or non-obvious decisions rather
  than repeat the code.
- Handle nullable, optional, loading, empty, error, and success states
  explicitly.
- Do not silently swallow errors.
- Do not expose internal or sensitive implementation details to users.

Naming conventions:

- Components: [CONVENTION OR PROJECT DEFAULT]
- Hooks: [CONVENTION OR PROJECT DEFAULT]
- Services: [CONVENTION OR PROJECT DEFAULT]
- Database objects: [CONVENTION OR PROJECT DEFAULT]
- Environment variables: [CONVENTION OR PROJECT DEFAULT]

## 6. Frontend and User Experience

- Reuse the existing design system and shared components.
- Preserve visual and behavioral consistency.
- Do not bypass connected design-system components with one-off replacements
  without a concrete reason.
- Critical actions must have clear loading, disabled, success, and error states.
- Prevent accidental duplicate submissions.
- Forms must have clear labels, validation feedback, and usable focus behavior.
- Ensure critical flows work on relevant mobile and desktop layouts.
- Preserve SSR-safe behavior in TanStack Start projects.
- Do not use browser-only APIs during server rendering without a guarded client
  boundary.
- Client-side validation improves UX but never replaces trusted validation or
  authorization.
- Hidden controls, unavailable routes, and frontend filtering are not security
  controls.

[Add design, content, accessibility, or localization rules here.]

## 7. Data Access and Application State

- Use established project data-access and query patterns.
- Fetch only the data required for the feature.
- Handle stale, loading, empty, retry, and failure states deliberately.
- Scope cache keys to every query and authorization discriminator that changes
  the result.
- Never share user-, role-, organization-, or tenant-specific cached results
  across access boundaries.
- Treat client-provided owner, tenant, organization, role, price, status, and
  permission values as untrusted.
- Validate critical state transitions at a trusted backend or database
  boundary.
- Do not duplicate authoritative server state into unsynchronized client-only
  state.
- When SSR is used, avoid leaking one request's user-specific data into another
  request through shared server state or caches.

[Add state-management or caching conventions here.]

## 8. Backend, Database, Auth, and Storage

- Continue using the project's existing Lovable Cloud or Supabase backend.
- Use PostgreSQL schema constraints and trusted server validation for data
  integrity.
- Use the established authentication provider and session model.
- Do not create parallel user, session, or permission systems without an
  explicit requirement.
- Keep privileged database and administrative operations server-side.
- Use the least-privileged client and role required for each operation.
- Store private files in appropriately protected storage paths and buckets.
- Treat signed URLs as temporary bearer credentials.
- Keep environment-specific configuration separate between test and production.
- Never infer that a backend setting, secret, policy, backup, or deployed schema
  is correct solely because related code exists.

[Add backend-specific requirements here.]

## 9. Database and Schema Changes

- Inspect the current schema, migrations, existing data, constraints, indexes,
  policies, grants, views, functions, triggers, and dependent code before a
  schema change.
- Manage schema changes through versioned migrations.
- Do not perform destructive changes without understanding existing data and
  dependencies.
- Check backward compatibility when old and new application versions may
  overlap.
- Prefer additive and staged migrations for incompatible changes.
- Separate schema expansion, compatible application deployment, backfill,
  validation, switching, and cleanup when necessary.
- Regenerate and verify database types when schema contracts change.
- Do not treat a code revert as a database rollback.
- Do not use `DROP ... CASCADE` as a shortcut.
- Define pre-change and post-change validation for risky migrations.
- Never claim production data, backups, or restore capability were verified
  without evidence.

[Add migration requirements here.]

## 10. Security

- Treat all browser and client input as untrusted.
- Authentication establishes identity; authorization determines allowed
  actions.
- Enforce authorization at the database, Edge Function, server route, or another
  trusted backend boundary.
- Never rely on UI state, hidden elements, routes, UUIDs, or hard-to-guess
  identifiers as access control.
- Keep secrets and privileged credentials out of browser bundles, source code,
  logs, screenshots, analytics, and user-visible errors.
- Never expose service-role, secret, database, provider, or administrative
  credentials to the browser.
- Environment variables exposed to client code must never contain secrets.
- Validate ownership, membership, tenant, role, and resource relationships for
  every protected action.
- Use RLS for exposed private PostgreSQL data when using Lovable Cloud or
  Supabase.
- Review both read and write authorization, including effective `USING` and
  `WITH CHECK` behavior where applicable.
- Restrict database functions, RPCs, storage operations, privileged backend
  clients, and public endpoints explicitly.
- Verify webhook requests using the provider's supported signature-verification
  method.
- Do not log passwords, tokens, secrets, complete authorization headers, or
  unnecessary sensitive personal data.
- Security-sensitive failures must fail closed.
- Automated security scans support, but do not replace, project-specific
  security review.

[Add workspace-specific security requirements here.]

## 11. Tenant and Role Boundaries

Apply this section only when a project contains organizations, workspaces,
teams, customer accounts, or other tenant boundaries.

- Derive tenant access from a trusted membership or resource relationship.
- A client-supplied tenant identifier is not proof of membership.
- Scope database access, storage, search, analytics, exports, caches,
  integrations, jobs, webhooks, and realtime features to the effective tenant.
- Roles must be evaluated within the correct tenant or resource scope.
- Validate invitations, membership changes, role changes, removals, ownership
  transfers, and tenant switching server-side.
- Consider stale sessions, claims, caches, signed URLs, realtime connections,
  and queued jobs after permissions change.
- Intentionally shared or global resources require an explicit sharing model.

[Add tenant-specific workspace rules here.]

## 12. External Integrations and Async Processing

- Bind provider accounts, credentials, OAuth connections, webhook
  configurations, and external identifiers to the correct user, resource, or
  tenant.
- Do not treat job payloads, queue messages, provider IDs, or webhook metadata
  as authorization proof.
- Verify external events before applying changes.
- Design retryable operations to be idempotent.
- Account for duplicate, delayed, missing, and out-of-order events.
- Background jobs must operate on an explicit, server-validated resource and
  tenant context.
- Avoid partial processing that cannot be safely resumed, retried, or
  reconstructed.
- Log enough context for diagnosis without exposing secrets or sensitive data.

[Add approved providers and integration requirements here.]

## 13. Testing and Verification

- Verify relevant behavior after every meaningful change.
- Run existing build, type, lint, and automated test checks when available.
- Add or update tests when business rules, validation, permissions, or critical
  state transitions change.
- For authorization-sensitive features, test both allowed and denied behavior.
- Use browser testing for important user flows when appropriate.
- Inspect console errors and failed network requests for affected flows.
- Test relevant loading, empty, error, retry, and duplicate-submission states.
- Test SSR and direct-route loading when the project uses TanStack Start.
- Use controlled test accounts and test data.
- Never test against real third-party user data.
- If something cannot be tested, state the limitation instead of marking it as
  passed.

Required workspace checks:

- [CHECK]
- [CHECK]
- [CHECK]

## 14. Reliability and Performance

- Make failures visible and diagnosable.
- Use bounded retries and appropriate timeouts for external operations.
- Do not retry non-idempotent operations blindly.
- Preserve enough context to investigate partial failures.
- Avoid uncontrolled polling, infinite loops, unbounded queries, and unbounded
  background work.
- Consider unavailable dependencies, malformed responses, timeouts, and partial
  provider failures.
- Avoid unbounded lists, payloads, loops, and database queries.
- Use pagination or limits for datasets that can grow.
- Avoid unnecessary repeated requests and avoidable rendering work.
- Consider indexes for commonly filtered, joined, or sorted columns.
- Do not claim scalability without appropriate measurements or tests.
- Keep performance concerns separate from correctness and security findings.

[Add reliability and performance expectations here.]

## 15. Documentation and Maintainability

- Keep Project Knowledge, architecture decisions, setup instructions,
  environment requirements, and integration assumptions current.
- Document non-obvious constraints and important external dependencies.
- Update generated types, schemas, tests, and documentation after contract
  changes.
- Avoid duplicating documentation that already has an authoritative location.
- Record assumptions and unresolved risks when they affect future work.
- Do not place temporary task instructions in Workspace Knowledge.

## 16. Prohibited Patterns

Do not:

- migrate the project's frontend or backend stack without explicit approval
- introduce a parallel database, authentication, or storage platform without a
  clear requirement
- expose secrets or privileged credentials
- use client state as authorization
- silently weaken validation, RLS, permissions, or constraints
- perform destructive database changes without impact analysis
- introduce duplicate implementations when an existing shared solution is
  appropriate
- ignore build, type, test, console, or network failures
- claim verification without evidence
- perform a broad rewrite when a focused change is sufficient
- leave temporary debug, test, mock, or bypass behavior enabled in production

[Add prohibited workspace patterns here.]

## 17. Completion Standard

Before declaring work complete:

1. Confirm the requested behavior is implemented.
2. Confirm affected existing behavior still works.
3. Run or describe the relevant verification.
4. Check security, data, compatibility, SSR, and operational implications.
5. Remove temporary code and debugging output.
6. State any unverified areas, assumptions, or residual risks.
