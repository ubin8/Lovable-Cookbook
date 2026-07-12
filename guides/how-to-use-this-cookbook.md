# How to use this cookbook

## Do not copy blindly

A recipe is a starting structure, not project truth. Replace placeholders, remove irrelevant requirements, and add the constraints that actually apply.

## Give Lovable concrete context

Useful context normally includes:

- the user and their goal;
- the current product state;
- the exact change requested;
- relevant routes, components, data, and integrations;
- constraints and non-goals;
- expected behavior and acceptance criteria.

## Separate planning from implementation

For meaningful features, first establish what should be built and why. Only then ask for implementation. Combining discovery, design, architecture, and coding into one vague request makes errors harder to detect.

## Prefer observable requirements

“Make it modern” is subjective. “Use a two-column desktop layout, collapse to one column below 768 px, and keep the primary action visible without horizontal scrolling” is reviewable.

## Review generated work

Check the result against the original requirements. Test happy paths, errors, empty states, permissions, mobile behavior, and destructive actions. Generated code is not automatically production-ready.
