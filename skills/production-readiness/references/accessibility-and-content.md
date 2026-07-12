# Accessibility, Mobile, and Content

Depth depends on the project. Do not enforce full WCAG on an internal tool; do not skip basics on a public site.

## Check
- Page title, meta description, favicon.
- Social share metadata on public routes where previews matter. Use route-specific titles, descriptions, and images when the shared content meaningfully differs; a suitable global fallback is acceptable.
- No placeholder or debug text on user-visible pages ("Lorem ipsum", "TODO", "REPLACE this", template titles like "Lovable App").
- No dead or 404 links in primary navigation.
- Sensible loading, error, empty, and success states on critical screens.
- Mobile rendering of critical flows at realistic viewport widths.
- Visible focus states on interactive elements.
- Keyboard operability of critical actions.
- Form labels and understandable error messages.
- Alt text on meaningful images.
- Obvious contrast issues where reliably detectable.
- No clipped dialogs, hidden actions, or unreachable buttons on target viewports.

## SEO gating
SEO is a launch gate only when public discoverability is an actual goal. Internal dashboards, private betas, and gated apps do not need a full SEO check. When SEO is in scope, verify unique titles/descriptions per route, canonical tags, structured data where used, and a valid `robots`/`sitemap` posture.

## Evidence rules
Prefer observation (rendered DOM, screenshots) over static inference for anything that must render correctly to a real user. Do not claim WCAG compliance from a partial visual review.
