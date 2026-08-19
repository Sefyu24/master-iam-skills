---
name: review-access-control
description: Review and design access control for web applications and APIs, and teach the IAM concepts behind the decisions. Use when asked to find IDOR or BOLA, broken function-level authorization, privilege escalation, organization-isolation failures, unsafe roles or permissions, stale access, or service and AI-agent access. Also use to create permission maps, authorization threat models, verification plans, RBAC, ABAC, ReBAC, role analysis, lifecycle controls, or practical IAM study guidance for labs and certification topics.
---

# Review Access Control

Turn business access rules into evidence, permission decisions, and tests. Give a practical IAM result instead of generic security advice.

## Set the boundary

- Work only on systems and artifacts that the user is authorized to inspect.
- Use source review, local tests, and user-approved test environments.
- Do not attempt to bypass controls on a live service.
- Do not expose credentials, tokens, secrets, or unnecessary personal data.
- Treat this as focused access-control work, not a complete security or compliance assessment.
- If the user requests only a review, do not modify code.
- If the user requests a fix, make the smallest durable change and add a denied-path test.

## Choose the outcome

Use one or more paths:

1. **Inspect:** Find access-control weaknesses and support each conclusion with evidence.
2. **Model:** Define actors, protected resources, actions, policy inputs, enforcement points, lifecycle rules, and tests.
3. **Learn:** Explain IAM through the decisions present in a lab, design, or implementation.

Read `guides/clear-technical-english.md` before producing a user-facing answer.

For an inspection, read `guides/review-playbook.md`.

For a permission design, read `guides/permission-modeling.md`.

For identity, authentication, federation, lifecycle, or non-human access context, read `guides/identity-context.md`.

## Establish expected behavior

Do not treat existing code as the intended policy.

Use this evidence order:

1. Rules supplied or approved by the user
2. Product requirements and access-policy documents
3. Authorization tests and acceptance criteria
4. Consistent server-side behavior
5. Names, comments, and interface labels

Record an unresolved rule as a policy question. Do not label uncertainty as a confirmed vulnerability.

## Build an access map

Identify:

- **Actors:** people, administrators, services, workloads, integrations, and agents
- **Identity context:** authenticated identity, active organization, session, client, assurance, and delegation
- **Resources:** records, files, collections, exports, configuration, secrets, and administrative functions
- **Actions:** list, read, create, change, remove, approve, share, export, assign, impersonate, and administer
- **Policy inputs:** roles, attributes, relationships, ownership, organization, resource state, risk, and time
- **Control points:** request handlers, middleware, service methods, policy engines, database policies, object storage, queues, and background jobs
- **Change events:** grant, update, suspend, revoke, expire, offboard, and transfer ownership

Use `templates/permission-map.md` when a table will make the rule easier to verify.

## Inspect an implementation

1. Identify the application framework, authentication integration, data stores, API patterns, and organization model.
2. Inventory sensitive entry points. Include APIs, server actions, jobs, webhooks, exports, file access, support tools, and administrative operations.
3. Trace a representative request from caller-controlled input to the protected action and data query.
4. Locate the component that decides access and the component that enforces the result.
5. Check function, object, field, organization, ownership, sharing, delegation, and administrative boundaries.
6. Check what happens after a role, relationship, status, or credential changes.
7. Inspect alternate routes to the same action and background paths that do not use the interactive request flow.
8. Record evidence and confidence before choosing severity.
9. Recommend a durable correction and a regression test.

Do not accept a valid login, a hidden control, a client-side check, an unpredictable identifier, or a network location as sufficient authorization.

Use `templates/review-report.md` for the final report and `templates/verification-plan.md` for tests.

## Model permissions

1. Start with the business action and protected resource.
2. Express each important question as: `May actor A perform action B on resource C under conditions D?`
3. Select a policy model that matches the rule:
   - use RBAC for stable responsibilities;
   - use ABAC for trusted attributes and request conditions;
   - use ReBAC for ownership, membership, hierarchy, sharing, and delegation;
   - combine models when one model cannot represent the rule clearly.
4. Deny access when no approved rule permits it.
5. Place enforcement in a trusted server or data layer.
6. Define who can grant, change, review, and remove access.
7. Define how changes reach sessions, tokens, caches, jobs, and derived permissions.
8. Write allowed and denied tests across actors, roles, resources, and organizations.

## Teach through decisions

For a learning request:

1. Define the term in plain language.
2. Contrast it with one commonly confused term.
3. Connect it to the user's lab or application.
4. Show the decision an IAM practitioner or developer must make.
5. Explain the failure that a weak decision can create.
6. End with a short check or small scenario when it helps learning.

Explain the vendor-neutral principle before a vendor implementation. Do not claim that the skill covers an entire certification syllabus or guarantees an exam result.

## Report with evidence

Classify each observation as:

- **Verified weakness:** The available evidence shows a reachable access failure.
- **Probable weakness:** The evidence strongly suggests a failure, but a relevant runtime control is unavailable.
- **Design concern:** The design makes correct enforcement difficult to establish.
- **Policy question:** The intended rule is missing or contradictory.
- **Effective control:** A useful control is present and traceable.

For each weakness, state:

- severity and confidence;
- actor, action, and resource;
- evidence with file and line when available;
- realistic failure path;
- business consequence;
- smallest durable correction;
- regression test;
- public standard or guidance mapping when useful.

## Use public sources

Read `guides/public-sources.md` before mapping a result to a standard or protocol.

- Prefer current primary sources.
- Check current versions when a standard, law, framework, or vendor behavior can change.
- Separate requirements from recommendations and design judgment.
- Write explanations in original language.
- Do not reproduce substantial sections of an external source.
- Do not claim compliance unless the required verification is complete.
