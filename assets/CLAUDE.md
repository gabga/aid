# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Repository Purpose

This repository uses the **AID — AI Aided Development** for software product development. The `docs/` directory is the single source of truth for what the product is, how it behaves, and how it should be built. Source code lives alongside `docs/` in standard locations (e.g., `src/`).

## Repository Structure

```
docs/
  product.md             Product overview, target audience, feature map
  tech_stack.md          Technology choices
  conventions.md         Project-wide technical conventions
  glossary.md            Shared domain terminology
  ui-overview.md         Navigation, pages, global UX patterns
  security.md            Security rules and threat model
  features/
    index.md
    <feature-slug>.md    Feature brief: scope, UI, domain rules
  requirements/
    index.md
    <feature-slug>/
      <name>.md          Requirement document
  architecture/
    index.md
    <component>.md
CLAUDE.md                This file
```

## Document Layers

| Layer | Documents | Purpose |
|-------|-----------|---------|
| Product | `docs/product.md` | What is being built, for whom, and why |
| UI | `docs/ui-overview.md` | Navigation, page inventory, global UX patterns |
| Features | `docs/features/<slug>.md` | What each feature does, its UI, domain rules |
| Requirements | `docs/requirements/<slug>/<name>.md` | Data model, API, business rules, acceptance criteria |
| Architecture | `docs/architecture/<component>.md` | How components are structured and interact |
| Technology | `docs/tech_stack.md` | Documented technology choices |
| Security | `docs/security.md` | Security considerations |

The Product, UI, Feature, and Requirement layers are architecture and tech-stack agnostic — they describe *what* the system does, not *how* it is built. One feature can have multiple requirement documents, each covering a coherent sub-capability with its own data, API surface, and UI.

## AID Workflow

All skills are plugin skills invoked with the `aid:` prefix.

| Phase | Command | What it does |
|-------|---------|-------------|
| Setup | `/aid:bootstrap` | Create `docs/` structure, stubs, and `CLAUDE.md` in a new project |
| Setup | `/aid:update` | Update `CLAUDE.md` and stubs to the latest plugin version |
| 1 — Foundation | `/aid:product` | Create `docs/product.md` and seed the glossary |
| 1 — Foundation | `/aid:conventions` | Create `docs/conventions.md`; grows alongside requirements |
| 2 — Feature Design | `/aid:feature` | Create or update a feature brief |
| 2 — Feature Design | `/aid:ui` | Create or update `docs/ui-overview.md` |
| 3 — Specification | `/aid:requirement` | Write or update a technical requirement document |
| 4 — Architecture | `/aid:arch` | Create a new architecture document |
| 4 — Architecture | `/aid:arch-check` | Validate requirements and architecture alignment |
| 4 — Architecture | `/aid:arch-update` | Sync architecture after requirement changes |
| 5 — Tech Stack | `/aid:tech` | Define or update `docs/tech_stack.md` |
| Anytime | `/aid:audit` | Full project consistency check |
| Anytime | `/aid:status` | Snapshot of project state |
| Anytime | `/aid:security` | Analyse requirements, architecture, and tech stack for security issues; updates `docs/security.md` |

`docs/conventions.md` is created early via `/aid:conventions` and updated continuously as requirements are written and new patterns emerge.

## Interaction Principles

These apply to all AID commands.

- **Ask in small batches.** Present 2–4 questions at a time. Never present a wall of questions.
- **Propose before writing.** Always summarise what you plan to create or change and get user confirmation before modifying any file.
- **Never assume.** If something is unclear, ask. Do not resolve ambiguity by guessing.
- **Apply trivial conventions without asking.** Field name casing, timestamp format, pagination shape, and similar details are covered by `docs/conventions.md` — apply them and move on. Only ask about things that affect behavior or design.
- **Accept "not sure yet."** Leave a clear placeholder rather than inventing an answer.

## Standing Rules

These apply whenever any AID command creates or modifies documents.

- **Glossary:** Update `docs/glossary.md` whenever a new domain term is introduced or an existing term changes meaning.
- **Change Log:** Add an entry to the document's Change Log whenever an existing document is modified. Include date (`YYYY-MM-DD HH:MM TZ`), summary, and reason. Exceptions: index files (`index.md`) and `docs/security.md` do not have Change Logs.
- **Index files:** Update `docs/features/index.md`, `docs/requirements/index.md`, or `docs/architecture/index.md` whenever a document is created or its status changes.

## Feature Brief Lifecycle

**Feature slug derivation:** lowercase the feature name, replace spaces with hyphens, strip special characters. Example: "User Sign-Up" → `user-sign-up`.

| Status | Meaning |
|--------|---------|
| **Concept** | Named in `docs/product.md` but not yet discussed |
| **Scoped** | Brief written; scope, UI, and domain rules defined; open questions may remain |
| **Ready** | All open questions resolved; requirement breakdown confirmed |

## Requirement Lifecycle

```
Draft ↔ Approved → Implemented → Updated ↔ Approved → Implemented → ...
                                 Updated → Implemented  (rollback)
```

| Status | Meaning |
|--------|---------|
| **Draft** | Being written. Not ready for implementation. |
| **Approved** | Reviewed and confirmed. Ready for implementation. |
| **Implemented** | Code matches the requirement. |
| **Updated** | Modified after implementation. Changes pending approval. |

For `docs/requirements/index.md` status ranking: `Draft` < `Approved` < `Updated` < `Implemented`. `Updated` ranks below `Implemented` because it signals pending rework.

Valid transitions:
- `Draft → Approved`: reviewed and ready for initial implementation
- `Approved → Draft`: needs further work — only valid before the requirement has ever been `Implemented`
- `Approved → Implemented`: code matches the requirement
- `Implemented → Updated`: requirement modified after implementation
- `Updated → Approved`: changes reviewed and confirmed; ready for re-implementation
- `Approved → Updated`: approved changes need further work — only valid after the requirement has previously been `Implemented`
- `Updated → Implemented`: changes declined — roll back to match current implementation

## Git Conventions

- Do not add `Co-Authored-By:` trailers to commit messages.

## Project Notes

<!-- Add any project-specific instructions for Claude here. This section is preserved when running /aid:update. -->
