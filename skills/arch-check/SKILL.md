---
name: arch-check
description: Check consistency between requirements and architecture documents — surface impacts, ownership conflicts, and best-practice violations.
---

# Architecture Impact Check

You are a senior systems architect with experience auditing requirement-architecture alignment.

Check whether requirements and architecture documents are consistent. This is a read-only command — do not modify any files.

## Step 1: Parse Arguments

Parse `$ARGUMENTS` to determine the check scope:
- If the argument starts with `REQ-`, the scope is a **single requirement** (feature slug: strip the `REQ-` prefix to get the directory name).
- If the argument ends with `.md` or matches an architecture file name, the scope is a **single architecture document**.
- If empty, the scope is a **full check** across all requirements and architecture documents.

## Step 2: Spawn Analysis Agent

Construct the agent prompt below by filling in `{SCOPE_INSTRUCTIONS}` based on the scope determined in Step 1:

- **Single requirement scope:** Replace `{SCOPE_INSTRUCTIONS}` with:
  `Focus on all requirement files in requirements/{feature-slug}/. Check them against all architecture documents. If the requirement directory does not exist, return an error stating that and stop.`

- **Single architecture scope:** Replace `{SCOPE_INSTRUCTIONS}` with:
  `Focus on the architecture document at architecture/{filename}. Check it against all requirements that reference it. If the file does not exist, return an error stating that and stop.`

- **Full check scope:** Replace `{SCOPE_INSTRUCTIONS}` with:
  `Perform a full check: analyze all requirements with status Draft, Approved, or Updated for architecture impact; analyze all architecture documents against all requirements that reference them.`

Then use the Task tool to launch an **Explore** agent with this prompt. Wait for it to complete before proceeding.

