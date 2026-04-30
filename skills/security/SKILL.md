---
name: security
description: Analyse requirements, architecture, and tech stack for security issues and update security.md with product-specific implementer rules.
---

# Security Check

You are a senior application security engineer with experience threat-modelling software systems.

Analyse requirements, architecture documents, and tech stack for security issues. Produce two outputs: findings (what to fix in the docs) and an updated `docs/security.md` (concrete implementation rules for developers).

## Step 1: Spawn Analysis Agent

Use the Task tool to launch an **Explore** agent with the following prompt. Wait for it to complete before proceeding.

> You are performing a security analysis of a requirements and architecture documentation repository. Read every file listed below, run all checks, and return structured findings. Do not modify any files.
>
> **Files to read:**
> - `docs/product.md`
> - `docs/tech_stack.md`
> - `docs/conventions.md`
> - `docs/glossary.md`
> - All files in `docs/features/` (glob `docs/features/*.md`)
> - All files in `docs/requirements/*/` (glob `docs/requirements/**/*.md`)
> - All files in `docs/architecture/` (glob `docs/architecture/*.md`)
> - `docs/security.md` (if it exists — read it to understand existing rules before proposing updates)
>
> Produce two sections in your response: **Findings** and **Implementer Rules**.
>
> ---
>
> ## Part 1: Findings
>
> Findings are issues in the documentation itself — gaps, bad patterns, or missing specifications that must be fixed in requirements, architecture documents, or `docs/tech_stack.md`. Group findings by severity: **Critical**, **High**, **Medium**, **Info**.
>
> For each finding, state:
> - What the issue is and where it appears (file and section)
> - Why it is a security concern
> - What change to make to the documentation to fix it
>
> ### Check 1: Authentication & Authorization Gaps
> - Does every non-public API endpoint in the requirements specify who is allowed to call it and under what conditions?
> - Are there endpoints where the caller's identity or permissions are not checked?
> - Can a user reach another user's resources by knowing a resource ID (IDOR)? Is ownership verification specified?
> - Is there any operation that changes state (create, update, delete) without an authorization check specified?
> - Are admin or privileged operations clearly restricted and documented as such?
>
> ### Check 2: Sensitive Data Exposure
> - Do any API responses include fields that should not be returned (e.g. password hashes, internal tokens, raw secrets, full PII when partial is sufficient)?
> - Are there list or search endpoints that could leak data about resources the caller does not own?
> - Are sensitive fields (tokens, secrets, keys, passwords, personal data) clearly identified in data models?
> - Do any requirements return raw tokens or secrets on reads other than the creation response?
> - Do any URL paths or query parameters carry sensitive values (tokens, passwords, session IDs)?
>
> ### Check 3: Input Validation & Injection
> - Are inputs validated in the requirements? Look for: string fields with no length limit, numeric fields with no range, free-text fields passed to queries or commands, file upload fields with no type or size constraints.
> - Are there requirements that construct queries, commands, or paths from user-supplied input without specifying sanitisation?
> - Are enum-constrained fields specified as enums, or left as open strings where only specific values are valid?
> - Are there mass-assignment risks — endpoints that accept a broad object and apply it wholesale to a record?
>
> ### Check 4: Rate Limiting & Abuse Prevention
> - Are sensitive operations (login, password reset, token generation, invitation) protected by rate limiting?
> - Are there operations that could be used to enumerate users, resources, or secrets if called in bulk?
> - Are there expensive operations (large exports, bulk reads, reports) that need throttling?
>
> ### Check 5: Secrets & Cryptography
> - Are tokens, passwords, API keys, or secrets stored as-is anywhere in the data models? They must be stored as hashes (SHA-256 or stronger) or via a key management service.
> - Is constant-time comparison specified for all secret comparisons? Timing attacks are a real risk.
> - Is encryption at rest specified for sensitive data fields (PII, payment data, credentials)?
> - Are cryptographic algorithm choices present in the architecture? Flag weak or unspecified choices.
> - Are token expiry and rotation policies specified?
>
> ### Check 6: Audit & Non-repudiation
> - Are security-relevant events (login, logout, permission change, resource deletion, export) logged?
> - Is the audit log itself protected from tampering or deletion?
> - Is there any operation that changes who can access what, without an audit trail specified?
>
> ### Check 7: Tech Stack Security
> - Based on `docs/tech_stack.md`, identify framework-specific or library-specific security concerns (e.g. ORM misuse patterns, default configs that need hardening, known vulnerability classes for chosen versions).
> - Are there missing security tools that should be in the stack (e.g. dependency scanning, SAST)?
> - Are there transport security requirements (TLS, HSTS, certificate pinning) that are unspecified?
> - Are CORS, CSP, and other security header policies defined?
>
> ### Check 8: Multi-tenancy & Data Isolation
> - If the product is multi-tenant, does every data query derive tenant scope from the verified identity token — not from a user-supplied parameter?
> - Are there any cross-tenant data leaks possible given the current data model and API design?
> - Is data isolation enforced at the query level, or only at the application logic level?
>
> ---
>
> ## Part 2: Implementer Rules
>
> Implementer rules are concrete, product-specific instructions for developers writing code. They are derived directly from the findings above and from the requirements, architecture, and tech stack as they exist. They are not generic best-practice reminders — every rule must reference a specific field, endpoint, entity, feature, or technology in this product.
>
> Write each rule as a short, imperative instruction. Examples of the right style:
> - "Do not return the `password_hash` field from any endpoint — strip it before serialising the response."
> - "Use an enum for `status` on the `Order` entity — do not accept arbitrary strings."
> - "Compare invitation tokens using constant-time comparison — do not use `==`."
> - "Derive `tenant_id` from the JWT claims, never from the request body or query parameter."
> - "Rate-limit the `/auth/login` endpoint to 5 attempts per minute per IP."
>
> Group rules by concern:
>
> ### Authentication & Authorization
> ### Data Exposure
> ### Input & Validation
> ### Secrets & Cryptography
> ### Rate Limiting
> ### Audit & Logging
> ### [Tech Stack Name] Specifics
> ### Multi-tenancy (omit section if product is single-tenant)
>
> For each rule, also note:
> - **Source:** which requirement, architecture document, or tech stack choice it is derived from
> - **Why:** one sentence on the security risk it prevents
>
> Omit any section that has no rules.
>
> ---
>
> Return your response in this exact format:
>
> ### Security Analysis Summary
> - Requirements checked: X
> - Architecture documents checked: X
> - Tech stack entries checked: X
> - Findings: X (Y critical, Z high, W medium, V info)
> - Implementer rules proposed: X (Y new, Z updates to existing)
>
> ### Findings
>
> #### Critical
> - **Issue:** [description] — **Location:** [file, section] — **Fix:** [what to change in the docs]
>
> #### High
> - **Issue:** [description] — **Location:** [file, section] — **Fix:** [what to change in the docs]
>
> #### Medium
> - **Issue:** [description] — **Location:** [file, section] — **Fix:** [what to change in the docs]
>
> #### Info
> - **Issue:** [description] — **Location:** [file, section] — **Fix:** [what to change in the docs]
>
> If a severity level has no findings, omit it.
>
> ### Implementer Rules
>
> #### Authentication & Authorization
> - [Rule] — **Source:** [file/section] — **Why:** [one sentence]
>
> #### Data Exposure
> - [Rule] — **Source:** [file/section] — **Why:** [one sentence]
>
> *(continue for each group that has rules)*

## Step 2: Present Findings

Present the **Findings** section from the agent's report. Then ask: "Would you like me to fix any of these in the documentation? If so, which ones?"

Apply any fixes the user selects directly using Edit and Write tools — do not spawn another agent for fixes.

## Step 3: Update security.md

After presenting findings (regardless of whether the user chose to fix any), present the proposed implementer rules from the agent's **Implementer Rules** section.

If `docs/security.md` does not exist, say: "I will create `docs/security.md` with these implementer rules." and create it.

If `docs/security.md` exists, diff the proposed rules against the existing content. Present only what is new or changed. Say: "I will add/update the following rules in `docs/security.md`." and show the diff.

In both cases, get user confirmation before writing.

Use `${CLAUDE_SKILL_DIR}/../../assets/templates/security_template.md` as the structure for `docs/security.md`. Omit any section that has no rules. Do not add a Change Log — the file is a current-state reference, not a history document. Do not add generic advice or explanations — only product-specific rules.
