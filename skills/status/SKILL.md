---
name: status
description: Show current project status — features, requirements, architecture, and quality coverage across all layers.
---

# Project Status

You are a senior technical program manager with experience assessing project health across documentation and delivery workstreams.

Give the user a quick overview of the current state of the requirements repository. This is a read-only command — do not modify any files.

## Step 1: Spawn Analysis Agent

Use the Task tool to launch an **Explore** agent with the following prompt. Wait for it to complete before proceeding.

> You are analyzing a requirements and architecture documentation repository. Read the files listed below and produce a status report. Do not modify any files.
>
> **Files to read:**
> - `docs/product.md`
> - `docs/tech_stack.md`
> - `docs/conventions.md`
> - `docs/glossary.md`
> - `docs/features/index.md`
> - `docs/requirements/index.md`
> - All `.md` files in `docs/requirements/` (glob `docs/requirements/**/*.md`)
> - `docs/architecture/index.md`
> - All `.md` files in `docs/features/` (glob `docs/features/*.md`)
> - All `.md` files in `docs/architecture/` (glob `docs/architecture/*.md`)
> - `docs/ui-overview.md` (if it exists)
> - `docs/security.md` (if it exists)
>
> A file is considered a **placeholder** if its content consists only of comment blocks, empty table rows, or boilerplate header with no real content filled in.
>
> Return a status report with exactly these sections:
>
> **Product**
> - Whether `docs/product.md` is filled in or still a placeholder
> - Whether `docs/tech_stack.md` is filled in or still a placeholder
> - Whether `docs/ui-overview.md` exists and is filled in, or is missing
>
> **Feature Briefs**
> - Total feature count (rows in `docs/features/index.md`, excluding header)
> - Breakdown by status: Concept / Scoped / Ready
> - Features with open questions (Status not Ready)
> - One line per feature: name, status, priority, requirement count
>
> **Requirements**
> - Total requirement documents
> - Breakdown by status: Draft / Approved / Implemented / Updated
> - One line per feature: name, requirement files, Lowest Status, Highest Status
>
> **Architecture**
> - Total architecture documents (all `.md` files in `docs/architecture/` excluding `index.md`)
> - For each: file name and which features it covers
> - If none exist beyond `index.md`, state that clearly
>
> **Quality & Security**
> - Whether `docs/security.md` exists and is filled in, or is missing
>
> **Conventions & Glossary**
> - Whether `docs/conventions.md` has real content or is still a placeholder
> - Number of terms defined in `docs/glossary.md`
>
> **Pipeline View**
> Show where each feature sits in the workflow:
> - No brief yet
> - Brief: Concept
> - Brief: Scoped (open questions remaining)
> - Brief: Ready (requirements not yet written)
> - Requirements: Draft
> - Requirements: Approved
> - Requirements: Implemented
>
> **What's Next**
> Based on what you found, list concrete suggestions for what to work on next:
> - If `docs/product.md` is a placeholder, suggest filling it in first
> - If features have no briefs, suggest running `/feature`
> - If briefs are Scoped with open questions, suggest resolving them
> - If briefs are Ready with no requirements, suggest running `/requirement`
> - If `docs/ui-overview.md` is missing but briefs exist, suggest running `/ui`
> - If `docs/conventions.md` is a placeholder, suggest running `/conventions`
> - If requirements are Draft, suggest reviewing for approval
> - If Approved requirements have no architecture doc, suggest `/arch`
> - If architecture exists, suggest running `/tech`
> - If Approved or Implemented requirements exist but `docs/security.md` is missing or a placeholder, suggest running `/security`
> - If the project is empty, suggest the initialization sequence: `/aid:product` → `/aid:feature` → `/aid:ui` → `/aid:conventions` → `/aid:requirement` → `/aid:arch` → `/aid:tech`

## Step 2: Present Report

Present the agent's report to the user verbatim. Then ask: "Would you like to work on any of these?"
