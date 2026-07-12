# Pre-launch checklist

## Product

- [ ] Core user flows work from start to finish.
- [ ] Empty, loading, success, and error states are handled.
- [ ] Destructive actions require appropriate confirmation.
- [ ] Copy and navigation are consistent.

## Access and data

- [ ] Authentication works across supported flows.
- [ ] Authorization is enforced on the server or data layer, not only in the UI.
- [ ] Users cannot access another user's private records.
- [ ] Database policies and migrations were reviewed.
- [ ] Secrets are not exposed in client code or repository history.

## Quality

- [ ] Mobile and desktop layouts were tested.
- [ ] Keyboard navigation and basic accessibility were checked.
- [ ] Major browsers were tested where relevant.
- [ ] Broken links and obvious console errors were removed.
- [ ] Analytics and error monitoring are configured where required.

## Operations

- [ ] Production environment variables are documented.
- [ ] Backup and recovery expectations are understood.
- [ ] Rollback or mitigation steps exist for risky changes.
- [ ] Ownership for support and maintenance is clear.
