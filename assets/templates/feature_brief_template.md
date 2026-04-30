# FEAT-<slug>: <Feature Name>

## Metadata
- **Status:** Concept | Scoped | Ready
- **Priority:** Must | Should | Could
- **Area:** <area from product.md>

## Summary
One paragraph: what this feature does, who uses it, and why it matters.

## User Problem
What problem does this solve? What is the user trying to accomplish?

## Scope
**In:** ...
**Out:** ...

## User Stories
- As a <role>, I want <action> so that <benefit>.

## UI / UX
Describe each screen or view this feature introduces or modifies. Reference `ui-overview.md` for global layout and navigation context; describe only what is specific to this feature.

### <Screen Name>
- **Route:** `/path`
- **Entry points:** how the user reaches this screen
- **Layout:** key UI elements — fields, buttons, lists, indicators
- **Interaction states:**

  | State | Behavior |
  |-------|----------|
  | Loading | ... |
  | Loaded | ... |
  | Success | ... |
  | Error | ... |

- **Navigation:** where the user goes on success or exit

## Domain Rules
The behavioral rules that govern this feature — what users can and cannot do, visible to the user. Not technical implementation details.

- DR-1: <rule>
- DR-2: <rule>

## Requirements
Requirement documents that implement this feature. Populated once the feature is Scoped.

- `<filename>.md` — one-liner description

## Dependencies
- FEAT-<slug> — why it must exist first

## Open Questions
Unresolved design decisions that must be answered before requirements can be written.

## Notes
Non-technical context, assumptions, or decisions made during feature definition.

## Change Log

| Date (YYYY-MM-DD HH:MM TZ) | Summary of Change | Reason |
|---------------------------|-------------------|--------|
