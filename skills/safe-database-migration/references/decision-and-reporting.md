# Decision and reporting

## Decisions

- **SAFE TO PREPARE** — source/target understood, dependencies checked, data compatible or migratable, RLS/grants assessed, locking controllable, backfill defined if needed, realistic recovery / forward-fix path, preflight and post-migration checks defined, no open blockers. Does **not** authorize execution.
- **CONDITIONAL** — preparable once concrete conditions are met. Each condition must state: required action, reason, verification criterion, effect on the migration.
- **BLOCKED** — realistic data loss; unknown live state on destructive change; unresolved incompatible app paths; RLS/grants would fail; existing data violates new constraints; backfill not deterministic/resumable; unacceptable locking risk; incompatible concurrent app versions; no realistic recovery / forward-fix; unassessed `DROP ... CASCADE`; migration entangles too many risky steps with no controllable intermediate states.
- **INSUFFICIENT EVIDENCE** — no responsible evaluation possible. Use the shortened output; do not produce the full report.

## Decision consistency

- Any unresolved `Migration blocker` requires `BLOCKED`.
- Any unresolved `Required before preparation` prevents `SAFE TO PREPARE`; the decision must be at least `CONDITIONAL`.
- An `Execution condition` may coexist with `SAFE TO PREPARE` only when it is an expected run-time gate rather than missing planning work.
- `SAFE TO PREPARE` may contain known irreversible steps and post-migration cleanup, but all consequences and controls must already be understood.
- `Accepted tradeoff` requires explicit evidence that the relevant owner deliberately accepted it.

## Full report format

1. Migration summary
2. Review coverage table (Area / Status / Evidence / Limits) — status ∈ {Reviewed, Partially reviewed, Not present, Not applicable, Not verifiable}. Areas at minimum: schema, existing data, queries + code deps, constraints + FKs, views/functions/triggers, RLS + grants, realtime + storage, locking + performance, backfill, rollback + recovery, validation, live/test drift.
3. Compatibility assessment — additive / backward / forward / destructive / API / deployment-order
4. Affected dependencies — Dependency / Current usage / Migration impact / Required action
5. Existing-data assessment — aggregates only; no sensitive raw values
6. Migration strategy — Expand / Dual Compatibility / Backfill / Validate / Switch / Contract. Do not inflate the pattern if a simpler single step is provably safe.
7. Locking and operational risk — required locks, blocking, table rewrite, index strategy, monitoring, window, open uncertainty. No runtime guarantees without measurements.
8. Security impact — migration-related only
9. Backfill plan — source, target, transform, batches, idempotency, concurrent writes, progress, errors, completion
10. Rollback and recovery — transaction, code, schema, data restore, forward fix, irreversible steps
11. Preflight validation — checks with stop criteria
12. Post-migration validation — schema, data, app, security, operational
13. Blockers, conditions, and tradeoffs — grouped as: Migration blocker / Required before preparation / Execution condition / Known irreversible step / Accepted tradeoff / Post-migration cleanup. `Accepted tradeoff` requires explicit evidence of deliberate acceptance by the relevant owner; never infer acceptance from silence.
14. Residual uncertainty — invisible live config, unprofiled data, missing monitoring evidence, untested restore, unknown clients, unchecked third parties
15. Final decision — one of the four values above with a short rationale

## Shortened output on INSUFFICIENT EVIDENCE

Only:

- known migration intent
- available evidence
- decision-critical missing evidence
- checks that cannot be performed
- required read-only verifications to move forward
- open risk
- final decision: `INSUFFICIENT EVIDENCE`

Do not invent strategy, passing data checks, lock assessment, rollback capability, or a complete dependency list.

## Finding shape

Each finding: title, category (`Migration blocker` / `Required before preparation` / `Execution condition` / `Known irreversible step` / `Accepted tradeoff` / `Post-migration cleanup` / `Not applicable` / `Not verifiable`), affected objects, observed evidence, expected state, risk, impact, recommended strategy, verification step, decision effect. Do not invent risk acceptance. Merge shared root causes.

## Hard constraints (mirror of SKILL.md)

Do not execute migrations, modify files, mutate data, drop objects, run backfills, apply constraints/indexes, terminate or cancel sessions, use privileged keys, run destructive tests, or claim live-schema match / existing backups / restore capability / zero-downtime without evidence. Do not invent down migrations for irreversible steps. Do not default to `DROP ... CASCADE`. Do not require another skill. Do not conflate code revert with DB rollback. Stop after the review and wait for the user's decision.
