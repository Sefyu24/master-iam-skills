# Permission modeling

Use this guide to design or explain an authorization system.

For role governance, role analysis, and role mining, read `role-design-and-mining.md`. For non-human actors and agents, read `non-human-identities-and-agents.md`.

## 1. Design objective

Create an authorization model that developers can implement, business owners can explain, and testers can verify.

Start from the intended business actions. Do not start from current buttons, database tables, identity-provider groups, or framework middleware.

## 2. Start with the rule

Write each decision in this form:

`May actor A perform action B on resource C under conditions D?`

Authentication establishes identity context. Authorization applies policy to one request. Record why the access exists.

For each protected capability, ask:

1. Who needs the capability?
2. Which resource type and actions are involved?
3. Does access apply to all resources of the type or only to specific ones?
4. Do tenant, ownership, membership, hierarchy, sharing, location, time, risk, or resource state matter?
5. Who grants and revokes the access?
6. Can the actor delegate the capability?
7. How long should access last?
8. Which duties must remain separate?
9. What evidence must the system retain?

Record each unanswered item as a Policy question. Do not invent business rules from role names.

## 3. Catalog actors and resources

List the actors: anonymous visitor, authenticated user, tenant member, tenant administrator, resource owner, support operator, platform administrator, and service, workload, integration, or artificial intelligence (AI) agent.

List the protected resource types: the tenant; projects, folders, teams, and other containers inside a tenant; documents, records, files, reports, and secrets; and policies, roles, groups, invitations, and audit events.

List the relationships that grant access, such as parent, owner, member, viewer, editor, approver, and delegate.

Separate a resource type from one resource. "Document editor" can mean a global role or a relationship with one document. State which meaning each rule uses.

## 4. Choose a model

### Discretionary Access Control

Discretionary Access Control (DAC) lets a resource owner grant access under the system's rules. It fits owner-controlled sharing. It needs sharing limits, audit, and safe defaults.

### Mandatory Access Control

Mandatory Access Control (MAC) applies classifications and clearances that a central authority manages. An individual owner cannot override the policy.

### Role-Based Access Control

Role-Based Access Control (RBAC) assigns permissions to roles and roles to actors. Use it when permissions follow a stable responsibility that a defined population shares.

RBAC becomes weak when roles have no owner, grow without review, or mix business access with resource-specific access. Role explosion (a new role for every tenant, object, or exception) hides the real rule.

### Attribute-Based Access Control

Attribute-Based Access Control (ABAC) evaluates attributes of the actor, resource, action, and environment against policy. Examples are department, region, resource classification, and transaction value.

For each attribute, record its authority, allowed values, quality checks, update path, freshness requirement, and behavior when the value is missing. A missing value must not grant access.

### Relationship-Based Access Control

Relationship-Based Access Control (ReBAC) evaluates relationships between actors and resources, and between resources. Use it when access follows ownership, tenant membership, hierarchy, sharing, or delegation.

Write a relationship as `subject - relation - object`. Read each line from left to right as a sentence. The first line says that Noor is a member of the Meteor studio.

- `user:noor - member - studio:meteor`
- `studio:meteor - owns - collection:campaign-assets`
- `asset:rough-cut - contained-in - collection:campaign-assets`

The relation name sets the direction. `collection:campaign-assets - parent - asset:rough-cut` states the same fact as the third line. Choose one relation for each fact and use it everywhere. Some policy engines put the object first, so check the notation.

The system infers access only through approved relationship paths. Example: a policy lets studio members view assets in collections that their studio owns. Noor can then view `asset:rough-cut`.

Define which relationship paths permit each action. Protect relationship changes as sensitive authorization operations.

### Rule-based and adaptive access

Rule-based access evaluates named conditions. Adaptive access changes a decision or the required assurance based on current context or risk.

Context is not an unexplained override. Document which values are trusted, when the system evaluates them, and whether the result is allow, deny, or step-up authentication.

### Combined policy

Combine models only when each model represents a different part of the rule.

```text
Allow publish when:
the actor has the publisher responsibility,
the actor belongs to the studio that owns the collection,
the asset passed review,
and the session meets the required assurance.
```

The first clause uses RBAC. The second uses ReBAC. The third uses a resource-state attribute (ABAC). The fourth is an adaptive condition.

### Anti-patterns

Avoid:

