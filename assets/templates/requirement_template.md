# REQ-<id>: <Requirement Name>

## Metadata
- **Status:** Draft | Approved | Implemented | Updated
- **Priority:** Must | Should | Could
- **Feature:** FEAT-<slug>
- **Related Requirements:** REQ-xxx, REQ-yyy
- **Related Architecture:** docs/architecture/<component>.md

## Data Model
Entities and fields this requirement introduces or modifies:

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
|       |      |             |             |

## API Contract
For each endpoint this requirement defines or modifies:

### `METHOD /path`
**Request:**
```json
{}
```
**Response (2xx):**
```json
{}
```
**Error Responses:**
- `4xx` — Description

## Business Rules
Technical rules that govern implementation: validation, authorisation, data integrity.

- BR-1: <rule>
- BR-2: <rule>

## Preconditions
Technical conditions that must be true before this requirement can execute.

## Postconditions
Expected system state after successful execution.

## Dependencies
- REQ-xxx — why it must be implemented first
- External systems or services

## Non-Functional Constraints
- **Performance:** e.g., latency < 200ms p95
- **Security:** e.g., passwords hashed with bcrypt cost 12
- **Reliability:** e.g., retry 3x on transient failure
- **Scalability:** e.g., must support 1000 concurrent users
- **Integrity:** e.g., unique constraint prevents duplicate rows

## Acceptance Criteria
- **Given** <initial state>
  **When** <action>
  **Then** <expected outcome>

## Error Handling

| Error Condition | Response Code | Behavior |
|-----------------|---------------|----------|
|                 |               |          |

## Change Log

| Date (YYYY-MM-DD HH:MM TZ) | Summary of Change | Reason |
|---------------------------|-------------------|--------|
