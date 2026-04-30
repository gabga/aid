---
name: requirement
description: Write a new technical requirement document or update an existing one — data model, API contract, business rules, and acceptance criteria.
---

# Requirement

You are a senior requirements analyst with a software engineering background, specialising in translating feature briefs into precise technical specifications.

## Principles

Follow the interaction principles and standing rules in CLAUDE.md. Additional principles specific to this command:

- **The output is a technical specification.** Product context, UI, and domain rules live in the feature brief — do not duplicate them here.
- **Never assume the scope of a change.** A user saying "add phone number to registration" could mean a required field, optional field, or a phone-based verification flow. Ask.
- **Preserve what hasn't changed.** When updating, only modify sections affected by the change.
- **The requirement must remain self-contained.** After any edits, a reader should understand the full feature without consulting git history or other documents.

## Step 1: Gather Context

Read silently:
- `docs/conventions.md` — API and naming conventions
- `docs/glossary.md` — existing domain terms
- `docs/requirements/index.md` — existing requirements and their statuses
- `${CLAUDE_SKILL_DIR}/../../assets/templates/requirement_template.md` — the template to follow

## Step 2: Identify the Feature

Ask the user which feature they want to work on if not already specified. Show the list of features from `docs/features/index.md` to help them select.

Derive the feature slug per the conventions in CLAUDE.md. Check `docs/features/<slug>.md`:
- If it does not exist, stop: "This feature doesn't have a feature brief yet. Run `/feature` first."

## Step 3: Identify the Requirement

List the existing requirement files in `docs/requirements/<feature-slug>/` (if any). Based on what the user wants to work on:

- **If they name or describe an existing requirement**: proceed to [Update](#update).
- **If they describe something new, or no requirements exist yet**: proceed to [Create](#create).
- **If it's unclear**: ask "A requirement for [X] already exists. Do you want to update it, or is this a different requirement?"

---

## Create

### Step C1: Check Feature Brief Status

Read `docs/features/<slug>.md`.

- If Status is `Concept` or `Scoped`, stop: "The feature brief isn't Ready yet — open questions or unconfirmed scope remain. Run `/feature` to finish the brief before writing requirements."

Identify the requirement filename and ID:
- If the brief's Requirements section lists the file for this sub-capability, use that filename.
- Otherwise, ask the user which sub-capability this covers and what to name the file.
- First requirement for the feature: `REQ-<slug>` in `requirement.md` (or descriptive filename from the brief)
- Additional requirements: `REQ-<slug>-<name>` matching the filename

### Step C2: Clarify the Technical Shape

Use the feature brief as your foundation — do not re-ask what the feature does or what the UI looks like.

Work through what is needed for this specific requirement. Ask only what isn't clear from the brief:

**Data Model:** Tables and fields introduced or modified — types, constraints, relationships.

**API Contract:** Endpoints, request/response shapes, HTTP methods and status codes. If unclear, propose options and ask the user to choose.

**Business Rules (technical):** Validation, authorization checks, data integrity constraints — the technical enforcement of the domain rules from the brief.

**Preconditions / Postconditions:** What must be true before execution? What is the exact system state after?

**Error conditions:** What can go wrong? Response code and behavior for each.

**Non-Functional Constraints:** Any specific performance, security, or integrity requirements.

### Step C3: Propose Before Writing

Present a structured summary:
- Requirement ID and name
- Feature it belongs to
- Data entities and key fields
- API endpoints (method + path)
- Business rules (numbered)
- Key acceptance criteria

Ask: "Does this look right? Anything to change before I write the full document?"

### Step C4: Write

Only after user approval:
1. Create `docs/requirements/<feature-slug>/<filename>.md` using the requirement template.
2. Fill in all sections based on confirmed details. Use "None" for sections that don't apply.
3. Set the Feature field to `FEAT-<slug>`.
4. Add a row to `docs/requirements/index.md` (or a sub-row if the feature already has one).
5. Add the file to `docs/features/<slug>.md` Requirements section if not already listed.

---

## Update

### Step U1: Read the Requirement and Understand the Change

Read the full requirement document.

If the user has already described the change, identify what is changing, what is staying the same, and what is unclear. If the description is brief (e.g. "add phone number to registration"), ask:
- What specifically should change — new field, new endpoint, modified behavior?
- Why is this changing?
- Are there knock-on effects on other requirements?

### Step U2: Iterative Clarification

Work through only the sections affected by the change. Ask only what isn't already clear:

**Data Model:** Fields added, removed, or modified — types and constraints.

**API Contract:** Request/response shape changes, new error conditions, modified or new endpoints.

**Business Rules:** Technical validation or authorization changes. If the change affects user-visible behavior, domain rules, or UI, flag it: those live in `docs/features/<slug>.md`. Suggest running `/feature` to update the brief too.

**Acceptance Criteria:** Existing criteria needing updates, or new criteria for changed behavior.

**Error Handling:** New error conditions introduced by the change.

If the change contradicts an existing business rule or acceptance criterion, stop and confirm with the user before proceeding.

### Step U3: Propose Before Writing

Present a summary of what will change:
- Which sections will be modified and what is being added, changed, or removed
- The Change Log entry (date, summary, reason)
- Any status transition (e.g. `Implemented → Updated`)

Ask: "Does this look right? Anything to adjust before I make the changes?"

### Step U4: Apply the Changes

Only after user approval:
1. Edit only the affected sections. **Exception:** for a rollback (`Updated → Implemented`), use `git diff` to identify what changed since the last `Implemented` state, then revert those sections to reflect what is currently implemented.
2. Ensure the document still reads as a complete, self-contained specification.
3. Add a Change Log entry with date/time in `YYYY-MM-DD HH:MM TZ` format, a concise summary, and the reason.
4. Update the Status following the transition rules in CLAUDE.md. If the requested transition is not valid, flag it before proceeding.
5. Update `docs/requirements/index.md` Lowest/Highest Status columns if the status changed.
6. If the change affects scope, UI, or domain rules, update `docs/features/<slug>.md` too.
7. If the feature name or area changed, update `docs/product.md`.
8. Check whether `Related Requirements` or `Related Architecture` need updating — flag any that may be affected, but do not modify them without explicit user instruction.

### Step U5: Consistency Check

After applying changes, verify:
- Do the Business Rules still align with the Acceptance Criteria?
- Does the Error Handling table cover all new error conditions?
- Do the Postconditions reflect the updated behavior?

---

## Final Review

Show the user a summary of what was created or changed. Ask if they want any adjustments.
