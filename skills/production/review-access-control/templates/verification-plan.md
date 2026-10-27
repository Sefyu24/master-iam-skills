# Authorization verification plan

> Template note: Give each test an identifier (ID) in the form TEST-###. In "Rule or abuse ID", list the permission-map rule (AUTH-###) and the threat-model abuse case (ABUSE-###) that the test covers. The example rows show a typical pattern for one rule: one allowed-path test and three denied-path tests. Replace the example IDs with your own. Run tests only in a local or user-approved test environment. Remove template notes from the final plan.

## Context

- Application or service:
- Environment:
- Protected boundary:
- Policy version:
- Authentication fixtures (how each test actor gets a session or token):
- Test data restrictions:

## Test actors

Add actors for other roles when the rules need them, for example a lower-privileged role and an administrator.

| Actor | Tenant | Role or relationship | Account and session state | Purpose | Expected scope |
|---|---|---|---|---|---|
| User A | Tenant A | | Active | Permitted actor or owner | Resources that the rule permits |
| User B | Tenant A | Same role as User A | Active | Same-tenant actor without access to User A's resource | Own resources only |
| User C | Tenant B | Same role as User A | Active | Cross-tenant actor | Tenant B resources only |
| User D | Tenant A | Previously had the same access as User A | Suspended or removed, with a session or token issued before the change | Stale-access test | None |
| Service A | Tenant A | | Active credential | Non-human access test (service, job, or agent) | Only the resources its purpose needs |

## Test cases

| Test ID | Rule or abuse ID | Actor | Action | Resource | Setup | Expected result | Expected data effect | Observed result | Evidence |
|---|---|---|---|---|---|---|---|---|---|
| TEST-001 | AUTH-001 | User A | | User A's resource | | Allow | | | |
| TEST-002 | AUTH-001, ABUSE-001 | User B | | User A's resource | User B sends the identifier of User A's resource. | Deny | No change or disclosure | | |
| TEST-003 | AUTH-001, ABUSE-002 | User C | | Tenant A resource | User C sends the identifier of a Tenant A resource. | Deny | No cross-tenant change or disclosure | | |
| TEST-004 | AUTH-001, ABUSE-005 | User D | | Resource that User D could reach before the change | Remove User D's access after User D signs in. Wait until the maximum propagation delay ends. | Deny | No change or disclosure | | |

Record an observed result only from an authorized run. An observed allow where the plan expects deny is evidence for a Verified weakness.

## Required boundaries

Link each boundary to at least one test, or state why it does not apply.

| Boundary | Test IDs | Reason if not applicable |
|---|---|---|
| Unauthenticated request | | |
| Wrong role | | |
| Cross-user object | TEST-002 | |
| Object-reference substitution in the path, query, body, and headers | TEST-002, TEST-003 | |
| Cross-tenant object | TEST-003 | |
| Collection and search filtering | | |
| Sensitive field read | | |
| Sensitive field change | | |
| Ownership transfer and sharing | | |
| Expired sharing or delegation | | |
| Removed role or membership | TEST-004 | |
| Stale session or token | TEST-004 | |
| Role and permission administration | | |
| Administrative operation | | |
| Background or service path | | |
| Agent access | | |
| Alternate entry point | | |
| Denied-event logging | | |

## Completion conditions

- Every sensitive action has an allowed-path test.
- Every sensitive action has at least one denied-path test.
- Every permission-map rule (AUTH-###) has at least one allowed-path test and one denied-path test.
- Every threat-model abuse case (ABUSE-###) has a test or a recorded reason for no test.
- Every required boundary has a test ID or a reason it does not apply.
- Tenant-scoped data has a cross-tenant test.
- Cross-user and cross-tenant tests use distinct records.
- Tests verify data and side effects, not only status codes.
- Permission changes have propagation tests. These tests run after the maximum delay and cover sessions, tokens, caches, and jobs when relevant.
- Failures leave the protected resource unchanged, with no partial update.
- Results do not expose secrets or unnecessary personal data.
- The tests run in continuous integration or another repeatable release gate.
