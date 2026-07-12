# CRM Project Knowledge

Defines durable context for one Lovable CRM project. Replace placeholders, remove irrelevant sections, and keep the final text below Lovable's limit. Workspace Knowledge still applies unless this file is more specific.

## Product

- Name: [PRODUCT NAME]
- Type: [SALES / CUSTOMER SUCCESS / SERVICE / PARTNER / OTHER]
- Description: [WHAT THE CRM DOES]
- Main goal: [PRIMARY BUSINESS OUTCOME]
- Primary users: [SALES REPS / MANAGERS / ADMINS / SUPPORT / OTHER]

## CRM Model

- Lead: [MEANING AND QUALIFICATION RULES]
- Contact: [MEANING AND IDENTITY RULES]
- Company/account: [MEANING AND RELATIONSHIPS]
- Deal/opportunity: [MEANING AND COMMERCIAL RULES]
- Team/tenant boundary: [NONE / TEAM / ORGANIZATION / WORKSPACE]
- Ownership model: [USER / TEAM / SHARED / OTHER]

Rules:

- Leads, contacts, companies, and deals are distinct unless explicitly defined otherwise.
- Derive access from trusted ownership, team, tenant, sharing, and resource relationships.
- Client-supplied owner, team, tenant, stage, value, probability, or status is not authorization proof.
- Document intentional cross-team or cross-tenant sharing.

## Roles and Permissions

| Role | Scope | Allowed | Restricted |
|---|---|---|---|
| [REP] | [OWN / TEAM / TENANT] | [ACTIONS] | [LIMITS] |
| [MANAGER] | [TEAM / TENANT] | [ACTIONS] | [LIMITS] |
| [ADMIN] | [TENANT / GLOBAL] | [ACTIONS] | [LIMITS] |

- Enforce permissions at a trusted backend or database boundary.
- Users may access only records allowed by ownership, team, tenant, role, or explicit sharing.
- Managers must not gain global access unless explicitly granted.
- Admin/support access must be explicit, least-privileged, and auditable.
- UI visibility, routes, UUIDs, and client state are not authorization.
- [PROJECT-SPECIFIC ACCESS RULES]

## Critical Flows

1. [LEAD CAPTURE OR IMPORT]
2. [ASSIGNMENT AND QUALIFICATION]
3. [CONTACT OR COMPANY CREATION]
4. [DEAL CREATION AND STAGE CHANGE]
5. [TASK, NOTE, EMAIL, OR MEETING]
6. [OWNERSHIP TRANSFER]
7. [REPORTING OR FORECAST]
8. [ARCHIVE, DELETE, OR MERGE]

Critical flows need clear states, duplicate-submit prevention, trusted authorization, and data integrity.

## Records and Deduplication

- Define when a lead becomes a contact, company, or deal.
- Do not merge records solely from name or email similarity.
- Define uniqueness for email, phone, domain, external IDs, and account numbers.
- Duplicate detection must support review before destructive merge.
- Merges must preserve provenance, activities, ownership, permissions, consent, and audit history.
- A contact may belong to multiple companies only if explicitly supported.
- [PROJECT-SPECIFIC IDENTITY OR DEDUPLICATION RULES]

## Deals, Pipeline, and Ownership

- Pipelines: [LIST]
- Stages: [LIST]
- Currency model: [SINGLE / MULTI-CURRENCY]
- Value source: [MANUAL / PRODUCTS / QUOTES / OTHER]
- Probability source: [STAGE DEFAULT / MANUAL / CALCULATED]
- Assignment model: [MANUAL / ROUND ROBIN / TERRITORY / RULE-BASED / OTHER]
- Reassignment authority: [ROLES]

Rules:

- Define allowed stage transitions and who may trigger them.
- Enforce transitions and required fields at a trusted boundary.
- Do not trust client-provided value, discount, probability, forecast category, close date, or result.
- Preserve stage history and important commercial changes.
- Closed-won/lost transitions must be explicit and auditable.
- Validate ownership and reassignment server-side.
- Users must not assign records to unauthorized users, teams, or tenants.
- Define what transfers with ownership: tasks, activities, access, notifications, and forecasts.
- [PROJECT-SPECIFIC DEAL OR ASSIGNMENT RULES]

## Activities and Communication

- Activity types: [LIST]
- Task states: [OPEN / IN PROGRESS / DONE / CANCELLED]
- Note visibility: [OWNER / TEAM / TENANT / PRIVATE]
- Email/calendar providers: [LIST]
- Call provider: [PROVIDER OR NONE]

Rules:

