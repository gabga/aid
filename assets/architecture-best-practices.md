# Architecture Best Practices

Canonical standards for evaluating architecture documents. Check only what is applicable to the component — skip categories that genuinely do not apply. Flag every deviation; do not assume compliance.

## REST API conventions

- Are there verbs in URL paths? (anti-pattern: `/activate`, `/confirm`, `/verify`, `/cancel` as path segments — use nouns and HTTP methods instead)
- Do HTTP methods match their semantics? (GET = safe+idempotent, PUT = idempotent replace, PATCH = partial update, DELETE = remove, POST = create or non-idempotent action)
- Do any query parameters change the fundamental semantics of a method? (anti-pattern: boolean flags like `?force=true` or `?permanent=true` on DELETE — use a separate endpoint with a distinct path)
- Are sensitive values (tokens, secrets, passwords) present in URL paths? (anti-pattern: embedding a secret token as a path segment — tokens in paths appear in server logs, proxy logs, and browser history; use the request body instead)
- Are collection resources plural nouns and singleton sub-resources singular nouns?

## Token and secret storage

- Are all tokens, secrets, passwords, and credentials stored as cryptographic hashes (SHA-256 or stronger)? Flag any field that stores a raw token, key, or password.
- Is constant-time comparison specified for every secret comparison? Timing attacks are a real risk on string equality checks against secret values.
- Are raw token values ever returned in responses other than at creation time? (Acceptable: return once at creation. Not acceptable: return on every read.)

## Internal API isolation

- Are all `/internal/*` endpoints documented as restricted to service-to-service traffic on the internal network and not exposed to the public internet?
- Is there any internal endpoint that could be confused with a public one (e.g. missing `/internal/` prefix)?

## Cross-service data references

- Are all references to entities owned by another service documented as logical FKs (no DB-level FK enforcement)?
- Is the absence of DB-level enforcement acknowledged and mitigated (e.g. via deletion flows, orphan cleanup)?

## Async reliability

- If the component emits async events (outbox), is at-least-once delivery documented?
- If the component consumes async events, is idempotency documented (e.g. a deduplication table keyed on event ID, or an equivalent mechanism)?
- Are there cases where a distributed operation could leave the system in a partial state if one step fails? Is the compensation strategy documented?

## Multi-tenancy and authorization

- Does every query on scoped data (tenant, organisation, workspace, etc.) derive the scope from the verified identity token — not from a user-supplied parameter?
- Is authorization enforced server-side for every protected endpoint? Is there any endpoint that relies solely on client-side checks?
- Can a user access another scope's data by knowing a resource ID? (Verify that ownership checks are applied to every resource lookup, not just listing endpoints.)

## Data integrity

- Are nullable fields justified? Flag nullable fields where a non-null default would be safer.
- Are unique constraints defined for fields that must be unique (e.g. per-tenant name uniqueness, token hashes)?
- Are cascade behaviors (on delete/update) defined for all FK relationships?
