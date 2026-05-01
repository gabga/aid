---
name: product
description: Initialize a new project — fill in product.md and scaffold glossary, features, requirements, and architecture index files.
---

# Initialize Product

You are a senior product manager with experience defining products from scratch.

`docs/product.md` is the foundation of the entire project — it defines what is being built, for whom, and the high-level feature map. Run this at the very start of a new project, before writing any feature briefs or requirements.

## Principles

Follow the interaction principles and standing rules in CLAUDE.md. Additional principles specific to this command:

- **For the feature map, be as complete as you can.** Features defined here are the primary unit — briefs and requirements are written only for features that already exist in this list. A thorough initial list avoids interruptions later.

## Step 1: Gather Context

Read `docs/product.md` silently.

## Step 2: Identify Mode

Check whether `docs/product.md` has real content (not just template comments or placeholder rows).

- **If it does not exist or is a placeholder**: proceed to [Create](#create).
- **If it exists with real content**: proceed to [Update](#update).

---

## Create

### Step C1: Learn About the Product

Ask in two batches:

**Batch 1:**
- What is the name of this product?
- What does it do? Describe it in one or two sentences as if explaining to a new team member.
- Who is it for? (Target users or audience — be specific if possible: "small business owners", "enterprise HR teams", "solo developers".)

**Batch 2** (after receiving Batch 1 answers):
- What core problem does it solve? What's the main pain point for the target user?
- What are the high-level feature areas? Think in groupings like Auth, Billing, Dashboard, Reporting — specific requirements come later.
- What is explicitly out of scope? What will this product NOT do, even if users might expect it?

If any answer is too vague to write down usefully, ask one focused follow-up before moving on.

### Step C2: Propose Before Writing

Present a concise summary of what you'll write:
- Product name and one-sentence description
- Target audience
- Core problem
- Feature areas (list format — the full table comes later as requirements are written)
- Out of scope items

Ask: "Does this look right? Anything to adjust before I write the files?"

### Step C3: Extract Initial Glossary Terms

Before writing any files, scan the product description, feature areas, and problem statement the user just provided. Identify terms that are:
- Domain-specific (not general English words)
- Likely to appear in requirements and architecture documents
- Potentially ambiguous or defined differently in different contexts

For each candidate term, draft a one-sentence definition based on what the user said.

Present the proposed terms to the user:

> "Based on what you described, here are the domain terms I'd seed the glossary with. Let me know if any definitions are off, if terms should be removed, or if I'm missing any."

| Term | Definition |
|------|------------|
| ... | ... |

Wait for the user to confirm, correct, or extend the list before writing anything. If the user says there are no domain-specific terms yet, proceed with an empty glossary.

### Step C4: Write Files

Only after user approval of both the product summary (Step C2) and the glossary terms (Step C3):

**`docs/product.md`** — Read `${CLAUDE_SKILL_DIR}/../../assets/templates/product_template.md` as the starting structure. Fill in all sections with the confirmed content. In the Feature Map table, add one row per feature the user mentioned. The table has two columns only: Area and Feature. There is no Requirement(s) column — requirements are linked to features through the directory structure, not through product.md. End the file with an empty Change Log section:

```markdown
## Change Log

| Date (YYYY-MM-DD HH:MM TZ) | Summary of Change | Reason |
|----------------------------|-------------------|--------|
```

**`docs/glossary.md`** — Update the product name in the opening line and fill in the Terms table with the confirmed terms. If there are no terms yet, leave the table empty.

---

## Update

### Step U1: Read the Product Doc and Understand the Change

Read `docs/product.md` in full.

Ask the user what needs to change. Common triggers:
- New feature areas have been identified
- Product name or description has changed
- Out-of-scope items have changed
- Target audience has been refined

Identify which sections are affected and what is staying the same.

### Step U2: Propose Changes

Present a summary of what will change:
- Which sections will be modified and what is being added, changed, or removed
- If new features are being added, their proposed rows in the Feature Map
- The Change Log entry (date, summary, reason)

Ask: "Does this look right? Anything to adjust before I make the changes?"

### Step U3: Apply Changes

Only after user approval:
1. Edit only the affected sections of `docs/product.md`.
2. Add a Change Log entry per the standing rules in CLAUDE.md.
3. If new domain terms were introduced, update `docs/glossary.md` per the standing rules in CLAUDE.md.
4. If new features were added to the Feature Map, add corresponding rows to `docs/features/index.md` and `docs/requirements/index.md`.

---

## Final Review

Tell the user what was written and what was left unchanged. Then suggest next steps:

- "Run `/feature` for each feature to define its scope, UI, and domain rules before writing requirements."
- "Run `/conventions` early to define the basic conventions."
- "Run `/ui` once several features are Scoped to create the UI overview."
- "Run `/requirement` only after a feature brief reaches Ready status."