> You are checking consistency between requirements and architecture documents in a requirements repository. Read the files listed below, run the analysis, and return structured findings. Do not modify any files.
>
> **Scope:** {SCOPE_INSTRUCTIONS}
>
> **Files to read:**
> - `docs/requirements/index.md`
> - All requirement documents: glob `docs/requirements/**/*.md`
> - All architecture documents: glob `docs/architecture/*.md`
> - `docs/conventions.md`
> - `${CLAUDE_SKILL_DIR}/../../assets/architecture-best-practices.md`
>
> If no architecture documents exist (only `docs/architecture/index.md`), return: "No architecture documents exist yet. Run `/aid:arch` to create one before running arch-check."
>
> If no requirement documents exist (i.e., `docs/requirements/index.md` has no data rows and no subdirectories contain `.md` files), return: "No requirements exist yet. Run `/aid:requirement` to create some before running arch-check."
>
> ---
>
> ### Ownership Model
>
> Each requirement declares which architecture component(s) it belongs to via the `Related Architecture` field in its Metadata section. This is the single source of truth for ownership.
>
> Each architecture document declares its **Ownership Boundaries**: owned entities, owned API prefixes, owned responsibilities, and what it explicitly does not own.
>
> Build an ownership map:
> - For each requirement, read its `Related Architecture` field.
> - Group requirements by their declared architecture component.
> - Requirements with `Related Architecture: None` or missing are **unassigned**.
>
> ---
>
> ### Analysis 1: Requirement → Architecture Impact
>
> For each requirement in scope (Draft, Approved, or Updated), compare it against every existing architecture document using the Ownership Boundaries. Look for:
>
> **Data Model Impact:** Does the requirement add, modify, or remove fields on entities in a component's owned entities? Does it introduce new entities under an existing component's responsibilities?
>
> **Interface / API Impact:** Does the requirement define new endpoints under a component's owned API prefixes? Does it change the behavior, request or response shape of endpoints already documented?
>
> **Data Flow Impact:** Does the requirement introduce new steps in a documented data flow? Does it change the order or conditions of existing flow steps?
>
> **Dependency Impact:** Does the requirement add new external service dependencies to an existing component? Does it introduce inter-component communication that isn't documented?
>
> **Security Impact:** Does the requirement change authentication or authorization rules for an existing component? Does it introduce new sensitive data that an existing component would handle?
>
> ---
>
> ### Analysis 2: Architecture → Requirements Consistency
>
> For each architecture document in scope, use the ownership map to find all requirements that declare it as their `Related Architecture`. Then check:
>
> **Content Alignment:** For each requirement referencing this component, does the architecture doc's data model, interfaces, and data flows still align with the requirement's current content? Flag mismatches.
>
> **Ownership Conflicts:** Does any requirement reference multiple architecture documents in its `Related Architecture` field? Flag for verification — this may be intentional or a conflict.
>
> **Responsibility Shifts:** Does a requirement's `Related Architecture` point to this component, but the requirement's endpoints, entities, or logic fall outside this component's Ownership Boundaries (or appear in its "not owned" list)? If so, check all other architecture docs' boundaries to identify a better fit. Report as: "`REQ-{slug}` references `docs/architecture/{component}.md`, but its [endpoints/entities/logic] fall under `docs/architecture/{other_component}.md`'s ownership boundaries."
>
> **Unassigned Requirements:** List all requirements where `Related Architecture` is "None" or missing. For each, check all architecture docs' boundaries to suggest the best-fit component. If no component covers it, suggest creating a new architecture doc.
>
> **Orphaned Architecture:** List architecture documents that no requirement references. These may be outdated or indicate requirements are missing their `Related Architecture` field.
>
> ---
>
> ### Analysis 3: Best Practices & Industry Standards
>
> Apply the best practices checklist from `${CLAUDE_SKILL_DIR}/../../assets/architecture-best-practices.md` (read above).
>
> ---
>
> **Return your findings in this exact format:**
>
> ### Impact Summary
> - Requirements checked: X
> - Architecture documents checked: Y
> - Issues found: X (Y impacts, Z consistency issues, W ownership issues, V best-practice violations)
>
> ### Requirement → Architecture Impact
>
> Group by architecture document. For each affected doc:
>
> #### `docs/architecture/{component}.md`
> - **Requirement:** `REQ-{slug}` — {name}
> - **Impact type:** Data Model / Interface / Data Flow / Dependency / Security
> - **What changed:** One sentence describing the specific impact
> - **Sections to update:** Which sections of the architecture doc need revision
>
> If a requirement has no impact on any existing architecture doc, list it under "No Impact" with a brief reason.
>
> ### Architecture → Requirements Consistency
>
> Group by architecture document:
>
> #### `docs/architecture/{component}.md`
> - Stale content (if any)
> - Responsibility shifts (if any)
>
> #### Unassigned Requirements
> List requirements with no `Related Architecture`, with suggested components.
>
> #### Orphaned Architecture
> List architecture documents with no requirement references.
>
> #### Ownership Conflicts
> List requirements referencing multiple architecture documents.
>
> ### Best Practices & Standards Violations
>
> Group by architecture document. For each document, list only actual violations — skip compliant items. If a document is fully compliant, write "No violations found."
>
> #### `docs/architecture/{component}.md`
> - **Category:** REST API / Token Storage / Internal API Isolation / Cross-service References / Async Reliability / Multi-tenancy / Data Integrity
> - **Violation:** One sentence describing the specific issue
> - **Standard:** What the industry standard or best practice requires
> - **Fix:** Concrete change that would bring this into compliance
>
> ### Suggested Next Steps
> Based on the findings, list what the user should do:
> - Architecture docs needing updates → offer to help with `/arch-update`
> - Responsibility shifts → suggest updating `Related Architecture` fields in requirements
> - Unassigned requirements → suggest setting `Related Architecture` or running `/arch`
> - Orphaned architecture docs → suggest verifying whether requirements are missing references or the doc is obsolete
> - Best-practice violations → list which documents need revision and what category of fix is needed
> - If no issues found → confirm that architecture and requirements are consistent and meet best practices

## Step 3: Present Findings

Present the agent's report to the user. Then ask: "Would you like me to help with any of these?"
