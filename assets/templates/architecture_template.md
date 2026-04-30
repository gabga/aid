# Architecture: <Component Name>

## Overview
What this component does, why it exists, and its primary responsibilities.

## Ownership Boundaries
Explicit declaration of what this component owns and does not own. These boundaries are used by `/arch-check` to determine whether requirements are correctly assigned to this component.

- **Owned entities:** <!-- Database tables/entities this component is the sole owner of, e.g., users, email_verifications -->
- **Owned API prefixes:** <!-- URL path prefixes this component serves, e.g., /api/v1/auth/* -->
- **Owned responsibilities:** <!-- Business capabilities this component handles, e.g., user registration, email verification, password hashing -->
- **Not owned:** <!-- Explicitly list what this component does NOT handle, even if it might seem related, e.g., "Does not handle user profile editing or password reset" -->

## System Context
Where this component sits in the overall system. Which components it communicates with and how (sync/async, protocol).

```
                ┌──────────┐
                │ Client   │
                └────┬─────┘
                     │ HTTPS
                ┌────▼─────┐
                │ This     │
                │ Component│
                └────┬─────┘
                     │
                ┌────▼─────┐
                │ Database │
                └──────────┘
```

## Components / Internal Structure
Sub-components or modules within this component:

| Component | Responsibility | Notes |
|-----------|---------------|-------|
|           |               |       |

## Data Model
Core entities owned by this component:

| Entity | Fields | Storage | Notes |
|--------|--------|---------|-------|
|        |        |         |       |

## Interfaces / APIs
Public API surface this component exposes. Summarize here; detailed contracts live in the relevant requirement documents.

| Method | Endpoint | Description | Defined In |
|--------|----------|-------------|------------|
|        |          |             |            |

## Data Flow
Step-by-step flows for key operations:

### <Operation Name>
1. Step one
2. Step two
3. Step three

## Infrastructure & Deployment
- **Runtime:** e.g., Node.js 20, Python 3.12
- **Framework:** e.g., Express, FastAPI
- **Database:** e.g., PostgreSQL 16
- **Deployment:** e.g., Docker, Kubernetes, single region
- **Configuration:** Environment variables, secrets management

## Constraints & Trade-offs
Key architectural decisions and their rationale:
- **Decision:** <what was decided>
  **Rationale:** <why>
  **Trade-off:** <what was given up>

## Security Considerations
- Authentication model
- Data protection (encryption at rest/in transit)
- Attack surface and mitigations
- Relevant compliance requirements