- Bind activities to the correct user, contact, company, deal, and tenant.
- Private notes, emails, attachments, and call data need explicit visibility rules.
- Do not expose private email, attachment, call, or calendar data to unauthorized users.
- Prevent duplicates from retries, syncs, or repeated webhooks.
- Outbound messages and calendar actions need appropriate confirmation.
- [PROJECT-SPECIFIC COMMUNICATION RULES]

## Imports, Exports, Bulk Actions, and Merge

- Import sources: [CSV / API / PROVIDER / OTHER]
- Export formats: [CSV / XLSX / PDF / OTHER]
- Bulk actions: [LIST]
- Merge authority: [ROLES]

Rules:

- Validate import scope, mapping, tenant/team ownership, and permissions before writing.
- Show preview and errors before destructive or large imports.
- Do not overwrite protected fields, ownership, consent, or status without explicit rules.
- Bulk actions need clear scope, counts, confirmation, and auditability.
- Exports must enforce permissions at request and download.
- Merge/bulk operations need partial-failure reporting and safe retry.
- [PROJECT-SPECIFIC IMPORT OR BULK RULES]

## Automations and Integrations

- Integrations: [EMAIL / CALENDAR / TELEPHONY / FORMS / BILLING / OTHER]
- Automations: [ASSIGNMENT / FOLLOW-UP / STAGE CHANGE / NOTIFICATION / OTHER]
- Jobs/queues: [DESCRIPTION]
- Webhooks: [DESCRIPTION]

Rules:

- Bind provider accounts, credentials, and external IDs to the correct user, team, tenant, or record.
- Make retries idempotent and safe against duplicate events.
- Background jobs need a server-validated record and tenant context.
- Revalidate ownership, stage, and permissions before privileged automated changes.
- Do not expose tokens, secrets, or sensitive provider payloads to browser code or logs.

## Reporting and Forecasting

- Reporting scope: [OWN / TEAM / TENANT / GLOBAL]
- Forecast model: [DESCRIPTION]
- KPIs: [LIST]

Rules:

- Reports must respect source-record ownership, team, tenant, and role boundaries.
- Aggregates must not reveal restricted records.
- Define sources of truth for pipeline, forecast, conversion, and activity metrics.
- Do not retrieve globally and hide unauthorized rows only in the frontend.
- [PROJECT-SPECIFIC REPORTING RULES]

## Backend, Security, and Privacy

- Backend: [LOVABLE CLOUD / SUPABASE]
- Authentication: [METHODS]
- Sensitive data: [LIST]
- Consent/do-not-contact rules: [DESCRIPTION]
- Retention requirements: [LIST]
- Compliance/policy requirements: [LIST]

Rules:

- Use versioned migrations and constraints for enforceable integrity.
- Use RLS for exposed private data.
- Keep privileged operations server-side.
- Restrict access by user, role, team, tenant, ownership, and record relationship.
- Keep secrets and sensitive data out of client code, URLs, logs, analytics, and user-facing errors.
- Preserve consent, unsubscribe, suppression, and do-not-contact states.
- Deletion/anonymization must preserve required legal, financial, and audit records.
- Do not claim compliance, consent, backup, deletion, or access guarantees without evidence.

## Design and Content

- Visual style: [STYLE]
- Density: [COMPACT / BALANCED / SPACIOUS]
- Tone: [TONE]
- Preferred terminology: [TERMS]

Rules:

- Follow the connected design system and shared components.
- Keep record identity, owner, stage, status, and next action visible where relevant.
- Distinguish editable data, calculated values, historical facts, and system events.
- Make merge, delete, bulk edit, reassignment, and communication actions visually distinct.
- Use proportionate confirmation for high-impact actions.

## Testing

- Required checks: [BUILD / TYPE CHECK / LINT / TESTS / BROWSER TESTS]
- Critical flows to verify: [LIST]

Rules:

- Test rep, manager, admin, team, and tenant permissions separately.
- Test own records, team records, unauthorized records, reassigned records, removed users, and stale sessions.
- Test duplicate imports, repeated webhooks, failed syncs, merge conflicts, partial bulk failures, and forbidden stage changes.
- Do not mark untested behavior as verified.

## Prohibited Patterns

Do not:

- trust client-supplied owner, team, tenant, stage, value, probability, consent, or status
- merge or overwrite records without provenance and conflict handling
- expose private notes, emails, attachments, contact data, or reports
- allow users to access or mutate records outside their allowed scope
- execute bulk changes without clear scope, confirmation, and auditability
- send messages or trigger integrations without authorization
- weaken RLS, validation, constraints, or permissions to make a flow work
- claim testing, data quality, compliance, or production readiness without evidence

## Completion Standard

Verify CRM rules, ownership, permissions, stage transitions, data integrity, and applicable checks. State unverified areas, assumptions, and residual risks.
