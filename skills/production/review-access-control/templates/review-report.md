# Access-control review

> Template note: Set severity and confidence with `../guides/finding-severity.md`. Do not assign severity from a pattern name. Do not state that the application is secure. State what the evidence shows and what remains unverified. Keep the Actor line and the Action and resource line in every observation. When a permission map or verification plan exists, use its rule identifiers (IDs), such as AUTH-001, and its test IDs, such as TEST-001. Omit the Non-human identities section when no job, service, integration, or agent performs a reviewed action. Remove template notes from the final report.

## Outcome

- Overall result:
- Highest severity (write None when no observation has a severity):
- Main affected boundary:
- Immediate action:

## Scope

- Reviewed artifacts:
- Review method (source review, local tests, or approved test environment):
- Authentication and session model:
- Tenant model and the application's term for a tenant:
- Business-policy sources (only rules that the user supplied, or "None supplied"):
- Runtime facts available:
- Exclusions:
- Assumptions (including each rule that you inferred):
- Unanswered policy questions (details in Policy questions):

## Access map

| Rule ID | Actor | Action | Resource | Required rule | Enforcement point |
|---|---|---|---|---|---|
| AUTH-001 | | | | | |

## Non-human identities

Write Unknown when the evidence does not show a value, and add a Policy question for it.

| Identity and credential | Accountable owner | Delegator or initiating user | Credential lifetime and storage | Tenant and action scope compared with task need | Audit attribution |
|---|---|---|---|---|---|
| | | | | | |

## Observations

### [Title]

- **Classification:** Verified weakness / Probable weakness / Design concern
- **Severity:** Critical / High / Medium / Low, or a conditional level with the fact that decides it
- **Confidence:** Confirmed / High / Moderate. Record a Low-confidence item as an investigation note, not as a weakness.
- **Actor:**
- **Action and resource:**
- **Affected rule:** AUTH-###, or the rule in plain words
- **Evidence:** file and line, or test result
- **Failure path:**
- **Unverified facts:** each fact that could change the classification or the severity, such as role scope, database policy, accepted fields, or caller input. For a Design concern, state the evidence that would show an access failure.
- **Business consequence:** state it as a condition when the failure path depends on an unverified fact
- **Durable correction:**
- **Regression test:** TEST-###, and the denied path it proves
- **Public guidance:**

## Policy questions

| Question | Why it changes the result | Decision owner |
|---|---|---|
| | | |

## Effective controls

List only controls with traceable evidence. Record a possible failure of a helper that you have not seen as a Policy question or a limit, not here. Do not list tenant separation that depends only on a caller-supplied argument or query parameter as an effective control.

| Control | Boundary it protects | Evidence |
|---|---|---|
| | | |

## Recommended order

1.
2.
3.

## Limits

- This was a focused identity and access management (IAM) and access-control review.
- This review does not claim complete application security or standards compliance.
- The absence of an observation does not prove that an area is secure.
- Checks not performed:
