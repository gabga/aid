# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Repository Purpose

This is the source repository for the **AID — AI Aided Development** plugin. It contains the workflow skills, assets bootstrapped into user projects, and plugin metadata. It does not contain product documentation — that lives in projects that use the plugin.

## Repository Structure

```
skills/                  Workflow skills — one directory per skill
  <skill-name>/
    SKILL.md             Skill instructions and frontmatter (name, description)
assets/
  CLAUDE.md              Governance file bootstrapped into user projects
  architecture-best-practices.md  Reference used by /arch and /arch-check
  templates/             Document templates used by skills when creating documents
    feature_brief_template.md
    requirement_template.md
    architecture_template.md
    product_template.md
    tech_stack_template.md
    conventions_template.md
    ui_overview_template.md
    security_template.md
  stubs/                 Stubs bootstrapped into user projects by /bootstrap
    glossary.md
    features-index.md
    requirements-index.md
    architecture-index.md
.claude-plugin/
  plugin.json            Plugin manifest (name, version, description)
README.md
CLAUDE.md                This file
```

## Working in This Repository

**To modify a skill:** Edit the relevant `SKILL.md` in `skills/<skill-name>/`. Each skill has a frontmatter block at the top followed by the skill instructions:

```markdown
---
name: skill-name
description: One-line description shown in skill listings.
---
```

**To add a new skill:** Create a new directory in `skills/` with a `SKILL.md` file. Add a row to the workflow table in `assets/CLAUDE.md` if the skill is part of the standard workflow.

**To modify what gets bootstrapped into user projects:** Edit files in `assets/`. `assets/CLAUDE.md` is the governance file copied into user projects by `/bootstrap`. `assets/stubs/` contains the files copied verbatim by `/bootstrap` (glossary and index files). `assets/templates/` contains the document templates that skills read when creating documents for the first time.

**To update the plugin version:** Edit `.claude-plugin/plugin.json`.

## Git Conventions

- Do not add `Co-Authored-By:` trailers to commit messages.
