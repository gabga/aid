---
name: ui
description: Create or update ui-overview.md — synthesise navigation structure, page inventory, and global UX patterns from feature briefs.
---

# Init UI Overview

You are a senior UX architect with experience defining navigation structures, information architecture, and product-wide interaction patterns.

`docs/ui-overview.md` is the product-level UI reference document. It gives any reader (human or AI) a complete mental model of the product's navigation, pages, and interaction patterns before reading individual feature briefs or requirements.

## When to use

- After a set of feature briefs has been written and Scoped/Ready
- When the navigation structure or page inventory has changed significantly

## Step 1: Gather Context

Read silently:
- `docs/product.md` — product overview and feature map
- `docs/features/index.md` — all feature briefs
- All files in `docs/features/` with Status `Scoped` or `Ready`
- `docs/ui-overview.md` — existing UI overview if it exists

## Step 2: Identify Mode

Check whether `docs/ui-overview.md` exists and contains real content (not just a placeholder or empty template).

- **If it does not exist or is a placeholder**: proceed to [Create](#create).
- **If it exists with real content**: proceed to [Update](#update).

---

## Create

### Step C1: Synthesise

Extract and consolidate UI information across all feature briefs:
- Every route mentioned across all UI/UX sections
- Navigation structure implied by entry/exit points
- Interaction patterns that appear in multiple features (loading states, toasts, confirmation dialogs, empty states)
- Core user flows that span multiple features

Present a structured outline to the user:
- Proposed navigation structure
- Proposed page inventory (route, name, purpose, access)
- Global patterns identified
- Core user flows identified

Ask: "Does this look right? Anything to add or change?"

### Step C2: Write

Only after user approval. Read `${CLAUDE_SKILL_DIR}/../../assets/templates/ui_overview_template.md` as the starting structure. Create `docs/ui-overview.md` with all sections filled in from the synthesised content.

---

## Update

### Step U1: Understand the Change

Read `docs/ui-overview.md` in full.

Ask the user what has changed or what they want to change. Common triggers:
- New features have been scoped — routes and pages need adding
- Navigation structure has changed
- New global UX patterns have emerged
- Existing patterns have been revised

Identify which sections are affected and what is staying the same.

### Step U2: Propose Changes

Present a summary of what will change:
- Which sections will be modified and what is being added, changed, or removed
- The Change Log entry (date, summary, reason)

Ask: "Does this look right? Anything to adjust before I make the changes?"

### Step U3: Apply Changes

Only after user approval:
1. Edit only the affected sections.
2. Add a Change Log entry with date/time in `YYYY-MM-DD HH:MM TZ` format, a concise summary, and the reason.

---

## Final Review

Show the user what was created or changed. Ask if they want any adjustments.
