# Add Responsive Behavior

## Recipe

- Category: Implementation
- Recommended mode: Agent Mode
- Risk level: Low to Medium
- Estimated scope: Small to Medium

## Purpose

Add robust responsive behavior to an existing Lovable interface without redesigning the product or weakening the desktop experience.

This recipe focuses on layout adaptation, content priority, interaction changes, overflow handling, touch usability, and breakpoint-specific behavior. It is not a generic “make it mobile-friendly” prompt. The goal is to make the same feature usable across relevant viewport sizes while preserving its hierarchy, meaning, and core actions.

## Use When

- a page works on desktop but breaks on smaller screens
- tables, forms, dashboards, sidebars, filters, modals, or navigation need responsive behavior
- content overflows or becomes unreadable
- mobile interactions need different placement or controls
- dense desktop layouts need prioritization on narrow screens
- responsive behavior is inconsistent across related components
- a feature needs verification across mobile, tablet, and desktop widths

## Do Not Use When

- the interface needs a full visual redesign
- the design system or layout strategy is still undefined
- the task is primarily about accessibility rather than responsive layout
- only one isolated CSS defect needs a tiny fix
- the feature has no meaningful smaller-screen use case
- the requested change alters product scope or information architecture

## Inputs

- `[TARGET]`: Route, page, component, or flow
- `[PRIMARY_TASK]`: Main user task the layout must support
- `[CURRENT_ISSUES]`: Known responsive failures
- `[SUPPORTED_VIEWPORTS]`: Mobile, tablet, desktop, wide desktop, or exact ranges
- `[CONTENT_PRIORITY]`: What must remain most visible
- `[CRITICAL_ACTIONS]`: Actions that must remain easy to reach
- `[EXISTING_PATTERNS]`: Current responsive components or conventions
- `[DESIGN_CONSTRAINTS]`: Required visual language, spacing, density, or component rules
- `[PRESERVED_BEHAVIOR]`: Desktop or existing behavior that must not regress
- `[OUT_OF_SCOPE]`: Explicit exclusions
- `[VERIFICATION]`: Required viewport and interaction checks

## Prompt

