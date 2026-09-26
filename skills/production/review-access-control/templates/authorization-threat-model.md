# Authorization threat model

> Template note: Use this model to find how an actor could break an authorization rule. Give each abuse case an identifier (ID) in the form ABUSE-###. Link it to the rule it breaks (AUTH-###) in the permission map and to the tests that prove the control (TEST-###) in the verification plan. An abuse case describes a possible failure path. It is not evidence of a weakness. Report a weakness only when evidence supports it. Remove template notes from the final model.

## System boundary

- Protected service:
- Data stores:
- Identity and token issuers:
- Policy services:
- Tenants and the application's term for a tenant:
- External integrations:

## Actors and trust

Include people, administrators, support staff, services, jobs, integrations, and agents.

| Actor | Authentication | Intended trust | Prohibited actions |
|---|---|---|---|
| | | | |

## Protected resources

| Resource | Sensitivity | Owner | Tenant boundary | Important states |
|---|---|---|---|---|
| | | | | |

## Trust boundaries and enforcement points

The enforcement point must deny access when the decision point is unavailable or returns an error. Record the actual failure behavior for each boundary.

| Boundary | Untrusted input | Decision point | Enforcement point | Failure behavior |
|---|---|---|---|---|
| Client to server | | | | |
| Server to database | | | | |
| Service to service | | | | |
| Queue or job | | | | |
| Agent to tool | | | | |

## Abuse cases

| Abuse ID | Abuse case | Actor | Entry point | Affected rule | Business consequence | Required control | Test IDs |
|---|---|---|---|---|---|---|---|
| ABUSE-001 | Read or change another user's object | | | AUTH-001 | | | TEST-002 |
| ABUSE-002 | Cross the tenant boundary | | | AUTH-001 | | | TEST-003 |
| ABUSE-003 | Invoke an administrative function without permission | | | | | | |
| ABUSE-004 | Change a role, owner, or tenant field in a request | | | | | | |
| ABUSE-005 | Reuse access after removal or expiry | | | AUTH-001 | | | TEST-004 |
| ABUSE-006 | Misuse a service or agent identity | | | | | | |
| ABUSE-007 | Read unpermitted records through a list, search, or export | | | | | | |

## Assumptions and Policy questions

| Assumption or Policy question | Risk if wrong or unresolved | Decision owner |
|---|---|---|
| | | |
