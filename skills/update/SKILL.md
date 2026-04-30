---
name: update
description: Update AID files (CLAUDE.md and stubs) in an existing project to the latest plugin version.
---

# Update

You are a senior technical program manager with a software engineering background, updating an existing project's AID workflow files to the latest version.

This command updates `CLAUDE.md` in the current project to match the latest version shipped with the plugin. It does not modify any documentation the user has written — only files originally created by `/bootstrap`.

## Step 1: Check Initialisation

If `CLAUDE.md` does not exist at the repository root, stop: "This project has not been bootstrapped yet. Run `/bootstrap` first."

## Step 2: Compare CLAUDE.md

1. Read `${CLAUDE_SKILL_DIR}/../../assets/CLAUDE.md` (the latest version from the plugin).
2. Read `./CLAUDE.md` (the current project version).
3. Compare the two and identify what has changed.

If they are identical, tell the user: "Your `CLAUDE.md` is already up to date." and stop.

## Step 3: Extract Project Notes

Read `./CLAUDE.md` and extract the content of the `## Project Notes` section, if it exists and contains anything beyond the placeholder comment. Save it — it will be re-inserted after the update.

## Step 4: Present Changes

Show the user a summary of what has changed:
- New sections added
- Existing sections modified
- Sections removed

Ask: "Would you like me to update `CLAUDE.md` to the latest version?"

## Step 5: Apply Update

Only after user confirmation:
1. Write the latest content to `./CLAUDE.md`.
2. If any Project Notes content was saved in Step 3, replace the placeholder comment in the new `## Project Notes` section with that content.

Tell the user the update is complete. If Project Notes were preserved, confirm that explicitly.
