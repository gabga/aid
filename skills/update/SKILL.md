---
name: update
description: Update AID files (CLAUDE.md and stubs) in an existing project to the latest plugin version.
---

# Update

You are a senior technical program manager with a software engineering background, updating an existing project's AID workflow files to the latest version.

This command updates `CLAUDE.md` and the stub index files in the current project to match the latest versions shipped with the plugin. It does not modify any documentation the user has written — only files originally created by `/bootstrap`.

## Step 1: Check Initialisation

If `CLAUDE.md` does not exist at the repository root, stop: "This project has not been bootstrapped yet. Run `/bootstrap` first."

## Step 2: Compare CLAUDE.md

1. Read `${CLAUDE_SKILL_DIR}/../../assets/CLAUDE.md` (the latest version from the plugin).
2. Read `./CLAUDE.md` (the current project version).
3. Compare the two and identify what has changed. Note whether an update is needed — do not stop here even if identical.

## Step 3: Compare Stubs

For each stub file, read the latest plugin version and the current project version side by side. Identify **structural changes only** — column headers, introductory text, new sections. Do not count user-entered rows (table data rows, glossary terms) as changes.

| Stub asset | Project file |
|---|---|
| `${CLAUDE_SKILL_DIR}/../../assets/stubs/features-index.md` | `docs/features/index.md` |
| `${CLAUDE_SKILL_DIR}/../../assets/stubs/requirements-index.md` | `docs/requirements/index.md` |
| `${CLAUDE_SKILL_DIR}/../../assets/stubs/architecture-index.md` | `docs/architecture/index.md` |
| `${CLAUDE_SKILL_DIR}/../../assets/stubs/glossary.md` | `docs/glossary.md` |

If a stub file does not exist in the project (e.g., the project was partially bootstrapped), note it as missing — it can be created in Step 6.

## Step 4: Report

If nothing needs updating (CLAUDE.md is identical and no structural stub changes), tell the user: "Everything is already up to date." and stop.

Otherwise, present a summary of all pending changes:

**CLAUDE.md** (if changed):
- New sections added
- Existing sections modified
- Sections removed

**Stub files** (for each with structural changes or missing):
- Which file
- What changed (e.g., "new column added to features index", "introductory text updated")
- Note: user-entered rows will be preserved

Ask: "Would you like me to apply these updates?"

## Step 5: Apply CLAUDE.md Update

Only after user confirmation, and only if CLAUDE.md needs updating:
1. Extract the content of the `## Project Notes` section from `./CLAUDE.md`, if it exists and contains anything beyond the placeholder comment.
2. Write the latest content to `./CLAUDE.md`.
3. If Project Notes content was saved, replace the placeholder comment in the new `## Project Notes` section with that content.

## Step 6: Apply Stub Updates

For each stub file that has structural changes or is missing:

- **If the file is missing**: write the latest stub as-is.
- **If the file exists**: apply only the structural changes — update column headers, introductory text, or new sections. Preserve all existing data rows exactly as they are. Do not reorder or remove user-entered content.

## Step 7: Confirm

Tell the user what was updated. If Project Notes were preserved, confirm that explicitly. If any stub rows may need updating due to new columns (e.g., a new required column was added to an index table), flag that and describe what to fill in.
