# Role design and mining

Use this guide to analyze existing access, propose roles, or review a role model. `permission-modeling.md` explains how to choose between role, attribute, and relationship models.

## Terms

- **Role:** A managed permission set associated with one responsibility.
- **Role-Based Access Control (RBAC):** A model that assigns permissions to roles and roles to actors.
- **Entitlement:** A grantable access item exposed by a target system, such as a permission, group, role, license, profile, or policy assignment.
- **Role mining:** The analysis of access data and business data to find candidate roles. It is a common Identity Governance and Administration (IGA) activity.
- **Separation of duties (SoD):** A rule that prevents one actor from holding two duties that together allow misuse, such as creating a payment and approving it.
- **Access package:** An optional, requestable set of access outside a base role.

Role mining produces evidence, not automatic truth. A business owner must confirm each candidate role before anyone assigns it.

## When a role fits

A role fits when a defined population shares a stable responsibility. A good role reduces repeated access decisions and still explains why access exists.

RBAC weakens when one role mixes business responsibility with access to specific resources. Use Attribute-Based Access Control (ABAC) or Relationship-Based Access Control (ReBAC) when attributes, tenant context, ownership, hierarchy, or sharing represent the rule better than a global role.

## Collect evidence

Use several inputs:

- current accounts, groups, roles, permissions, and direct grants;
- human resources (HR) attributes such as job, department, location, manager, project, and worker type;
- approved process descriptions;
- application and resource ownership, and resource sensitivity;
- request history, approval history, and repeated access requests;
- actual access use, when the signal is reliable and its collection is proportionate to the purpose;
- policy constraints and incompatible duties;
- exception history and temporary access;
- interviews with managers, users, role owners, application owners, and security teams.

A usage signal is not reliable when the access supports rare but required work. HR attributes are personal data. Use only the attributes the analysis needs, and do not copy personal data into the report.

Technical similarity does not prove identical business need. Do not convert a frequent permission pattern into a role without a business explanation.

## The common-pattern trap

When most members of a group hold a permission, treat the pattern as a question. A common pattern can represent old access, temporary work, direct exceptions, or privilege accumulation.

Ask:

1. Does normal work require the permission for every proposed member?
2. Why do some members not have it?
3. Are the missing grants errors, or are the current grants excessive?
4. Does the permission belong in a smaller role or an optional access package?
5. Does it conflict with another duty?
6. Does its risk require individual approval or a shorter duration?

Choose one of five outcomes:

- **Include:** Add the permission to the role.
- **Exclude:** Keep the permission out of the role, and review the current grants.
- **Separate:** Create a smaller role or an access package.
- **Govern as an exception:** Grant it individually, with an approver, a purpose, and an expiry date.
- **Investigate:** Record a Policy question when the business rule is missing.

Example: in an accounts-payable team, 42 of 50 people can change supplier bank details. First, ask why 8 people cannot. The 8 people might be new starters, which suggests missing grants. The 42 grants might also come from an old migration project, which suggests excessive access. If the same people also approve payments, the permission conflicts with that duty. The permission then needs a separate role with independent approval, even when the pattern is common.

## Role quality

A durable role has:

- a clear name and a business purpose tied to one responsibility;
- an explainable population rule for assignment;
- permissions that support the same responsibility;
- a business owner and a technical owner;
- a request and approval rule;
- SoD constraints that name incompatible roles and permissions;
- a review interval and a retirement condition;
- an exception process;
- tests or analysis that show effective access, including inherited access.

## Role model risks

Look for six risks:

- **Role explosion:** Separate roles for every tenant, object, project, or exception. The number of roles grows, and the real rule becomes hidden.
- **Permission dumping:** A role used as a container for unrelated access.
- **Hidden inheritance:** Broad access that arrives through nested groups or role hierarchies that reviewers cannot explain.
- **Ownerless roles:** Roles that remain after the business responsibility disappears.
- **Direct-grant bypass:** Individual grants that silently defeat the role model.
- **Self-escalation:** A role holder who can assign the same role or a stronger role without independent authority.

Require an independent approver for any grant of an equal or stronger role. Treat role assignment and role changes as protected administrative actions.

In a design review, classify each of these risks as a Design concern. In an inspection, report a Verified weakness only when evidence shows a reachable access failure. For example, a test shows that a role holder can assign a stronger role to their own account. Use `finding-severity.md` to assign severity.

## Measure the result

Judge a role model by its outcomes. Check whether it:

- reduces repeated grants and approvals;
- makes access easier to explain;
- reduces incorrect and stale access;
- preserves necessary exceptions;
- improves review quality;
- avoids unacceptable privilege concentration.

Use these indicators as evidence for the outcomes:

- coverage (the share of access granted through roles);
- the number and age of exceptions;
- unused permissions inside roles;
- conflicting permissions;
- roles without an active owner;
- review completion;
- access removal after a responsibility change.

Do not measure success only by coverage. A high coverage value can hide excessive or stale access that someone moved into roles.

## Deliver a role analysis

For each candidate role, state:

- the purpose, population rule, and permissions;
- the business owner and the technical owner;
- the evidence and its limits;
- SoD conflicts;
- patterns that you rejected, and why;
- open Policy questions;
- allowed-path and denied-path tests for the role's sensitive actions.

Use `../templates/permission-map.md` when a table makes the role easier to verify. `public-sources.md` lists the RBAC model sources.
