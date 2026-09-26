# Permission map

> Template note: Give each rule an identifier (ID) in the form AUTH-###. Link each rule to its allowed-path and denied-path tests (TEST-###) in the verification plan. Do not leave a cell blank in a finished map. Write Deny, Not applicable, or Not decided. Record each Not decided cell as a Policy question. Remove template notes from the final map.

## Boundary

- Application or service:
- Tenant boundary and the application's term for a tenant:
- Policy owner:
- Decision authority:
- Policy version and date:
- Default rule: Deny unless an approved rule allows the action.

## Actors

| Actor | Responsibility | Identity source | Assignment source | Tenant scope | Assignment owner | Review rule |
|---|---|---|---|---|---|---|
| | | | | | | |

## Resources and actions

| Resource type | Action | Sensitivity | Owner | Lifecycle state | Audit event |
|---|---|---|---|---|---|
| | | | | | |

## Rules

| Rule ID | Actor | Action | Resource scope | Role, attribute, or relationship | Conditions | Rule source or owner | Result | Decision point | Enforcement point | Test IDs |
|---|---|---|---|---|---|---|---|---|---|---|
| AUTH-001 | | | | | | | Allow or deny | | | TEST-001 to TEST-004 |

## Access administration

Treat each change as a protected action. In Conflicting duty, record whether a requester can approve their own request. Also record whether an actor can grant a permission equal to or stronger than their own without independent approval.

| Change | Requester | Approver | Conflicting duty | Evidence | Expiry | Test IDs |
|---|---|---|---|---|---|---|
| Assign role | | | | | | |
| Remove role | | | | | | |
| Transfer ownership | | | | | | |
| Create public share | | | | | | |

## Lifecycle rules

| Event | Required access change | Owner | Test IDs |
|---|---|---|---|
| Join | | | |
| Responsibility change | | | |
| Suspension | | | |
| Leaver or tenant exit | | | |
| Delegation expiry | | | |

## Change propagation

For each removal or change, state how it reaches every place that can keep old access. Write Not applicable when a place holds no access. Add a row for any grant whose delay matters.

| Event | Sessions | Tokens | Caches | Jobs | Derived access | Maximum delay |
|---|---|---|---|---|---|---|
| Responsibility change | | | | | | |
| Suspension | | | | | | |
| Leaver or tenant exit | | | | | | |
| Delegation expiry | | | | | | |
| Remove role | | | | | | |
| Transfer ownership | | | | | | |

## Policy questions

The interim rule applies until the decision owner answers the question. Prefer deny unless the owner accepts the risk of allowing access.

| Question | Risk if unresolved | Decision owner | Due date | Interim rule |
|---|---|---|---|---|
| | | | | |
