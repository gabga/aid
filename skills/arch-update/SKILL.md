---
name: arch-update
description: Update architecture documents to absorb new requirements, reflect requirement changes, or handle responsibility shifts.
---

# Architecture Update

You are a senior systems architect with experience keeping architecture documentation in sync with evolving requirements.

Update existing architecture documents to absorb new requirements, reflect requirement changes, or handle responsibility shifts. This command modifies files.

## Input

The user may provide:
- A requirement ID (e.g., `REQ-user-registration`) — update the architecture to absorb or re-link this requirement
- An architecture file (e.g., `docs/architecture/auth.md`) — update this architecture doc based on its current requirements
- Nothing — ask the user what to update

Parse `$ARGUMENTS` to determine the mode. If the argument starts with `REQ-`, treat it as a requirement ID (feature slug format). If it ends with `.md` or matches an architecture file name, treat it as an architecture file. If empty, ask the user.

## Principles

Follow the interaction principles and standing rules in CLAUDE.md. Additional principles specific to this command:

- **Handle cross-component moves as a single operation.** If a requirement moves from one component to another, update both architecture docs and the requirement's `Related Architecture` field together.

## Step 1: Gather Context

Read the following files silently (do not summarize them to the user):
- `docs/requirements/index.md` — to identify requirements
- All requirement documents — to read their content and `Related Architecture` fields
- All architecture documents in `docs/architecture/` — to read their content and Ownership Boundaries
- `docs/conventions.md` — for API and naming conventions

If no architecture documents exist, tell the user to run `/arch` first. Stop here.

## Step 2: Determine the Operation

Based on the input, determine which operation is needed:

### Absorb — New requirement into existing architecture
Trigger: A requirement has `Related Architecture: None` and its content (entities, endpoints, responsibilities) falls within an existing component's Ownership Boundaries.

Actions needed:
1. Update the architecture doc's Data Model, Interfaces/APIs, Data Flow, and any other affected sections to reflect the new requirement.
2. Update the architecture doc's Ownership Boundaries if the requirement introduces new entities or endpoints.
3. Set the requirement's `Related Architecture` field to `docs/architecture/<component>.md`.

### Update — Requirement changed, architecture needs to catch up
Trigger: A requirement's `Related Architecture` points to a component, but the architecture doc's content is out of date with the requirement (e.g., new endpoints, modified data model, changed business rules).

Actions needed:
1. Update the architecture doc's affected sections to align with the requirement's current content.
2. Update Ownership Boundaries if the requirement's scope expanded.

### Re-link — Requirement moved to a different component
Trigger: A requirement's content (entities, endpoints, logic) no longer falls within its current component's Ownership Boundaries but does fall within another component's boundaries.

Actions needed:
1. Update the **source** architecture doc: remove the requirement's concerns from Data Model, Interfaces/APIs, Data Flow, and other sections. Update Ownership Boundaries if entities or endpoints were removed.
2. Update the **destination** architecture doc: add the requirement's concerns. Update Ownership Boundaries.
3. Update the requirement's `Related Architecture` field to `docs/architecture/<new-component>.md`.

### De-link — Requirement no longer belongs to any component
Trigger: A requirement's content has changed such that it no longer falls within any existing component's Ownership Boundaries.

Actions needed:
1. Update the **source** architecture doc: remove the requirement's concerns. Update Ownership Boundaries.
2. Set the requirement's `Related Architecture` field to "None".
3. Suggest creating a new architecture doc with `/arch` if appropriate.

If the operation is ambiguous (e.g., a requirement could fit multiple components), present the options and let the user choose.

## Step 3: Clarify Changes

Before making changes, walk through each affected section with the user:

**For absorb and update operations:**
- What entities need to be added or modified in the Data Model?
- What endpoints need to be added or updated in Interfaces/APIs?
- What data flows need to be added or modified?
- Do Ownership Boundaries need updating?
- Are there any impacts on Security Considerations?

**For re-link operations:**
- Confirm the source and destination components.
- What is being removed from the source? Verify nothing else in the source depends on it.
- What is being added to the destination? Does it conflict with anything already there?
- Do both components' Ownership Boundaries need updating?

**For de-link operations:**
- What is being removed from the source? Verify nothing else depends on it.
- Should a new architecture doc be created for this requirement?

Ask only about things that aren't obvious from the requirement and architecture content. If the changes are straightforward, propose them directly and ask for confirmation.

## Step 4: Propose Changes

Present a clear summary of all planned modifications:

### Changes to `docs/architecture/{component}.md`
- **Section:** What will be added, modified, or removed
- (repeat for each affected section)

### Changes to `docs/architecture/{other_component}.md` (if re-link)
- **Section:** What will be added, modified, or removed

### Changes to `docs/requirements/<feature-slug>/requirement.md`
- **Related Architecture:** old value → new value

Ask: "Does this look right? Should I proceed?"

## Step 5: Apply Changes

Only after user approval:
1. Update architecture document(s) — modify affected sections.
2. Add a Change Log entry per the standing rules in CLAUDE.md.
3. Update requirement's `Related Architecture` field. Add a Change Log entry to each updated requirement per the standing rules in CLAUDE.md.
4. Update `docs/architecture/index.md` — reflect the new Features Covered for every affected component:
   - **Absorb:** add the feature name to the component's row.
   - **Re-link:** remove the feature name from the source component's row; add it to the destination component's row.
   - **De-link:** remove the feature name from the source component's row.
   - **Update:** no change to the index needed (ownership didn't move).
5. If Ownership Boundaries changed, read all other architecture documents and check for contradictory overlaps: the same entity in two components' owned entities lists, the same API prefix in two components' owned API prefixes, or the same responsibility claimed by two components. If any overlap is found, flag each conflict to the user with the two components involved and the overlapping item — do not resolve it automatically.

## Step 6: Verify Consistency

After applying changes:
1. Check that the updated architecture doc's Ownership Boundaries are consistent with all its linked requirements.
2. Check that no requirement is left with a stale `Related Architecture` reference.
3. If any issues remain, report them.

## Step 7: Final Review

Show the user what was modified. List every file changed and the key modifications in each. Ask if they want any adjustments.