```text
Add responsive behavior to the following existing Lovable interface.

<target>
Target: [TARGET]
Primary user task: [PRIMARY_TASK]
Current responsive issues: [CURRENT_ISSUES]
Supported viewports: [SUPPORTED_VIEWPORTS]
Content priority: [CONTENT_PRIORITY]
Critical actions: [CRITICAL_ACTIONS]
Existing responsive patterns: [EXISTING_PATTERNS]
Design constraints: [DESIGN_CONSTRAINTS]
Behavior that must remain unchanged: [PRESERVED_BEHAVIOR]
Out of scope: [OUT_OF_SCOPE]
Required verification: [VERIFICATION]
</target>

Do not redesign the feature, change its information architecture, replace the design system, or alter business behavior.

Your task is to make the existing interface work deliberately across the supported viewport ranges.

## Responsive Objective

The result must preserve:

- the primary user task
- the visual hierarchy
- the meaning and order of information
- access to critical actions
- readable content
- usable interaction targets
- valid loading, empty, success, and error states
- existing desktop behavior unless explicitly changed

Responsive behavior should be intentional. Do not rely on accidental wrapping or hidden overflow.

## Step 1: Inspect the Current Layout

Before editing:

1. Read Workspace Knowledge and Project Knowledge.
2. Inspect the target route and all layout containers that influence it.
3. Identify the existing breakpoint system and design tokens.
4. Inspect shared responsive components already used in the project.
5. Identify fixed widths, min-widths, absolute positioning, grids, flex layouts, sticky elements, portals, and overflow containers.
6. Inspect responsive behavior of loading, empty, error, and populated states.
7. Check whether navigation, filters, tables, forms, dialogs, and action bars already have mobile variants.
8. Identify whether the layout depends on viewport width, container width, or content width.

Do not add arbitrary breakpoints before understanding the existing system.

## Step 2: Build a Responsive Behavior Map

Define the intended behavior by region, not by individual CSS rule.

For each major region, document:

- desktop structure
- tablet structure
- mobile structure
- what stays visible
- what moves
- what stacks
- what collapses
- what becomes scrollable
- what becomes a drawer, sheet, menu, or disclosure
- what may be truncated
- what must never be hidden

Major regions may include:

- primary navigation
- secondary navigation
- header
- sidebar
- filter controls
- page actions
- content grid
- table or list
- detail panel
- form
- footer
- modal or sheet

Use content priority to decide adaptation. Do not hide important information merely to avoid layout work.

## Step 3: Apply Layout Rules

### Containers

- use the project’s existing container widths and spacing scale
- avoid hard-coded viewport assumptions when container behavior is more appropriate
- prevent horizontal page overflow
- preserve intentional inner scrolling where required
- avoid nested scroll regions unless the existing interaction requires them

### Grid and Flex Layouts

- define deliberate column reduction
- prevent content from shrinking below usable width
- allow regions to stack when hierarchy is preserved
- avoid fragile combinations of fixed width and flexible width
- ensure long labels and real content do not break the layout
- use consistent gaps rather than one-off margins

### Content Priority

Classify content as:

- critical: must remain immediately visible
- important: may move but should remain easy to access
- secondary: may collapse behind disclosure
- optional: may be omitted only when product behavior allows it

Do not hide critical actions or required context on mobile.

### Ordering

- preserve logical reading order
- ensure visual reordering does not create confusing keyboard or screen-reader order
- keep labels close to their values
- keep actions near the content they affect
- avoid moving destructive actions into a more prominent mobile position

## Step 4: Adapt Common UI Patterns

### Navigation

- preserve access to all required destinations
- use the project’s existing mobile navigation pattern
- do not create a second navigation system without need
- keep the current route and active state visible
- ensure menus close predictably after navigation
- prevent body scroll when an overlay navigation is open

### Sidebars and Filters

Choose based on user task:

- persistent sidebar on wide screens
- collapsible region on medium screens
- drawer or sheet on narrow screens
- inline controls when the filter set is small

Requirements:

- preserve active filter visibility
- show when filters are applied
- keep reset behavior available
- do not lose unsaved filter state when opening or closing
- return focus appropriately after closing

### Tables

Do not make every table horizontally scrollable by default.

Choose the safest pattern:

- horizontal scroll for comparison-heavy data
- priority columns with secondary details in expansion
- card/list transformation for record-centric data
- stacked key-value layout for narrow detail views

Preserve:

- row identity
- primary value
- status
- critical action
- sorting or selection semantics where required

Do not remove columns that are required for decision-making.

### Forms

- stack fields when side-by-side layout becomes cramped
- preserve field order and grouping
- keep labels visible
- keep validation adjacent to the field
- avoid fixed-width inputs
- ensure help text wraps cleanly
- keep primary and secondary actions distinct
- preserve entered values across layout changes
- account for mobile keyboards and viewport resizing

### Action Bars

- keep the primary action reachable
- avoid overflowing action rows
- collapse secondary actions into an existing menu pattern when necessary
- preserve destructive-action separation
- do not use a sticky action bar if it obscures content or existing navigation
- include safe-area spacing where required

### Modals, Dialogs, Drawers, and Sheets

- use the project’s existing overlay pattern
- ensure content fits the viewport
- allow internal scrolling without trapping content
- keep header, body, and actions usable
- prevent actions from appearing off-screen
- preserve focus trapping and close behavior
- switch to a full-height sheet only when appropriate to the existing design

### Dashboards and Cards

- reduce columns intentionally
- preserve metric meaning and comparison context
- avoid cards with inconsistent heights caused by uncontrolled text
- do not shrink charts until labels become unreadable
- provide alternate layout or horizontal exploration when necessary

## Step 5: Handle Real Content

Test with content that stresses the layout:

- long names
- long email addresses
- translated or expanded labels
- large numbers
- multi-line status text
- missing optional data
- many actions
- long error messages
- dense table rows
- empty states with calls to action

Use appropriate wrapping, truncation, line clamping, or scrolling.

Rules:

- do not truncate information required to distinguish records
- provide access to full content when truncation is used
- do not allow unbroken strings to force page overflow
- do not design only around placeholder content

## Step 6: Preserve Interaction Quality

Responsive behavior includes interaction, not only layout.

Verify:

- touch targets are large enough for practical use
- hover-only actions have a touch-accessible equivalent
- focus remains visible
- menus and overlays are reachable by keyboard
- sticky elements do not cover focused controls
- orientation changes do not lose state
- resizing does not reset forms, filters, tabs, pagination, or selections
- drag interactions have an alternative when required
- tooltips are not the only source of essential information

Do not create separate mobile business logic unless the product explicitly requires it.

## Step 7: Check State Variants

Every responsive region must work in all relevant states:

- loading
- empty
- filtered empty
- error
- partial failure
- populated
- long content
- permission-restricted
- disabled
- selected or expanded

Do not verify only the ideal populated state.

## Step 8: Prevent Common Responsive Regressions

Check specifically for:

- horizontal page overflow
- clipped menus or popovers
- unreachable actions
- overlapping fixed and sticky regions
- broken z-index behavior
- dialogs taller than the viewport
- tables that hide record identity
- controls that wrap into ambiguous groups
- content hidden behind bottom navigation
- focus moving off-screen
- unreadable text or chart labels
- breakpoint flicker
- duplicate mobile and desktop content rendered simultaneously
- layout shift caused by late-loading content

Remove CSS overrides that fight the design system when a shared pattern already exists.

## Breakpoint Rules

- use existing project breakpoints
- add a new breakpoint only when a real layout transition requires it
- define behavior by content need, not by a specific device model
- avoid excessive breakpoint-specific exceptions
- keep adjacent viewport ranges behaviorally coherent
- prefer component-level responsiveness when the component is reused in different containers

Do not create arbitrary values solely to make one screenshot pass.

## Verification Matrix

Verify at representative widths from the project’s supported ranges.

For each width, check:

- primary task completion
- navigation
- content hierarchy
- action reachability
- overflow
- forms
- overlays
- loading, empty, and error states
- keyboard and touch behavior
- existing desktop behavior

Use this result format internally:

| Viewport | State | Primary Task | Overflow | Actions | Result |
|---|---|---|---|---|---|

When possible, verify at:
- narrow mobile
- standard mobile
- tablet
- standard desktop
- wide desktop

Also test near breakpoint boundaries, not only at exact preset widths.

## Stop Conditions

Stop and report instead of forcing local CSS fixes when:

- the layout requires changing information architecture
- the design system lacks a required responsive primitive
- shared components have conflicting responsive contracts
- the target depends on a broader shell or navigation redesign
- critical data cannot fit without a product decision
- mobile behavior would require removing required functionality
- the task would duplicate entire desktop and mobile implementations
- responsive issues originate from an unstable underlying component

In that case, explain the smallest prerequisite decision or refactor required.

## Completion Conditions

The task is complete only when:

- the primary task works across supported viewports
- critical content and actions remain available
- no unintended page-level overflow remains
- layout transitions are deliberate
- existing desktop behavior is preserved
- real content does not break the interface
- state variants are responsive
- touch and keyboard interaction remain usable
- verification covers representative widths and breakpoint boundaries
- unverified cases are reported honestly

## Final Response Format

# Added Responsive Behavior: [TARGET]

## Responsive Changes
- [REGION]: [BEHAVIOR BY VIEWPORT]
- [REGION]: [BEHAVIOR BY VIEWPORT]

## Preserved Behavior
- [DESKTOP OR EXISTING CONTRACT]

## Files Changed
- `[PATH]`: [PURPOSE]

## Verification
- Narrow mobile: [PASS / FAIL / NOT RUN]
- Standard mobile: [PASS / FAIL / NOT RUN]
- Tablet: [PASS / FAIL / NOT RUN]
- Desktop: [PASS / FAIL / NOT RUN]
- Wide desktop: [PASS / FAIL / NOT RUN]
- Breakpoint boundaries: [PASS / FAIL / NOT RUN]

## Remaining Limitations
- [ITEM]
- Write “None identified” only when justified.

Do not repeat the full responsive behavior map in the final response.
```

## Expected Output

A focused responsive implementation that:

1. preserves the primary user task
2. adapts layout according to content priority
3. keeps critical actions accessible
4. avoids unintended overflow
5. uses existing breakpoints and components
6. handles real content and state variants
7. preserves desktop behavior
8. verifies representative widths and breakpoint boundaries

## Quality Checklist

- [ ] The existing breakpoint system was inspected.
- [ ] Layout behavior was defined by region.
- [ ] Critical content remains visible.
- [ ] Critical actions remain reachable.
- [ ] Horizontal overflow is intentional only where justified.
- [ ] Tables, forms, filters, overlays, and navigation use appropriate responsive patterns.
- [ ] Real long-form content was considered.
- [ ] Loading, empty, and error states were checked.
- [ ] Touch and keyboard interaction remain usable.
- [ ] Desktop behavior did not regress.
- [ ] Breakpoint boundaries were verified.
- [ ] No duplicate mobile architecture was introduced.
