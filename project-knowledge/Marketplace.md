# Marketplace Project Knowledge

Defines durable context for one Lovable marketplace project. Replace placeholders, remove irrelevant sections, and keep the final text below Lovable's limit. Workspace Knowledge still applies unless this file is more specific.

## Product

- Name: [PRODUCT NAME]
- Stage: [PROTOTYPE / MVP / BETA / PRODUCTION]
- Type: [PRODUCTS / SERVICES / BOOKINGS / DIGITAL GOODS / OTHER]
- Main goal: [PRIMARY BUSINESS OUTCOME]
- Primary users: [BUYERS / SELLERS / OPERATORS / OTHER]

## Marketplace Model

- Listing owner: [SELLER / PROVIDER / BUSINESS]
- Buyer: [CUSTOMER TYPE]
- Operator: [ROLE]
- Transaction: [ORDER / BOOKING / CONTRACT / PURCHASE]
- Inventory/capacity: [DESCRIPTION]
- Core resources: [LISTINGS / ORDERS / PAYMENTS / PAYOUTS / REVIEWS / DISPUTES]

Rules:

- Derive access from trusted buyer, seller, order, and resource relationships.
- Client-supplied seller, buyer, price, fee, order, or payout IDs are not authorization proof.
- Scope private data, files, analytics, exports, jobs, webhooks, and integrations to the correct party.
- Document public, buyer-visible, seller-visible, and operator-only fields.
- Platform-wide actions require an explicit operator role.

## Roles and Permissions

| Role | Scope | Allowed | Restricted |
|---|---|---|---|
| [BUYER] | [GLOBAL / RESOURCE] | [ACTIONS] | [LIMITS] |
| [SELLER] | [SELLER / RESOURCE] | [ACTIONS] | [LIMITS] |
| [OPERATOR] | [GLOBAL] | [ACTIONS] | [LIMITS] |

- Enforce permissions at a trusted backend or database boundary.
- Sellers may manage only their own listings, orders, payouts, and settings unless a team model says otherwise.
- Buyers may access only their own private orders, messages, payments, and disputes.
- Operator actions must be explicit, least-privileged, and auditable.
- [PROJECT-SPECIFIC ACCESS RULES]

## Critical Flows

1. [SELLER ONBOARDING]
2. [CREATE OR PUBLISH LISTING]
3. [SEARCH OR DISCOVERY]
4. [CHECKOUT OR BOOKING]
5. [PAYMENT AND ORDER CREATION]
6. [FULFILLMENT]
7. [REFUND OR DISPUTE]
8. [PAYOUT]

Critical flows need clear states, duplicate-submit prevention, trusted authorization, and data integrity.

## Domain and State Rules

- Listing: [OWNER, VISIBILITY, STATUS RULES]
- Order/booking: [PARTIES, STATUS RULES]
- Payment/payout: [SOURCE OF TRUTH]
- Review/dispute: [ELIGIBILITY AND RULES]
- Listing states: [LIST]
- Order states: [LIST]
- Pricing model: [FIXED / VARIABLE / QUOTE / AUCTION / OTHER]
- Inventory source: [DESCRIPTION]
- Moderation: [NONE / AUTOMATIC / MANUAL]

Rules:

- Validate seller status and listing ownership server-side.
- Do not trust client-provided price, discount, inventory, availability, fee, or status.
- Prevent overselling, double booking, and negative inventory where relevant.
- Define how price or availability changes affect carts, quotes, and pending orders.
- Public listings must exclude private seller, buyer, payment, and operational fields.
- Define and enforce allowed order-state transitions server-side.
- Calculate totals, fees, taxes, discounts, and entitlements at a trusted boundary.
- Prevent duplicate order creation and duplicate fulfillment.
- Define cancellation, expiry, no-show, partial fulfillment, and partial refund behavior.
- [PROJECT-SPECIFIC LISTING OR ORDER RULES]

## Payments, Fees, and Payouts

- Provider: [PROVIDER]
- Payment model: [DIRECT / PLATFORM / CONNECT / OTHER]
- Platform fee: [MODEL]
- Payout schedule: [DESCRIPTION]
- Refund authority: [ROLE]
- Tax responsibility: [PLATFORM / SELLER / PROVIDER / OTHER]

Rules:

- Client-supplied price, fee, payment, payout, discount, or tax values are untrusted.
- Bind provider customers, payments, transfers, refunds, and payouts to the correct order and seller.
- Verify provider webhooks with the supported method.
- A valid webhook does not prove the event belongs to the expected seller or order.
- Make payment and payout processing idempotent.
- Define failed payments, chargebacks, refunds, payout failures, and negative balances.
- Never expose provider secrets or sensitive payment payloads to browser code or logs.

