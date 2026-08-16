# Identity context for application access

Use this guide when a review or learning request involves identity records, accounts, authentication, federation, lifecycle changes, or non-human actors.

## Working objects

- **Entity:** A person or non-person that can act or receive access.
- **Identity:** A system's representation of an entity.
- **Account:** A record that one application or platform uses for an entity.
- **Identifier:** A value that distinguishes a record within a defined namespace.
- **Authenticator:** Something the claimant controls and uses to authenticate.
- **Credential:** A trusted binding between an identity and identity evidence or one or more authenticators.
- **Permission:** An allowed action on a resource.
- **Entitlement:** A grantable access item exposed by a target system.
- **Role:** A managed permission set associated with a responsibility.
- **Relationship:** A governed connection such as member, owner, viewer, parent, or delegate.

One entity can have accounts in several systems. Do not assume that a matching display name or email address proves that two accounts belong to the same entity.

## Security functions

Keep these functions separate:

1. **Identification:** State which identity is requesting access.
2. **Authentication:** Verify the identity claim through an approved authenticator.
3. **Authorization:** Decide whether the actor can perform a requested action on a resource.
4. **Accounting:** Record security-relevant activity and decisions.

A successful sign-in supplies identity context. It does not grant every application action.

## Authentication and sessions

Review:

- how an authenticator becomes bound to an identity;
- which factors and assurance are required for sensitive actions;
- how recovery can change or replace authenticators;
- what claims enter a session;
- how long the session and refresh capability remain usable;
- when reauthentication or step-up is required;
- how sign-out, revocation, role changes, and organization changes affect existing sessions.

Authentication freshness and authorization freshness are different. A recent sign-in can still carry an outdated role or organization membership.

## Federation

Single sign-on describes the user's sign-in experience. Federation describes trust between security domains.

An application that accepts an upstream assertion or token must still:

- validate the issuer, signature, audience, and time limits;
- use the correct token type;
- protect account linking and callback flows;
- decide which claims are trusted for business authorization;
- apply local resource and organization rules;
- handle claim changes and session termination.

OAuth 2.0 supports delegated access. OpenID Connect adds an identity layer for authentication. A valid access token does not prove permission for every object or business action.

## Access change lifecycle

Model access as changing state:

- **Start:** Create or connect the identity and grant approved initial access.
- **Change:** Recalculate access when responsibility, project, organization, risk, or status changes.
- **End:** Remove access, terminate usable sessions, reassign owned resources, and preserve required evidence.
- **Temporary grant:** Record a purpose and expiry, then remove the grant automatically.

Reconciliation compares intended access with actual target state. Use it to detect failed changes, unexpected accounts, direct grants, and drift.

Correlation connects an account to the correct identity or accountable owner. Prefer stable authoritative identifiers. Treat uncertain matches as reviewable decisions.

## Non-human actors

Apply the same access discipline to a service, workload, integration, API client, or AI agent.

Record:

- purpose;
- accountable owner or team;
- approved resources and actions;
- credential type and storage location;
- creation and expiry dates;
- review interval;
- expected calling environment;
- delegation source when the actor acts for a person.

Prefer narrow, short-lived credentials when the platform supports them. Do not give a background job a global administrator identity when it needs one action on one resource type.

For an AI agent, evaluate both who can instruct it and what tools or resources it can reach. Bind delegated authority to the initiating user, approved purpose, scope, and time window.
