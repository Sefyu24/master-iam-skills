# IAM foundations

Use this guide when a learning request or a review needs the core identity and access management (IAM) objects, the four security functions, or the access principles.

## IAM and IGA

IAM is the discipline that controls how an organization represents each entity and governs what that entity can access. Here, organization means the enterprise or institution that runs the systems, not a tenant.

Identity Governance and Administration (IGA) is the part of IAM that governs access over time. It covers lifecycle events, access requests, approvals, provisioning, reconciliation, access reviews, ownership, policy, and audit evidence.

An application check that denies a cross-tenant read is an access-control decision. The process that approves a role assignment, reviews it on a schedule, and removes it when the holder leaves is IGA.

## Working objects

Use these objects as a working model:

1. **Entity:** A person or non-person that can act, be identified, or receive access. Non-person entities include services, workloads, devices, integrations, and artificial intelligence (AI) agents.
2. **Actor:** An entity that performs an action in a system.
3. **Identity:** A system's representation of an entity. An organization can keep one enterprise identity that connects the entity's records from several systems.
4. **Account:** The record that one application or platform uses for an entity.
5. **Identifier:** A value that distinguishes an identity or account within a defined namespace, such as an employee number or an application user identifier.
6. **Authenticator:** Something the claimant (the entity that tries to authenticate) possesses or controls and uses to authenticate.
7. **Credential:** A trusted binding between an identity and identity evidence or one or more authenticators.
8. **Permission:** An allowed action on a resource.
9. **Entitlement:** A grantable access item that a target system exposes. It can represent a permission, group, role, license, profile, or policy assignment.
10. **Role:** A managed set of permissions associated with a business or technical responsibility.
11. **Relationship:** A governed connection between an actor and a resource, or between two resources, such as member, owner, viewer, parent, or delegate.

The identity-versus-account distinction is a practical IAM model, not a universal database rule. A small application can keep both in one record. An enterprise platform often separates them so that it can correlate one entity with many accounts.

Do not assume that a matching display name or email address proves that two accounts belong to the same entity.

## Example: one employee, many records

One employee can have:

- one workforce record in the human resources (HR) system;
- one enterprise identity;
- one directory account;
- one payroll account;
- one support-system account.

The employee is the entity. Each system holds its own representation. Correlation links each account to the correct enterprise identity. When the employee leaves, every account needs an end action, not only the directory account.

## The IAAA chain

Identification, Authentication, Authorization, and Accounting (IAAA) are four security functions. Keep them separate:

1. **Identification:** State which identity is requesting access.
2. **Authentication:** Verify the identity claim through an approved authenticator.
3. **Authorization:** Decide whether the actor can perform the requested action on the requested resource.
4. **Accounting:** Record security-relevant activity and decisions.

Some sources call the fourth function accountability or auditing.

A successful sign-in supplies identity context. It does not authorize every action. Authorization evaluates each requested action on each resource.

Example: a user signs in with a passkey. That is authentication. The application must still decide whether this user can export the tenant's invoices. That is authorization. The record of the export is accounting.

Registration, identity proofing, and authenticator enrollment come before this chain.

## Access principles

- **Least privilege:** Grant only the access that an approved purpose requires.
- **Need to know:** Limit access to information to a valid business need, even inside an approved role.
- **Default deny:** Deny access when no approved rule permits it.
- **Separation of duties:** Prevent one actor from controlling incompatible steps of a sensitive process, such as requesting and approving the same access.
- **Traceability:** Record who requested, approved, granted, changed, used, reviewed, and removed access.
- **Complete mediation:** Check authorization on every request and every path that reaches a protected action. Do not rely on an earlier check.
- **Time limitation:** Expire temporary access and delegation when the approved period ends.

Least privilege limits what an actor can do. Need to know limits what information an actor can see.

## Why IAM becomes difficult

IAM connects technical state with business responsibility. Both change all the time:

- One entity can have many accounts, and the correlation between them can be wrong.
- Permission names can be unclear, so reviewers cannot tell what they approve.
- The owners of services and resources can change.
- Roles can keep historical access after responsibilities change.
- A service can keep its credentials after its project ends.
- A person can belong to several tenants, and the active tenant can change within one session.
- A shared resource can inherit access through several relationship paths.

Treat identities, accounts, and relationships as changing business data. Do not treat authorization as a one-time sign-in check.

## Go deeper

Read another guide only when the question needs more depth:

- authentication, sessions, and federation: `authentication-and-federation.md`;
- joiner, mover, and leaver events, provisioning, and access reviews: `lifecycle-and-governance.md`;
- access models and policy placement: `permission-modeling.md`;
- services, workloads, and AI agents: `non-human-identities-and-agents.md`.
