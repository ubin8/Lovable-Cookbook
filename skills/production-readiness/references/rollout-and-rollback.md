# Rollout and Recovery

Rollout and recovery expectations must match the project's risk profile.

Every launch needs a clear release action, a minimum post-launch verification path, and a proportionate way to revert or stabilize a harmful release. Formal launch windows, escalation paths, staged rollout, status pages, and user communications are required only where the project's scale or risk justifies them.

Missing a specific person's name is not by itself a NO-GO. When ownership is undefined, emit a `Required before launch` condition such as "Assign release ownership before launch" rather than blocking outright.

## Rollout — check (apply proportionally)
- Release ownership assigned (person or ownership area).
- Target launch window or time, where scheduling matters.
- Release process (who publishes, who verifies).
- Post-publish smoke tests: the minimum set of critical flows that must be re-verified on the live production environment.
- Feature flags or staged rollout for high-risk changes.
- Pilot group where appropriate.
- Metrics to watch immediately post-launch.
- Abort criteria — what forces a rollback.
- Communication path if things go wrong (status page, user comms, internal channel) — required only when user impact or scale justifies it.
- Incident owner and escalation — required only for projects where an incident could cause meaningful harm.
- Known risks and workarounds accepted for launch.


## Recovery — check
A rollback plan must be concrete enough to execute under time pressure. Cover:
- How to unpublish or revert to the previous build.
- Data consequences of the rollback (see `data-and-migrations.md`).
- Effect on external integrations (webhooks, OAuth callbacks, payment state) after rollback.
- Effect on users who already produced data on the new version.
- Effect on secrets/configuration changes made for the new version.

"We just redeploy the old version" is insufficient when:
- Data migrations have run.
- External webhooks have been switched.
- New secrets or configuration are required.
- Users have already created new data on the new schema.

If any of the above apply, require a concrete data/state plan or classify the rollout as high risk.

## Smoke-test plan structure (produce, do not execute)
1. Final pre-check on the version to be published.
2. Publish action (owner performs it).
3. Immediate post-publish smoke tests on production, per critical flow.
4. Monitoring window: what to watch and for how long.
5. Abort criteria and rollback trigger.
6. Recovery path if abort triggers.

Never publish, deploy, or trigger the recovery yourself.
