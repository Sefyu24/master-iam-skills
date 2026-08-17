# Authorization verification plan

## Context

- Application or API:
- Environment:
- Protected boundary:
- Policy version:
- Test data restrictions:

## Test actors

| Actor | Organization | Role or relationship | Session state | Expected scope |
|---|---|---|---|---|
| | | | | |

## Test cases

| ID | Actor | Action | Resource | Setup | Expected result | Observed result | Evidence |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Required boundaries

- Unauthenticated request
- Wrong role
- Cross-user object
- Cross-organization object
- Collection and search filtering
- Sensitive field read
- Sensitive field change
- Expired sharing or delegation
- Removed role or membership
- Stale session or token
- Administrative operation
- Background or service path
- Alternate entry point
- Denied-event logging

## Completion conditions

- Every sensitive action has an expected allow test.
- Every sensitive action has at least one denied test.
- Organization-scoped data has a cross-organization test.
- Permission changes have propagation tests.
- Failures leave the protected resource unchanged.
- Results do not expose secrets or unnecessary personal data.
