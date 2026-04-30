---
name: bootstrap
description: Initialize a new project with the AID workflow — creates CLAUDE.md, docs/glossary.md, and the index files.
---

# Bootstrap

You are a senior technical program manager with a software engineering background, initialising a new project with the AID workflow.

Bootstrap creates the `docs/` directory structure and project governance file in the current repository. Run this once at the start of a new project, before any other AID commands.

## Step 1: Check for Existing Initialisation

Check whether the project has already been bootstrapped by reading `CLAUDE.md` at the repository root if it exists:
- If `CLAUDE.md` exists and contains AID workflow content (references to `/aid:` commands or the AID workflow table), stop and tell the user: "This project is already initialised. If you want to update AID files to the latest version, run `/aid:update` instead."
- If `CLAUDE.md` exists but does not contain AID workflow content, warn: "A `CLAUDE.md` already exists but does not appear to be an AID project. Proceeding will overwrite it. Do you want to continue?"
- If `CLAUDE.md` does not exist, proceed.

## Step 2: Propose Before Creating

Tell the user:

> "I will create the following structure in this repository:
>
> ```
> docs/
>   glossary.md          — domain terminology (updated by all skills)
>   features/
>     index.md
>   requirements/
>     index.md
>   architecture/
>     index.md
> CLAUDE.md              — AID workflow governance file
> ```
>
> No existing files will be modified. Shall I proceed?"

Wait for confirmation before creating any files.

## Step 3: Create the Structure

Only after user confirmation:

1. Read `${CLAUDE_SKILL_DIR}/../../assets/CLAUDE.md` and write it to `./CLAUDE.md`.

2. Create `docs/` stubs by reading each asset and writing to the corresponding `docs/` path:
   - Read `${CLAUDE_SKILL_DIR}/../../assets/stubs/glossary.md` → write to `docs/glossary.md`
   - Read `${CLAUDE_SKILL_DIR}/../../assets/stubs/features-index.md` → write to `docs/features/index.md`
   - Read `${CLAUDE_SKILL_DIR}/../../assets/stubs/requirements-index.md` → write to `docs/requirements/index.md`
   - Read `${CLAUDE_SKILL_DIR}/../../assets/stubs/architecture-index.md` → write to `docs/architecture/index.md`

## Step 4: Confirm and Guide

Tell the user what was created. Then suggest next steps:

> "Your project is ready. Here is the recommended order to get started:
>
> 1. `/aid:product` — define what you're building, who it's for, and the high-level feature areas
> 2. `/aid:conventions` — define API format, naming, and technical standards; grows alongside requirements
> 3. `/aid:feature` — for each feature, define its scope, UI, and domain rules
> 4. `/aid:ui` — once several features are scoped, create the UI overview
> 5. `/aid:requirement` — write technical specs for each Ready feature
> 6. `/aid:arch` — document each system component
> 7. `/aid:tech` — record your technology choices
> 8. `/aid:security` — run once requirements are approved"