- one global role for resource-specific access;
- ungoverned string comparisons spread across routes;
- a caller-supplied role, tenant, or owner that the server does not validate;
- hidden policy spread across many layers;
- policy that cannot explain why it granted access.

## 5. Place policy components

- **Policy Enforcement Point (PEP):** Intercepts the operation and applies the result.
- **Policy Decision Point (PDP):** Evaluates the applicable rules.
- **Policy Information Point (PIP):** Supplies trusted attributes, relationships, risk, and resource facts.
- **Policy Administration Point (PAP):** Manages policy definitions and changes.

These are logical responsibilities. One component can hold several of them.

Record:

- where trusted identity and tenant context originate;
- where the policy source of truth lives, and who changes it;
- where attributes and relationships come from;
- where the application requests a decision and enforces it;
- how the database, cache, object storage, queue, and background worker preserve the decision.

Enforce decisions in a trusted server or data layer. Apply the same rule on every path to the protected action. Bind each decision to the active tenant and the resource's tenant.

A global route guard can enforce broad access. A resource-specific rule needs a check after the system loads the resource, its owner, and its tenant.

An independent data-layer boundary adds defense in depth. Test each duplicate control separately.

## 6. Build the permission map

Default rule: deny unless an approved rule allows the action.

For each rule, record:

- rule identifier (ID), such as `AUTH-001`;
- result: allow or deny;
- permitted actors;
- required role, attribute, or relationship;
- tenant boundary and other conditions, including resource state;
- approval or separation rule;
- source or owner of the rule;
- decision point and enforcement point;
- audit event;
- expiry or revocation behavior;
- allowed-path and denied-path test IDs, such as `TEST-001`.

Do not leave a blank entry ambiguous. State whether a blank means deny, not applicable, or not decided. Record "not decided" as a Policy question.

Use `../templates/permission-map.md`.

## 7. Govern policy changes

Define who can:

- create permissions and roles;
- assign and revoke them;
- approve exceptions;
- create public or cross-tenant sharing;
- transfer ownership;
- change policy code or relationship data;
- use break-glass access (emergency access outside the normal approval path) during an incident;
- review and retire access.

Protect administrative actions with equal or stronger rigor than ordinary business actions. For sensitive operations, consider separation of duties, approval, step-up authentication, limited duration, and enhanced audit.

Do not let an actor grant a permission equal to or stronger than their own without independent approval.

## 8. Design lifecycle and freshness

For each grant, record its source and reason, approver or approving policy, start date, expiry, owner, review rule, and revocation event.

For every grant and revocation, define the effect on each place that can hold access:

- active sessions and refresh capability;
- authorization caches, groups, and relationships;
- background jobs;
- application programming interface (API) keys and service credentials;
- shared links and derived permissions;
- downstream systems;
- resources that a departing actor owns.

Document the maximum acceptable propagation delay. Test the system after the delay expires.

If a self-contained token (one that carries its own claims) can outlive a revocation, define its lifetime and compensating controls.

Design ownership transfer and orphan detection for resources, services, keys, integrations, and agents. An orphan has no accountable owner.

## 9. Design tests

Give every material rule at least one allowed-path test and one denied-path test.

For each sensitive action, cover every case that applies:

- expected allow;
- unauthenticated request;
- wrong role;
- cross-user and cross-tenant access;
- object substitution (a swapped resource ID);
- sensitive-field read and change;
- role assignment or ownership transfer;
- stale session, token, or claim;
- relationship removal;
- a direct API call or another entry point;
- a service, background, or agent path.

Test the resulting data and side effects. A denied response is not sufficient when part of the operation already occurred.

Use `../templates/verification-plan.md`. To find abuse cases first, use `../templates/authorization-threat-model.md`.

## 10. Review a model

Ask:

- Can two paths to the same action reach different checks?
- Can the caller choose its own role, tenant, owner, or trust context?
- Do list queries and single-object lookups return only authorized data?
- Can the caller change restricted fields such as owner, role, tenant, or status?
- Can one permission grant additional permissions?
- Can access survive the loss of the relationship or attribute that granted it?

## 11. Required outputs

A complete design includes:

1. scope and assumptions;
2. actor catalog;
3. resource and action catalog;
4. permission map with its default rule;
5. role, attribute, and relationship definitions;
6. decision and enforcement architecture;
7. grant, change, review, and revoke rules;
8. tenant-isolation rules when tenants exist;
9. non-human and delegation rules when they apply;
10. authorization verification plan;
11. open Policy questions and risks.
