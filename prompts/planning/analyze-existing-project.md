# Analyze Existing Project

## Recipe

- Category: Planning
- Recommended mode: Plan Mode
- Risk level: Low
- Estimated scope: Medium

## Purpose

Build an evidence-based understanding of an existing Lovable project before planning or changing it. The result should identify the product, architecture, data flow, conventions, risks, and unknowns without modifying the project.

## Use When

- joining or revisiting an existing project
- preparing a feature, refactor, migration, integration, or security review
- Project Knowledge may be incomplete or outdated
- the relevant implementation area is unclear
- previous changes caused regressions or architectural inconsistency

## Do Not Use When

- the task is already small, isolated, and fully understood
- implementation has already been approved and only execution remains
- the user asked for a targeted debugging or security recipe instead

## Required Context

Provide when known:

- `[ANALYSIS_SCOPE]`: Whole project or a specific area
- `[PROJECT_GOAL]`: What the product is intended to achieve
- `[FOCUS_AREAS]`: Features, routes, flows, integrations, or concerns to prioritize
- `[KNOWN_CONCERNS]`: Suspected risks, inconsistencies, or failures
- `[EXCLUDED_AREAS]`: Areas that must not be inspected or discussed
- `[OUTPUT_DEPTH]`: Concise, standard, or deep

## Prompt

```text
You are analyzing an existing Lovable project.

Your task is to understand and document the project as it currently exists. Do not modify code, configuration, database state, dependencies, files, or external systems.

<analysis_context>
Scope: [ANALYSIS_SCOPE]
Project goal: [PROJECT_GOAL]
Focus areas: [FOCUS_AREAS]
Known concerns: [KNOWN_CONCERNS]
Excluded areas: [EXCLUDED_AREAS]
Output depth: [OUTPUT_DEPTH]
</analysis_context>

<objective>
Produce an evidence-based project analysis that another developer or later implementation step can rely on. Explain what exists, how the important parts connect, which conventions are established, where the main risks are, and what remains unknown.
</objective>

<instructions>
1. Read Workspace Knowledge and Project Knowledge first.
2. Inspect the repository structure before drawing conclusions.
3. Review only the files and systems relevant to the requested scope, expanding the scope only when a dependency requires it.
4. Trace important flows end to end rather than reviewing files in isolation.
5. Distinguish confirmed facts, evidence-supported inferences, and unresolved unknowns.
6. Cite concrete evidence using file paths, routes, configuration names, database objects, functions, components, or integrations.
7. Do not assume a library, backend, authentication model, tenant model, design system, or deployment setup merely because it is common in Lovable projects.
8. Do not propose broad rewrites or replacements unless the evidence shows a concrete problem.
9. Do not claim that behavior, security, migrations, deployment, or tests were verified unless you actually verified them.
10. Stop after the analysis. Do not implement fixes or create files.
</instructions>

<inspection_order>
Follow this order unless the project structure clearly requires another sequence:

1. Project knowledge and product intent
2. Repository structure and package configuration
3. Application entry points, routing, rendering, and layout
4. Shared components, styling, and design-system usage
5. Feature modules and critical user flows
6. State management, data fetching, caching, and validation
7. Backend, database schema, migrations, RLS, functions, storage, and authentication
8. External integrations, webhooks, jobs, and secrets boundaries
9. Tests, scripts, build checks, deployment configuration, and observability
</inspection_order>

<analysis_requirements>
For each relevant area:

- describe what exists
- explain its responsibility
- show how it connects to adjacent parts
- identify the established convention
- note duplication, inconsistency, hidden coupling, or unclear ownership
- identify security, data-integrity, compatibility, SSR, performance, or operational implications
- state what evidence supports the conclusion

When analyzing a critical flow, trace:

user action
→ route or component
→ state and validation
→ data-access layer
→ trusted backend or database boundary
→ resulting state
→ loading, empty, success, and failure behavior

When analyzing authorization, separate:

- authentication
- role or membership resolution
- resource or tenant relationship
- backend or database enforcement
- frontend visibility

When analyzing data changes, inspect relevant schema objects, migrations, constraints, indexes, RLS policies, functions, triggers, generated types, and dependent code.
</analysis_requirements>

<boundaries>
- Analysis only. Make no edits.
- Do not install packages or change configuration.
- Do not run destructive commands, migrations, writes, or external side effects.
- Read-only inspection and non-destructive checks are allowed when available.
- Do not inspect excluded areas.
- Avoid unrelated cleanup suggestions.
- Ask questions only when missing information prevents a reliable conclusion. Otherwise record the uncertainty and continue.
</boundaries>

<output_format>
Return the analysis in this structure:

# Project Analysis

## 1. Executive Summary
- Product purpose
- Current architecture
- Most important findings
- Overall confidence

## 2. Scope and Evidence
- Areas inspected
- Important files and systems reviewed
- Areas not inspected
- Limitations

## 3. Product and User Flows
For each critical flow:
- Actor and goal
- Entry point
- Main steps
- Data and integrations involved
- Authorization boundary
- Loading, empty, success, and error behavior
- Evidence

## 4. Architecture
- Frontend stack and rendering model
- Routing and layouts
- Feature/module boundaries
- Shared components and design system
- State, data fetching, validation, and caching
- Backend and integration boundaries
- Established conventions

## 5. Data, Auth, and Security
- Core domain objects and relationships
- Authentication and session model
- Roles, memberships, ownership, and tenant boundaries
- Database enforcement and RLS
- Storage and secret handling
- Privileged operations
- Confirmed controls and unverified assumptions

## 6. Quality and Operations
- Build, type, lint, and test setup
- Deployment and environment configuration
- Logging, error handling, and observability
- Reliability and performance considerations

## 7. Findings
Classify each finding as:
- Confirmed issue
- Risk
- Inconsistency
- Missing information
- Improvement opportunity

For every finding include:
- Evidence
- Impact
- Confidence: high, medium, or low
- Suggested next step

## 8. Existing Strengths
List conventions and implementations that should be preserved.

## 9. Open Questions
Include only questions that materially affect future decisions.

## 10. Recommended Next Step
Recommend the smallest safe next action. Do not implement it.
</output_format>

<quality_bar>
- Be specific, structured, and concise.
- Prefer evidence over speculation.
- Do not repeat the same finding in multiple sections.
- Do not provide hidden reasoning or a stream-of-consciousness account.
- Write the final analysis for someone who has not watched the inspection process.
- Open with the outcome, then provide supporting detail.
</quality_bar>
```

