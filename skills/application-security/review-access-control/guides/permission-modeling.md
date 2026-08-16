# Permission modeling

Use this guide to design or explain an authorization system.

## Start with the rule

Write a decision in this form:

`May actor A perform action B on resource C under conditions D?`

For each important rule, identify:

- the actor and active identity context;
- the resource type and specific resource;
- the requested action;
- ownership, membership, or delegation relationships;
- trusted attributes and their authorities;
- organization and resource boundaries;
- risk, time, resource state, and assurance conditions;
- the reason that access exists.

## Choose a model

### Role-Based Access Control

Use Role-Based Access Control (RBAC) when permissions follow a stable responsibility. Define role owners, assignment rules, review intervals, and incompatible combinations.

Avoid a new role for every object, organization, or exception. That pattern hides the real rule and creates role growth.

### Attribute-Based Access Control

Use Attribute-Based Access Control (ABAC) when policy depends on trusted facts about an actor, resource, action, or environment.

For each attribute, record its authority, allowed values, update path, freshness requirement, and behavior when the value is missing.

### Relationship-Based Access Control

Use Relationship-Based Access Control (ReBAC) when access follows ownership, membership, hierarchy, sharing, or delegation.

Represent a relationship as a subject, relation, and object:

- `user:noor - member - studio:meteor`
- `studio:meteor - owns - collection:campaign-assets`
- `asset:rough-cut - contained-in - collection:campaign-assets`

Define which relationship paths permit each action. Protect relationship changes as sensitive authorization operations.

### Combined policy

Combine models only when each part represents a different part of the business rule.

Example:

```text
Allow publish when:
the actor has the publisher responsibility,
the actor belongs to the studio that owns the collection,
the asset passed review,
and the session meets the required assurance.
```

## Place policy responsibilities

- **Enforcement:** Intercept the operation and apply the result.
- **Decision:** Evaluate the applicable rules.
- **Information:** Supply trusted attributes, relationships, risk, and resource facts.
- **Administration:** Govern policy definitions and changes.

These are logical responsibilities. A small application can implement several responsibilities in one component.

Enforce decisions in a trusted server or data layer. Apply the same rule to every path that reaches the protected action.

## Build the permission map

For each resource and action, record:

- permitted actors;
- required role, relationship, or attribute;
- organization boundary;
- resource-state constraint;
- approval or separation rule;
- enforcement point;
- audit event;
- expiry or revocation behavior;
- allowed-path test;
- denied-path test.

Use `../templates/permission-map.md`.

## Govern roles

A useful role packages permissions that support one clear responsibility.

Use evidence such as:

- approved process descriptions;
- repeated access requests;
- actual permission use when the signal is reliable;
- resource sensitivity;
- incompatible duties;
- exception history;
- interviews with role owners and application owners.

Do not convert a frequent permission pattern into a role without a business explanation. A common pattern can represent old access, temporary work, direct exceptions, or privilege accumulation.

Measure role quality through coverage, exceptions, unused permissions, conflicting permissions, ownership, review completion, and removal after responsibility changes.

## Design change behavior

For every grant and removal, define the effect on:

- active sessions;
- refresh capability;
- authorization caches;
- group and relationship data;
- queued work;
- API keys and service credentials;
- shared links;
- derived permissions;
- resources owned by a departing actor.

Document the maximum acceptable propagation delay. Test the system after the delay expires.

## Design verification

For each sensitive action, include:

- one expected allow case;
- one unauthenticated case;
- one wrong-role case;
- one cross-user case;
- one cross-organization case when organizations exist;
- one stale-session or stale-claim case;
- one relationship-removal case when relationships grant access;
- one field-level case for sensitive fields;
- one alternate-entry-point case;
- one service or background path when it exists.
