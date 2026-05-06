# AID — AI Aided Development

A Claude Code plugin for AI-driven software product development. Define your product, features, requirements, and architecture in structured Markdown — then hand them to Claude to implement, test, and deploy.

## Install

First, add the marketplace:

```
/plugin marketplace add https://github.com/gabga/aid
```

Then install the plugin:

```
/plugin install aid
```

Then bootstrap a new project:

```
/aid:bootstrap
```

This creates `CLAUDE.md`, `docs/glossary.md`, and the index files in your project repository. Run `/aid:product`, `/aid:conventions`, and `/aid:tech` to fill in the remaining docs.

## How it works

AID structures the work of defining a software product into five layers:

| Layer        | Documents                            | Purpose                                              |
| ------------ | ------------------------------------ | ---------------------------------------------------- |
| Product      | `docs/product.md`                    | What is being built, for whom, and why               |
| UI           | `docs/ui-overview.md`                | Navigation, page inventory, global UX patterns       |
| Features     | `docs/features/<slug>.md`            | What each feature does, its UI and domain rules      |
| Requirements | `docs/requirements/<slug>/<name>.md` | Data model, API, business rules, acceptance criteria |
| Architecture | `docs/architecture/<component>.md`   | How components are structured and interact           |
| Technology   | `docs/tech_stack.md`                 | Documented technology choices                        |
| Security     | `docs/security.md`                   | Security considerations                              |

Requirements describe the **current desired state** of each feature. When a feature changes, the requirement is updated in place. The Change Log tracks what changed and why.

## Skills

| Phase              | Skill                   | What it does                                              |
| ------------------ | ----------------------- | --------------------------------------------------------- |
| Setup              | `/aid:bootstrap`        | Create `docs/` structure, stubs, and `CLAUDE.md` in a new project |
| Setup              | `/aid:update`           | Update `CLAUDE.md` and stubs to the latest plugin version |
| 1 — Foundation     | `/aid:product`          | Create `docs/product.md` and seed the glossary            |
| 1 — Foundation     | `/aid:conventions`      | Create `docs/conventions.md`; grows alongside requirements |
| 2 — Feature Design | `/aid:feature`          | Create or update a feature brief                          |
| 2 — Feature Design | `/aid:ui`               | Create or update `docs/ui-overview.md`                    |
| 3 — Specification  | `/aid:requirement`      | Write or update a technical requirement document          |
| 4 — Architecture   | `/aid:arch` | Create a new architecture document                        |
| 4 — Architecture   | `/aid:arch-check`       | Validate requirements and architecture alignment          |
| 4 — Architecture   | `/aid:arch-update`      | Sync architecture after requirement changes               |
| Tech Stack         | `/aid:tech`             | Define or update `docs/tech_stack.md`                     |
| Anytime            | `/aid:audit`            | Full project consistency check                            |
| Anytime            | `/aid:status`           | Snapshot of project state                                 |
| Anytime            | `/aid:security`   | Security analysis; updates `docs/security.md`             |

## License

Released under the [MIT License](LICENSE).