## Buyer, Seller, Review, and Dispute Lifecycle

- Seller states: [PENDING / ACTIVE / SUSPENDED / REJECTED / CLOSED]
- Buyer states: [ACTIVE / SUSPENDED / DEACTIVATED / DELETED]
- Review eligibility: [RULE]
- Messaging participants: [ROLES]
- Dispute initiators: [ROLES]
- Moderator roles: [ROLES]

Rules:

- Define seller onboarding, verification, approval, suspension, and closure.
- Define what happens to listings, orders, disputes, funds, and payouts after suspension or closure.
- Restrict suspended or unverified sellers from protected commercial actions.
- Private messages and attachments are visible only to authorized participants and documented operators.
- Define moderation, appeal, and evidence-retention rules.
- Operator overrides, removals, refunds, and sanctions must be auditable.
- [PROJECT-SPECIFIC LIFECYCLE OR MODERATION RULES]

## Backend and Data

- Backend: [LOVABLE CLOUD / SUPABASE]
- Authentication: [METHODS]

| Object | Purpose | Scope | Sensitivity |
|---|---|---|---|
| [OBJECT] | [PURPOSE] | [BUYER / SELLER / ORDER / GLOBAL] | [CLASSIFICATION] |
| [OBJECT] | [PURPOSE] | [SCOPE] | [CLASSIFICATION] |

- Use versioned migrations and constraints for enforceable integrity.
- Use RLS for exposed private data.
- Keep privileged operations server-side.
- Apply retention/deletion across database, files, exports, logs, caches, and external systems.

## Files, Search, Analytics, and Exports

- Listing media: [DESCRIPTION]
- Private attachments: [DESCRIPTION]
- Search/analytics: [DESCRIPTION]
- Exports: [DESCRIPTION]

Rules:

- Authorize private files independently from UI visibility.
- Validate client-supplied paths against the actor, seller, order, and resource.
- Search may expose only fields intended for public discovery.
- Seller analytics must not reveal private data from other sellers or buyers.
- Exports must enforce source-data access boundaries.
- Aggregates must not reveal protected marketplace data.

## Integrations, Jobs, and Webhooks

- Integrations: [LIST]
- Jobs/queues: [DESCRIPTION]
- Webhooks: [DESCRIPTION]

Rules:

- Bind provider accounts, credentials, and external IDs to the correct seller, order, or platform account.
- Job payloads, provider IDs, and webhook metadata are not authorization proof.
- Background jobs need a server-validated resource and party context.
- Make retries idempotent and safe against duplicate delivery.
- Revalidate order, seller, or payment relationships before privileged processing.

## Design and Content

- Visual style: [STYLE]
- Tone: [TONE]
- Preferred terminology: [TERMS]

Rules:

- Clearly distinguish listing, cart, order, payment, refund, payout, and dispute states.
- Show fees, taxes, totals, cancellation terms, and commitments before confirmation.
- Do not promise protection, compliance, availability, or outcomes that are not implemented.
- [PROJECT-SPECIFIC DESIGN OR CONTENT RULES]

## Security and Privacy

- Sensitive data: [LIST]
- Compliance/policy requirements: [LIST]
- Project-specific security rules: [LIST]

Rules:

- Restrict access by user, role, seller, buyer, order, and resource relationship.
- Do not claim escrow, buyer protection, fraud prevention, or payment guarantees without evidence.
- Preserve records required for disputes, fraud, accounting, taxes, and legal obligations.

## Testing

- Required checks: [BUILD / TYPE CHECK / LINT / TESTS / BROWSER TESTS]
- Critical flows to verify: [LIST]

Rules:

- Test buyer, seller, and operator permissions separately.
- Test own resources, another seller's resource, another buyer's order, suspended accounts, and stale sessions.
- Test duplicate checkout, repeated webhooks, payment/refund/payout failures, inventory races, and forbidden transitions.
- Do not mark untested behavior as verified.

## Prohibited Patterns

Do not:

- trust client-supplied seller, buyer, price, fee, inventory, status, payment, or payout values
- create orders, refunds, transfers, or payouts without idempotency protection
- expose private buyer, seller, payment, dispute, or contact data
- allow sellers to access another seller's resources
- allow buyers to access another buyer's private orders or messages
- weaken RLS, validation, constraints, or permissions to make a flow work
- claim payment safety, fraud protection, testing, or production readiness without evidence

## Completion Standard

Verify marketplace rules, party boundaries, amounts, state transitions, and applicable checks. State unverified areas and residual risks.
