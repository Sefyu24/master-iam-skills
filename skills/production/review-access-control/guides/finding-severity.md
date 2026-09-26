# Severity and confidence

Use this guide after you record the evidence for an observation. It sets two separate values:

- **Severity** describes the realistic business impact and the reachability of the behavior.
- **Confidence** describes the strength of the available evidence.

Reachability means which actor can trigger the behavior, what access that actor needs first, and how much effort or complexity the path requires.

Record the evidence and the confidence before you choose severity. Keep the two values separate.

## Severity levels

### Critical

Use Critical when a reachable access failure can broadly compromise protected data or administrative control with limited prerequisites. Typical impacts are platform-wide or tenant-wide compromise, unrestricted cross-tenant access, and broad privileged control. The actor needs little additional access and little complexity.

Examples:

- any authenticated user can grant platform administrator access;
- a public endpoint can export the data of every tenant;
- one tenant administrator can take control of other tenants.

### High

Use High when a realistic actor can access or change sensitive resources outside the intended scope, gain materially stronger privileges, or perform a high-impact administrative action.

Examples:

- one user can read or remove another user's private records;
- a member can change the resources of another tenant;
- a standard user can invoke an administrator operation.

The last example is High even when the actor does not gain a stronger role. The administrative effect sets the level.

### Medium

Use Medium when the weakness has meaningful impact but a narrower reach. A narrower reach means meaningful prerequisites, less sensitive resources, a limited scope, or a limited stale-access window. Also use Medium for a Design concern that the supplied evidence shows on a reachable path, when that concern is likely to produce unauthorized access.

Examples:

- removed access remains active for a documented but excessive period;
- a low-impact field has no role-specific write control;
- the logs cannot attribute sensitive operations or denial events to an actor.

### Low

Use Low when the impact is narrow, when the problem is a defense-in-depth gap, or when it mainly reduces clarity, maintainability, or assurance. The evidence does not show an unauthorized outcome.

Examples:

- permission logic is duplicated and hard to audit, but the current paths enforce the intended rule;
- a public resource has no explicit policy documentation;
- denial responses disclose minor implementation detail.

An audit gap is Low when it affects routine events. It is Medium when it prevents attribution of sensitive operations or denials.

## Policy questions have no severity

When the business rule is unknown, classify the observation as a Policy question. Do not assign a severity.

State:

- the decision that the owner must make;
- who owns the decision;
- the risk of each option;
- which option would make the current behavior a weakness.

Missing documentation alone does not prove unauthorized access.

Do not state a conditional escalation, such as "this becomes Critical if users can create tenants", as the severity. Set the severity from the evidence you have, and name the fact that would change it.

## Conditional severity

When severity depends on a fact that you cannot see, such as the accepted schema, upstream validation, the caller's authorization, or the accepted propagation delay, state the level for each outcome and name the deciding fact. Examples:

- Severity: High if the accepted schema lets a member write role, tenant, owner, or status fields. Medium if only low-impact profile fields are writable.
- Severity: Medium if policy requires role removal faster than the token lifetime and no compensating control exists. None otherwise.

Assign severity to a Design concern only when the supplied code or configuration shows the unsafe design on a reachable path. When a concern depends only on a component that you have not seen, such as session creation, a policy function, a library default, or a database policy, record a Policy question or an investigation note with no severity.

When no observation has a severity, write "Highest severity: None" in the report.

## Confidence levels

- **Confirmed:** The evidence demonstrates the complete failure path. The code path and the relevant policy show the unauthorized result. The implementation is unambiguous, or a safe local test reproduces the result.
- **High:** The evidence strongly supports the conclusion, but one relevant runtime fact, control, or configuration is unavailable.
- **Moderate:** Several facts support the concern, but the result depends on missing policy, data, or deployment context. An alternative control can change the result. Use Moderate, not High, when more than one relevant fact is unavailable, for example both the role scope and the database policy. Count only facts that could block the failure path. When a handler reads no caller identity and passes none to the data layer, a database policy can act only through the same unseen middleware, so treat them as one missing fact and use High. Facts that change severity or the correction, such as role scope or cascade behavior, do not lower confidence.
- **Low:** The observation is useful for investigation but is not ready to report as a weakness. Do not present it as a headline item.

High and Low appear on both scales. Always name the scale in a report, for example "Severity: High. Confidence: Moderate."

## Match classification and confidence

The classification in `../SKILL.md` and the confidence level both describe evidence strength. Keep them consistent.

| Classification | Confidence |
|---|---|
| Verified weakness | Confirmed |
| Probable weakness | High or Moderate |
| Design concern | Confirmed or High. State which evidence would show an access failure. When the concern depends on context that you have not seen, record a Policy question or an investigation note instead. |
| Any observation with Low confidence | Record it as an investigation note, not as a weakness. |

Do not pair a Verified weakness with Moderate or Low confidence. If the evidence is incomplete, classify the observation as a Probable weakness.

## Severity questions

Answer these questions before you choose a level:

1. Which actor can reach the behavior?
2. What authentication or existing privilege does the actor need first?
3. Which resources, tenants, or people does the behavior affect, and how many?
4. Can the actor read, create, change, remove, approve, impersonate, or grant access?
5. Can the owner reverse the effect?
6. Does the behavior involve sensitive, regulated, financial, or security data?
7. Can the actor repeat or automate the behavior?
8. Do monitoring, approvals, or other controls materially reduce the likelihood or the impact?
9. Does the behavior create a path to stronger privileges?

Tie severity to the business context of the application. A pattern name such as insecure direct object reference (IDOR) or privilege escalation describes a type of access failure, not its impact. Do not assign severity from a pattern name alone.
