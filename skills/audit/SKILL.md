---
name: audit
description: Full project consistency check — cross-references, template compliance, glossary coverage, and lifecycle status across all documents.
---

# Project Audit

You are a senior technical lead with experience reviewing documentation repositories for consistency and completeness.

Audit the requirements repository for consistency and completeness. Report findings. Do NOT fix anything without asking.

## Step 1: Spawn Audit Agents

Use the Task tool to launch two **Explore** agents in parallel — one for consistency checks and one for redundancy and semantic consistency. Wait for both to complete before proceeding.

### Agent 1: Consistency Agent

> You are auditing a requirements and architecture documentation repository. Read every file listed below, run all checks, and return structured findings. Do not modify any files.
>
> **Files to read:**
> - `CLAUDE.md`
> - `docs/product.md`
> - `docs/tech_stack.md`
> - `docs/conventions.md`
> - `docs/glossary.md`
> - `docs/features/index.md`
> - `docs/requirements/index.md`
> - `docs/ui-overview.md` (if it exists)
> - `${CLAUDE_SKILL_DIR}/../../assets/templates/feature_brief_template.md`
> - `${CLAUDE_SKILL_DIR}/../../assets/templates/requirement_template.md`
> - `${CLAUDE_SKILL_DIR}/../../assets/templates/architecture_template.md`
> - All files in `docs/features/` (glob `docs/features/*.md`)
> - All files in `docs/requirements/*/` (glob `docs/requirements/**/*.md`)
> - All files in `docs/architecture/` (glob `docs/architecture/*.md`)
> - `docs/security.md` (if it exists)
>
> Run the following checks and collect findings as **errors** (will cause implementation problems), **warnings** (gaps or inconsistencies), **suggestions** (improvements), or **questions** (ambiguous — needs user input rather than a fix).
>
> ---
>
> ### Check 1: Feature Brief Coverage
> - Every feature in `docs/product.md` must have a row in `docs/features/index.md`. Missing rows are warnings.
> - Every row in `docs/features/index.md` must have a corresponding file in `docs/features/`. Missing files are errors.
> - Feature brief Status must be one of: `Concept`, `Scoped`, `Ready`. Any other value is an error.
> - A brief with Status `Ready` must have at least one file listed in its Requirements section. Missing is a warning.
>
> ### Check 2: Brief–Requirement Consistency
> - Every requirement file in `docs/requirements/*/` must be listed in the Requirements section of its corresponding feature brief. Missing entries are warnings.
> - Every file listed in a brief's Requirements section must exist in `docs/requirements/<slug>/`. Missing files are errors.
> - A requirement's `Feature` metadata field must reference an existing `docs/features/<slug>.md`. Missing or broken reference is an error.
>
> ### Check 3: Requirements Index Consistency
> - Every directory in `docs/requirements/*/` must have a row in `docs/requirements/index.md`. Missing rows are errors.
> - Every row in `docs/requirements/index.md` must point to an existing directory. Dangling rows are errors.
> - The Lowest Status and Highest Status columns must match the actual statuses in the requirement files under each directory. Mismatches are errors.
> - The Priority in the index must match the Priority in the requirement files. Mismatches are errors.
>
> ### Check 4: Feature Map Consistency
> - Every requirement directory must correspond to a feature slug in `docs/product.md`. Directories with no matching feature are errors.
> - Features in `docs/product.md` with no requirement directory are warnings (expected while specifying the product).
>
> ### Check 5: Template Compliance
> - For each feature brief, verify all sections from `${CLAUDE_SKILL_DIR}/../../assets/templates/feature_brief_template.md` are present. Missing sections are errors.
> - For each requirement document, verify all sections from `${CLAUDE_SKILL_DIR}/../../assets/templates/requirement_template.md` are present. Missing sections are errors.
> - For each architecture document, verify all sections from `${CLAUDE_SKILL_DIR}/../../assets/templates/architecture_template.md` are present. Missing sections are errors.
> - Every architecture file in `docs/architecture/` (excluding `index.md`) must have a row in `docs/architecture/index.md`. Missing rows are errors.
>
> ### Check 6: Requirement Lifecycle Consistency
> - Each requirement's Status must be one of: `Draft`, `Approved`, `Implemented`, `Updated`. Any other value is an error.
> - A requirement with status `Updated` must have at least one Change Log entry. Missing entry is an error.
> - A requirement with status `Approved` or `Implemented` must have a `Related Architecture` field that references an existing architecture document. Missing or broken link is an error.
> - The `docs/requirements/index.md` Lowest/Highest Status must correctly reflect the range of statuses in each feature directory. Mismatches are errors.
>
> ### Check 7: Cross-References
> - Every requirement's Related Requirements must reference existing feature slugs (`REQ-<feature-slug>`). Check that `docs/requirements/<feature-slug>/` exists. Missing directories are errors or questions depending on context.
> - For `Approved` or `Implemented` requirements, `Related Architecture` must reference an existing architecture file. For `Draft` and `Updated`, `None` is acceptable.
> - Every architecture document's requirement references must point to existing feature directories.
> - If a reference points to something that doesn't exist, flag as a question.
>
> ### Check 8: Glossary Coverage
> - Scan all feature briefs, requirement and architecture documents for domain-specific terms.
> - Compare against `docs/glossary.md`.
> - Terms that appear to be domain-specific but are not in the glossary are suggestions.
>
> ### Check 9: Conventions Compliance
> - If `docs/conventions.md` is a placeholder (no real content), skip this check and note it as a warning.
> - Otherwise: verify API endpoints in requirements follow the naming conventions in `docs/conventions.md`. Deviations are warnings.
> - Verify response formats match the standard envelope. Deviations are warnings.
> - Verify error codes use the standard codes. Deviations are warnings.
> - If a deviation appears intentional (explicitly noted in the requirement), flag as a question rather than a warning.
>
> ### Check 10: Architecture Coverage
> - Every requirement with status `Approved` or `Implemented` should have a linked architecture document. Missing links are errors.
> - Architecture documents that no requirement references are warnings.
>
> ### Check 11: Security Coverage
> - If there are any requirements with status `Approved` or `Implemented`, `docs/security.md` must exist at the repository root. If it is missing, flag as a warning and note that `/security` should be run.
> - If `docs/security.md` exists, verify it is non-empty and contains at least one rule section. An empty or stub file is a warning.
>
> ---
>
> **Return your findings in this exact format:**
>
> ### Audit Summary
> - Total feature briefs: X
> - Total requirements: X
> - Total architecture docs: X
> - Issues found: X (Y errors, Z warnings, W suggestions, V questions)
>
> ### Errors
> - [Description] — [File/location] — [Suggested fix]
>
> ### Warnings
> - [Description] — [File/location] — [Suggested fix]
>
> ### Suggestions
> - [Description] — [File/location] — [Suggested fix]
>
> ### Questions
> - [Question] — [Context]
>
> If a section has no items, write "None."

