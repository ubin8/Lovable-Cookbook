# Release Context and Risk Profile

Establish the concrete product and launch context before reviewing anything else. Depth of every later section is calibrated from here.

## Determine
- Product type: public marketing site, content platform, internal tool, SaaS, multi-tenant SaaS, e-commerce, payments app, AI application, MVP/beta, existing production app.
- Target users: general public, registered users, paying customers, internal staff, specific organizations.
- Launch stage: preview only, internal beta, closed pilot, public beta, regular production, expansion of an existing live app.
- Public vs internal exposure and expected concurrency.
- Data criticality: anonymous, PII, financial, health, regulated, business-confidential.
- Business-critical actions: signup, checkout, payout, content publication, messaging, external API writes.
- Integrations: payments, email/SMS, third-party APIs, webhooks, AI providers, storage.
- Multi-tenant model, role model, sharing surfaces.
- Known regulatory or contractual constraints — if the user has stated any.

## Risk profile
Classify as low / medium / high / especially critical based on blast radius, reversibility, data sensitivity, and user population. Justify the classification briefly.

## Calibration rules
- Internal tool with a handful of trusted users: skip SEO gate, keep security/data/recovery proportional.
- Public marketing site: SEO, content polish, accessibility, share metadata matter more; data/migration risk is usually small.
- Payments, PII, or multi-tenant: strict security, RLS/authorization, data, migration, and incident-response depth.
- AI apps processing sensitive input: privacy, prompt/data leakage, provider dependency, cost/abuse limits.
- MVP or private pilot: focus on core flows, safety of destructive actions, ability to recover; do not require enterprise monitoring or full compliance program.

Never treat "MVP" as a license to skip real launch blockers (secret leakage, missing RLS, broken auth, data loss risk).