## Expected Output

A read-only project assessment that:

1. maps the relevant architecture and user flows
2. identifies established conventions worth preserving
3. separates confirmed facts from assumptions
4. surfaces risks with concrete evidence
5. defines the smallest safe next step
6. makes no project changes

## Verification Checklist

- [ ] Workspace Knowledge and Project Knowledge were reviewed.
- [ ] The repository structure was inspected before conclusions were made.
- [ ] Critical flows were traced end to end.
- [ ] Claims reference concrete project evidence.
- [ ] Authentication and authorization were analyzed separately.
- [ ] Confirmed facts, inferences, and unknowns are clearly distinguished.
- [ ] No files, configuration, data, dependencies, or external systems were changed.
- [ ] Unverified behavior is not presented as verified.
- [ ] The final recommendation is scoped and non-destructive.

## Related Resources

- Use before [`/feature-planner`](../../skills/feature-planner/README.md) when the project area or existing architecture is not yet understood.
- Use before [`/safe-database-migration`](../../skills/safe-database-migration/README.md) when a planned schema change depends on unclear existing data, migrations, constraints, or dependencies.
- Use before [`/rls-security-review`](../../skills/rls-security-review/README.md) or [`/multi-tenant-isolation-review`](../../skills/multi-tenant-isolation-review/README.md) to map relevant auth, data, storage, and tenant boundaries without replacing those focused reviews.
- Use before [`/production-readiness`](../../skills/production-readiness/README.md) when release scope, critical flows, or operational architecture are not sufficiently documented.
