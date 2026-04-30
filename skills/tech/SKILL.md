---
name: tech
description: Define or update the technology stack — frontend, backend, database, infrastructure, and development tooling choices.
---

# Initialize Tech Stack

You are a senior tech lead with broad experience selecting and justifying technology stacks across the full stack.

`docs/tech_stack.md` records the technology choices for the project. Run this after architecture documents have been written — technology choices are informed by requirements and architecture.

## Principles

- **Only ask about what applies.** If the project has no mobile app, skip that section entirely. If it's an API-only backend, skip all frontend sections.
- **Accept "TBD" or "not decided yet" for any choice.** Mark it clearly in the file. Don't force decisions that haven't been made.
- **Offer options when the user is unsure.** Suggest 2-3 common choices for each decision point.
- **Infer what you can from ecosystem conventions.** If the user picks Next.js, note that TypeScript and the built-in router are implied — don't ask again. If they pick Prisma, note that the migration tool is `prisma migrate`. State the inference and confirm rather than asking from scratch.
- **For details covered by `docs/conventions.md`** (API format, naming, auth header, pagination) — do not ask about them here. Those belong in `/conventions`.

## Step 1: Gather Context

Read silently:
- `docs/product.md` — to understand the product type (web app, mobile, API-only, etc.) and infer which sections apply
- `docs/tech_stack.md` — to check if it already has real content

## Step 2: Identify Mode

Check whether `docs/tech_stack.md` has real content (not just placeholder comments or empty sections).

- **If it does not exist or is a placeholder**: proceed to [Create](#create).
- **If it exists with real content**: proceed to [Update](#update).

---

## Create

### Step C1: Determine the Product Shape

Based on `docs/product.md`, determine what kind of product this is:
- Web frontend only?
- Web + mobile?
- Backend / API only?
- Something else (CLI, desktop, embedded)?

If unclear from `docs/product.md`, ask: "Does this product have a web frontend? A mobile app? Or is it backend/API only?"

Use the answer to determine which sections of `docs/tech_stack.md` apply.

### Step C2: Gather Technology Choices

Ask in batches of 2-4 questions. Skip sections that don't apply.

**Frontend — Web** (if applicable):
- Framework and language? (Common choices: Next.js/TypeScript, React+Vite/TypeScript, Vue 3/TypeScript, SvelteKit/TypeScript)
- Styling approach? (Common: Tailwind CSS, CSS Modules, styled-components)
- State management? (Note: often not needed with server components or simple apps — confirm whether they need it. Common: Zustand, Redux Toolkit, Context API)
- Form handling? (Common: React Hook Form, Formik — or framework built-ins)

**Frontend — Mobile** (if applicable):
- Framework? (Common: React Native with Expo, Flutter)
- Language? (TypeScript for React Native, Dart for Flutter)
- Navigation library? (React Navigation for RN; built-in for Flutter)

**Backend:**
- Framework and language? (Common: Express/TypeScript, Fastify/TypeScript, NestJS/TypeScript, FastAPI/Python, Django/Python, Go/Gin, Go/Echo)
- ORM or query builder? (Common: Prisma, Drizzle, TypeORM for JS/TS; SQLAlchemy for Python; GORM for Go)
- Authentication approach? (Common: JWT with refresh tokens, server-side sessions — just the mechanism, not the header format)
- Validation library? (Common: Zod, Joi for JS/TS; Pydantic for Python; built-in for Go)

**Database:**
- Primary database? (Common: PostgreSQL, MySQL, SQLite — specify version if known)
- Migration tool? (Often tied to ORM — state the inference and confirm)
- Cache? (Common: Redis — or none if caching isn't planned)

**Infrastructure:**
- Hosting? (Common: AWS, Vercel, Railway, fly.io, DigitalOcean, self-hosted VPS)
- Containerization? (Docker — or none)
- CI/CD? (Common: GitHub Actions, GitLab CI — or none for now)
- Email service? (Common: Resend, SendGrid, AWS SES — or none if no outbound email)

**Development Tools** (if JS/TS project — skip if not applicable):
- Package manager? (Common: pnpm, npm, bun)
- Linting and formatting? (Common: Biome for both, or ESLint + Prettier)
- Testing framework? (Common: Vitest, Jest for JS/TS; Pytest for Python; built-in `testing` for Go)

### Step C3: Key Decisions

Ask: "Are there any technology choices where the reason behind the decision is worth documenting? For example: 'We chose PostgreSQL over MongoDB because our data is highly relational.' These go in the Decisions & Rationale section."

Capture each as: Decision | Choice | Rationale.

If the user has no specific decisions to record, that section can be left empty.

### Step C4: Propose Before Writing

Present a structured summary of the full tech stack. Ask: "Does this look right? Anything to change or add before I write the file?"

### Step C5: Write

Only after user approval:
1. Read `${CLAUDE_SKILL_DIR}/../../assets/templates/tech_stack_template.md` as the starting structure. Create `docs/tech_stack.md`, skipping any sections that don't apply to this project (remove the heading entirely, don't leave it empty).
2. Mark any TBD choices with `TBD — [brief note on what's being decided]` so they're easy to find later.
3. Keep inferred values (e.g., "Prisma Migrate — tied to Prisma ORM choice") with a brief note so the rationale is clear.
4. End the file with an empty Change Log section:

```markdown
## Change Log

| Date (YYYY-MM-DD HH:MM TZ) | Summary of Change | Reason |
|----------------------------|-------------------|--------|
```

---

## Update

### Step U1: Read the Tech Stack and Understand the Change

Read `docs/tech_stack.md` in full.

Ask the user what needs to change. Common triggers:
- A TBD choice has been decided
- A technology is being replaced (new framework, different database, etc.)
- A new layer is being added (e.g., adding a mobile app to a web-only project)
- A key decision rationale needs documenting

Identify which sections are affected and what is staying the same.

### Step U2: Clarify What's Changing

For each technology being changed:
- What is the new choice?
- Does it replace an existing entry or add a new section?
- If replacing: why? Should the rationale be documented in Decisions & Rationale?
- Are there knock-on effects on other sections? (e.g., switching ORM also changes the migration tool)

### Step U3: Propose Changes

Present a summary of what will change:
- Which sections will be modified and what is being added, changed, or removed
- The Change Log entry (date, summary, reason)

Ask: "Does this look right? Anything to adjust before I make the changes?"

### Step U4: Apply Changes

Only after user approval:
1. Edit only the affected sections.
2. Add a Change Log entry with date/time in `YYYY-MM-DD HH:MM TZ` format, a concise summary, and the reason.
3. If any conventions in `docs/conventions.md` were marked as provisional pending this tech decision, flag them: "Now that the tech stack is confirmed, these provisional conventions may need updating — run `/conventions` to review them."

---

## Final Review

Tell the user what was written or changed. Ask if they want any adjustments.
