---
name: conventions
description: Create or update conventions.md — define API format, error envelope, naming, authentication, pagination, date format, and ID format.
---

# Initialize Conventions

You are a senior backend engineer with experience establishing API standards and technical conventions.

`docs/conventions.md` defines project-wide technical conventions: API format, response envelope, error codes, naming, authentication headers, pagination, date format, and ID format. Defining these once prevents inconsistency across all requirements and implementation.

Run this early — after `/product` — to create a stub with the basics (ID format, date format, error envelope, auth header). The file grows continuously as requirements are written and patterns emerge. It does not need to be complete before writing requirements.

## Principles

Follow the interaction principles and standing rules in CLAUDE.md. Additional principles specific to this command:

- **Propose a complete set of defaults up front.** Most projects use the same patterns. The user should only need to flag what they want to change — not answer a question per convention.
- **Derive defaults from `docs/tech_stack.md` where possible.** If the tech stack shows JWT auth, Bearer token is the natural default. If it shows PostgreSQL, UUIDs are a natural ID format.
- **Be concrete and complete.** Vague conventions cause inconsistency. Every convention must be specific enough that two developers independently make the same choice.
- **Don't ask about things determined by other files.** Framework choices live in `docs/tech_stack.md`. Feature-specific rules live in requirements. `docs/conventions.md` covers cross-cutting standards only.

## Step 1: Gather Context

Read silently:
- `docs/tech_stack.md` — to derive informed defaults; if it is a placeholder or missing, use generic defaults and note them as provisional
- `docs/product.md` — for the product name
- `docs/conventions.md` — to check if it already has real content

## Step 2: Identify Mode

Check whether `docs/conventions.md` has real content (beyond placeholder comments or empty sections).

- **If it does not exist or is a placeholder**: proceed to [Create](#create).
- **If it exists with real content**: proceed to [Update](#update).

---

## Create

### Step C1: Assemble and Propose Defaults

Based on `docs/tech_stack.md`, assemble a full set of sensible defaults. Present them to the user as a proposal to react to — not as questions:

---

**API**
- Base URL: `https://<domain>/api/v1`
- All request and response bodies: JSON (`Content-Type: application/json`)
- Field names: `snake_case`
- Versioning: path-based (`/api/v1/...`); breaking changes require a new version

**Standard Response Envelope**

Success:
```json
{
  "data": { ... },
  "meta": { "request_id": "uuid" }
}
```

Error:
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable description"
  },
  "meta": { "request_id": "uuid" }
}
```

**Standard Error Codes**

| HTTP Status | Code | Meaning |
|-------------|------|---------|
| 400 | `VALIDATION_ERROR` | Request body failed validation |
| 401 | `UNAUTHORIZED` | Missing or invalid authentication |
| 403 | `FORBIDDEN` | Authenticated but lacks permission |
| 404 | `NOT_FOUND` | Resource does not exist |
| 409 | `CONFLICT` | Resource state conflict |
| 410 | `GONE` | Resource existed but is no longer available (e.g., expired token) |
| 422 | `UNPROCESSABLE_ENTITY` | Semantically invalid request |
| 429 | `RATE_LIMITED` | Too many requests |
| 500 | `INTERNAL_ERROR` | Unexpected server error |
| 503 | `SERVICE_UNAVAILABLE` | Downstream dependency unavailable |

**Authentication**
- Header: `Authorization: Bearer <token>` *(adjust if tech stack uses sessions or API keys)*

**Pagination** (for list endpoints)
- Style: cursor-based
- Query params: `?cursor=<value>&limit=<int>`
- Defaults: limit 20, max 100
- Response includes `next_cursor` and `has_more` in `meta`

**Naming**

| Context | Convention | Example |
|---------|-----------|---------|
| API endpoints | kebab-case | `/auth/refresh-token` |
| JSON fields | snake_case | `access_token` |
| Database tables | snake_case, plural | `users`, `refresh_tokens` |
| Database columns | snake_case | `created_at` |
| Environment variables | SCREAMING_SNAKE_CASE | `DATABASE_URL` |

**Date and Time**
- Format: ISO 8601 UTC — `YYYY-MM-DDThh:mm:ssZ`
- Database storage: `timestamptz`

**ID Format**
- UUID v4, represented as strings in JSON

---

Ask: "These are the standard defaults. Do any need to change for your project? Anything to add?"

### Step C2: Clarify Customizations

If the user requests changes, confirm the details before writing. Common customizations:

- **Different pagination style** (e.g., offset-based): confirm parameter names (`page`, `per_page`), default and max values, and how total count is returned.
- **Different ID format** (e.g., ULID, auto-increment integer): confirm JSON representation and whether mixed formats will exist.
- **Different auth scheme** (e.g., API keys, sessions): confirm the header or cookie name and format.
- **Additional error codes**: confirm the HTTP status and code name.
- **Password policy** (if the project has password-based auth): confirm minimum length, character requirements, hashing algorithm and cost factor. Default if not specified: minimum 8 characters, must contain uppercase, lowercase, digit, and special character; bcrypt cost 12; plaintext never persisted or logged.
- **Any additional project-specific conventions**: ask for the rule and an example.

If `docs/tech_stack.md` shows a non-JWT auth approach (e.g., sessions), adjust the Authentication section default before proposing. If auth is not yet decided, note it as TBD.

### Step C3: Write

Only after the user confirms the customizations (or confirms the defaults are fine):
1. Read `${CLAUDE_SKILL_DIR}/../../assets/templates/conventions_template.md` as the starting structure. Create `docs/conventions.md`, using the product name from `docs/product.md` in the opening line.
2. Include only conventions that apply to this project — if there's no password auth, omit the Password Policy section.
3. If pagination isn't relevant (API-only, no list endpoints planned), note that it will be added when needed rather than leaving a placeholder.
4. End the file with an empty Change Log section:

```markdown
## Change Log

| Date (YYYY-MM-DD HH:MM TZ) | Summary of Change | Reason |
|----------------------------|-------------------|--------|
```

---

## Update

### Step U1: Read the Conventions and Understand the Change

Read `docs/conventions.md` in full.

Ask the user what needs to change. Common triggers:
- A new pattern emerged during requirements (new error code, new naming rule, etc.)
- A provisional convention has been confirmed now that the tech stack is decided
- An existing convention needs tightening or correcting

Identify which sections are affected and what is staying the same.

### Step U2: Clarify What's Changing

For each convention being changed or added, confirm:
- The exact new value or rule
- Whether it replaces an existing entry or adds a new one
- Whether the change affects any already-written requirements (flag these, but do not modify them automatically)

### Step U3: Propose Changes

Present a summary of what will change:
- Which sections will be modified and what is being added, changed, or removed
- The Change Log entry (date, summary, reason)

Ask: "Does this look right? Anything to adjust before I make the changes?"

### Step U4: Apply Changes

Only after user approval:
1. Edit only the affected sections.
2. Add a Change Log entry per the standing rules in CLAUDE.md.
3. If any conventions were marked as provisional and are now confirmed, remove the provisional note.

---

## Final Review

Show the user the key conventions written or changed (a brief summary, not the full file). Ask if they want any adjustments.

Then suggest: "Conventions are referenced from requirements rather than restated — keep this file as the single source of truth. Update it whenever a new pattern is introduced during `/requirement` sessions."
