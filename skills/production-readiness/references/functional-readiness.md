# Functional Readiness

Verify the flows that actually matter for this launch. Do not run a generic feature crawl.

## Identify critical flows first
Derived from the product and launch context. Common examples:
- Signup, sign-in, sign-out, session restore, password reset / magic link.
- Onboarding.
- The product's core action (create → save → reload → edit → delete).
- Invitations, org/role switch, permission-scoped views.
- Checkout / payment / subscription change / refund.
- Email or notification delivery.
- Data export, account deletion.

## For each critical flow check (proportional to risk)
- Happy path completes end-to-end and persists correctly.
- Required validation and required fields behave.
- Error states: server error, validation error, network failure.
- Empty and initial states.
- Cancel, reload, and back navigation mid-flow.
- Double submission and idempotency for money/notification actions.
- Missing permission / wrong role.
- Expired or invalid session.
- Slow or failing dependent requests.
- Mobile rendering of the critical path (viewport widths the target audience actually uses).

## Evidence rules
- Use browser testing / preview interactions when available; capture the observed URL, state, and screenshots as evidence.
- A code read is not proof a flow works. Never mark a flow `Passed` unless it was actually exercised end-to-end.
- Status per flow: `Passed`, `Failed`, `Partially verified`, `Not tested`, `Not applicable` — with a one-line evidence note.

## Common launch-blocking symptoms
- Auth loop, silent auth failure after refresh, missing session persistence.
- Successful UI state but no data written (or written to the wrong tenant/user).
- Payment succeeds but no server-side fulfillment.
- Destructive action without confirmation or without server-side authorization.
- Critical route crashes on mobile or on cold load.

Never launder unverified flows into `Passed` because "the code looks right".

## Safe external-action testing

- Use provider sandbox / test mode for payments.
- Use dedicated test accounts and test recipients for email or notifications.
- Do not trigger real refunds, payouts, charges, bulk sends, or irreversible external actions.
- When production delivery cannot be safely exercised, verify configuration and test-mode behavior, then mark the production path as `Partially verified` or `Not verifiable` — never `Passed`.