### Agent 2: Redundancy and Semantic Consistency Agent

> You are auditing a requirements and architecture documentation repository for redundant content and semantic inconsistencies. Read every file listed below and return structured findings. Do not modify any files.
>
> **Files to read:**
> - `docs/product.md`
> - `docs/conventions.md`
> - `docs/glossary.md`
> - `docs/ui-overview.md` (if it exists)
> - All files in `docs/features/` (glob `docs/features/*.md`)
> - All files in `docs/requirements/*/` (glob `docs/requirements/**/*.md`)
> - All files in `docs/architecture/` (glob `docs/architecture/*.md`)
>
> Run the following checks and collect findings as **warnings** (worth fixing) or **suggestions** (worth considering).
>
> ---
>
> ### Check 1: Inline Conventions
> Scan all feature briefs, requirements, and architecture documents for content that restates conventions already defined in `docs/conventions.md` — API response envelopes, error codes, field naming, pagination, date formats, ID formats, authentication headers. Any restatement is a warning: the document should reference `docs/conventions.md` rather than duplicate the rule.
>
> ### Check 2: Inline Glossary Definitions
> Scan all feature briefs, requirements, and architecture documents for inline definitions of domain terms (e.g. "a Workspace is a..."). If the term is already defined in `docs/glossary.md`, the inline definition is a warning. If the term is not in the glossary, it is a suggestion to add it.
>
> ### Check 3: Duplicate Business Rules
> Identify business rules, validation rules, or constraints that appear in more than one document. Rules that are intentionally shared (e.g. a rule that spans features) should be noted as a question — is this deliberate or a copy-paste? Rules that are clearly identical and belong in one place are warnings.
>
> ### Check 4: Duplicate Data Model Definitions
> Identify entities or fields that are defined in more than one requirement or architecture document. Flag cases where the same entity is partially defined in multiple places — this is a warning indicating the definitions should be consolidated.
>
> ### Check 5: Semantic Contradictions
> Identify statements across documents that contradict each other about the same entity, endpoint, field, or rule. Examples:
> - A field marked required in one document and optional in another
> - An endpoint described with different request or response shapes in a requirement vs. its architecture document
> - A business rule stated differently in a feature brief and the corresponding requirement
> Flag each contradiction as a warning with the specific documents and lines involved.
>
> ### Check 6: Feature Brief vs Requirement Overlap
> Identify content in feature briefs that is duplicated verbatim or near-verbatim in requirement documents. Feature briefs should describe scope and intent; requirements should describe the technical spec. Overlap is a suggestion to decide where the content belongs and remove it from the other.
>
> ---
>
> **Return your findings in this exact format:**
>
> ### Redundancy Summary
> - Documents checked: X
> - Issues found: X (Y warnings, Z suggestions, W questions)
>
> ### Warnings
> - [Description] — [File(s)/location] — [Suggested fix]
>
> ### Suggestions
> - [Description] — [File(s)/location] — [Suggested fix]
>
> ### Questions
> - [Question] — [Context]
>
> If a section has no items, write "None."

## Step 2: Present Findings

Present both agents' reports to the user, clearly separated. Then ask: "Would you like me to fix any of these? If so, which ones?"

When the user selects items to fix, apply the fixes directly using Edit and Write tools — do not spawn another agent for fixes.
