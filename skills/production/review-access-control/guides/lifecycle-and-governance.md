# Lifecycle and governance

Use this guide when a review or learning request involves joiners, movers, leavers, provisioning, reconciliation, correlation, account conditions, access reviews, or stale access. For term definitions, see `glossary.md`.

Identity Governance and Administration (IGA) applies lifecycle events, requests, approvals, provisioning, reconciliation, access reviews, ownership, policy, and audit evidence to access administration. IGA is part of the broader Identity and Access Management (IAM) discipline.

## Lifecycle events

Identity state and access change over time. IGA practice uses these workforce terms:

- **Joiner:** An entity starts a governed relationship with an organization. Establish the identity, create or correlate its accounts, and grant approved initial access.
- **Mover:** An access-relevant fact changes. Recalculate access when responsibility, department, project, location, employment status, tenant membership, or risk changes. Remove access that the new context does not need.
- **Leaver:** The relationship ends. Remove access, terminate usable sessions and refresh capability, revoke keys and tokens, recover assets, reassign owned resources, and preserve required evidence.
- **Temporary change:** Grant time-bound access for support, a project, an incident, or delegated work. Record the purpose and expiry, then remove the grant automatically.

`identity-context.md` calls the same events Start, Change, End, and Temporary grant. Only the names differ.

A suspension blocks use for a period without ending the relationship. Like a leaver event, it must stop usable sessions and tokens. Unlike a leaver event, it keeps the identity and ownership records so that an administrator can restore access.

A mover event that adds new access but does not remove old access creates privilege accumulation.

Apply the same lifecycle thinking to contractors, partners, services, workloads, integrations, and agents. For non-human lifecycle and offboarding, see `non-human-identities-and-agents.md`.

## Provisioning and reconciliation

Provisioning creates or changes accounts and access in a target system. It can include policy checks, approvals, technical fulfillment, and audit records. Deprovisioning disables or removes them.

Automation does not remove governance responsibility. For each automated change, record the source, policy, approver, result, and error path. A connector that fails silently leaves access that nobody intended.

System for Cross-domain Identity Management (SCIM) standardizes provisioning between domains. It moves account and group data. It does not decide who should have access. For SCIM sources, see `public-sources.md`.

Reconciliation compares intended access with actual target-system state. It detects failed changes, unexpected accounts, direct grants, and drift.

## Correlation

Correlation links an account to the correct identity or accountable owner.

Use stable, authoritative identifiers when available. Do not rely only on a mutable email address or display name. A matching display name or email address does not prove that two accounts belong to the same entity. Treat probabilistic matches as reviewable decisions.

Incorrect correlation can merge two people, hide an orphan account, or assign one person's access history to another.

## Account conditions

- **Uncorrelated account:** No trusted link exists between the account and an enterprise identity.
- **Dormant account:** The account has not been used during the defined period.
- **Orphan account:** No valid identity or accountable owner remains responsible for the account.
- **Disabled account:** The target system prevents normal use of the account. Sessions, keys, tokens, delegated access, or downstream accounts can still exist.

These conditions can overlap. For example, an orphan account can also be dormant. Do not use the terms as synonyms.

Do not treat a disabled account as removed access until you check its sessions, keys, tokens, delegated access, and downstream accounts.

## Access reviews

An access review (also called access certification) asks an accountable reviewer to keep, change, or remove existing access. It should answer more than "Does this person still work here?"

Give the reviewer:

- the business purpose;
- current responsibilities;
- resource sensitivity;
- role grants and direct grants;
- inherited and delegated access;
- recent use, when usage data is a valid signal;
- ownership and approval authority;
- separation-of-duties conflicts (combinations that let one person complete a sensitive process alone);
- expiry and exception records.

The reviewer needs understandable permission names and enough context to make a real decision. A list of unexplained entitlement codes invites approval without review.

Record each decision. Use reconciliation to confirm that a removal decision reached the target system.

## Privilege accumulation

Privilege accumulation happens when an actor receives new grants but keeps old grants that the current responsibility does not need. Common causes:

- job changes and other mover events;
- project work;
- direct exceptions;
- duplicate accounts;
- inherited relationships;
- stale group membership.

Detect accumulation through event-driven review, reconciliation, access analysis, and ownership checks. Periodic access reviews alone can miss access that should have ended immediately.

A frequent permission pattern can reflect accumulation. Do not convert it into a role without a business explanation. See `role-design-and-mining.md`.

## Application review questions

- Does deactivation end active sessions and token refresh?
- Do group, role, and tenant membership changes take effect within the documented maximum propagation delay?
- Can a failed provisioning or revocation change remain hidden?
- Does the provisioning integration disable and remove accounts, or does it only create them?
- Does the system remove temporary access automatically when it expires?
- Who owns an account, and the resources it owns, when the original owner leaves?
- Can a deleted tenant keep active storage links, jobs, application programming interface (API) keys, or cached data?
- Are direct grants visible beside role and relationship grants?
- Can an administrator explain the complete path that produced a user's effective access?

## Report lifecycle observations

A missing governance process is usually a Design concern or a Policy question. It is not a weakness by itself.

Report a Verified weakness when evidence shows that access that should have ended is still usable. For example, a leaver's API key still reads customer records. Use Probable weakness when the code shows that path but a relevant runtime fact is unavailable.

State the maximum propagation delay that the policy accepts. Test the system after the delay expires. Use `../templates/verification-plan.md` for the test and `../templates/permission-map.md` for lifecycle rows.
