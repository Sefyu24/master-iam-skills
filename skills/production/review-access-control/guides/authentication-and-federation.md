# Authentication and federation

Use this guide when a review or learning request involves sign-in, authenticators, assurance, sessions, single sign-on, federation, or identity protocols. For term definitions, see `glossary.md`.

## Identity sequence

Keep these activities distinct:

1. **Registration:** An identity service creates a relationship with an entity.
2. **Identity proofing:** The service establishes confidence that the applicant is the claimed person or entity.
3. **Authenticator enrollment:** The service binds an approved authenticator to the identity.
4. **Identification:** The claimant presents an identifier or identity claim.
5. **Authentication:** The claimant proves control of an enrolled authenticator.
6. **Authorization:** The system evaluates the requested action against policy.
7. **Accounting:** The system records security-relevant activity and decisions.

Steps 4 to 7 are the four security functions: identification, authentication, authorization, and accounting.

An application does not need to perform every step itself. It can rely on another service. It must still understand the trust boundary and the claims that it consumes.

## Authentication factors

Authenticators belong to three factor categories:

- something the claimant knows, such as a password or a personal identification number (PIN);
- something the claimant has, such as a security key or a registered device;
- something the claimant is, such as a fingerprint.

Context such as device state, location, behavior, or risk can influence an authentication decision. Context is not automatically an independent authentication factor.

Multi-factor authentication (MFA) uses authenticators from more than one factor category. Two passwords are still one factor category.

## Assurance

Do not collapse all identity confidence into one score. The National Institute of Standards and Technology (NIST) Digital Identity Guidelines, Special Publication (SP) 800-63-4, separate three assurance levels:

- **Identity Assurance Level (IAL):** Confidence in identity proofing.
- **Authentication Assurance Level (AAL):** Confidence in the authentication event and in authenticator management.
- **Federation Assurance Level (FAL):** Confidence in the protection and use of federated assertions.

Use the current NIST SP 800-63 suite for formal assurance requirements. Do not state that an application meets an assurance level unless someone verified its controls against the current revision. When the required assurance for a sensitive action is not documented, record a Policy question.

## Single sign-on and federation

Single sign-on (SSO) is a user experience. The user signs in once and reaches more than one application without a new primary authentication for each one.

Federation is a trust arrangement between security domains. One domain relies on identity information that another domain issues.

They often work together, but they are not synonyms. SSO can work inside one security domain without federation.

Federation uses these roles:

- **Identity Provider (IdP):** Authenticates the user and issues assertions or tokens that carry claims about that user.
- **Authorization server:** The OAuth 2.0 component that issues access tokens to client applications. An OpenID Connect provider is an authorization server that also authenticates the user and issues ID tokens.
- **Service provider:** The Security Assertion Markup Language (SAML) term for the application that accepts an assertion from the IdP.
- **Relying Party (RP):** The OpenID Connect and NIST term for the application that relies on an identity service.
- **Resource server:** The application programming interface (API) that accepts access tokens and protects resources.

The target application still makes or enforces its own business authorization decisions. A federated sign-in tells the application who the user is. It does not decide which records that user can change.

## Protocol distinctions

- **SAML 2.0:** An Extensible Markup Language (XML) standard for exchanging security assertions. Many organizations use it for browser SSO between an IdP and a service provider.
- **OAuth 2.0:** An authorization framework for delegated access to protected resources. A client application gets an access token with a limited scope. OAuth alone does not define user authentication.
- **OpenID Connect (OIDC):** An identity layer built on OAuth 2.0. It adds an ID token and standard claims about the authenticated user and the authentication event.
- **System for Cross-domain Identity Management (SCIM):** A protocol and schema for identity provisioning between domains. It runs over the Hypertext Transfer Protocol (HTTP). It does not replace authentication or business authorization. For provisioning, see `lifecycle-and-governance.md`.
- **Kerberos:** A ticket-based authentication protocol. A trusted key distribution service issues tickets that let a user reach network services.

## ID token and access token

These tokens have different purposes and audiences:

- An **ID token** tells the client application that a user authenticated, and when and how. Its audience is the client application.
- An **access token** lets a client call a protected API within a scope. Its intended audience is the resource server.

Check both directions of confusion:

- An API must not accept an ID token as an access token.
- A client must not treat an access token as proof of the user's identity.

A valid access token does not prove that the user can perform every application action. Validate the token, its intended audience and scope, and the business authorization for the requested resource.

## Sessions

Authentication creates or updates a security context. A session carries that context across requests.

Refresh capability lets a client get new tokens or extend a session without a new sign-in. An OAuth refresh token is one example.

Step-up authentication requires a stronger or more recent authentication before a sensitive action.

Review:

- how the session starts and which authentication event created it;
- how account recovery can change or replace authenticators;
- what claims and tenant context the session carries;
- how long the session and its refresh capability remain usable;
- when the application requires reauthentication or step-up authentication;
- how sign-out and revocation work, locally and at the IdP;
- what happens when role, employment status, tenant membership, ownership, or risk changes;
- whether cached or self-contained claims can remain valid after access should end.

Treat account recovery as an authentication path. A weak recovery flow can bypass a strong authenticator.

Authentication freshness and authorization freshness are related but different problems. A recent sign-in can still carry an outdated role or tenant membership.

## Review questions

- Does the application validate the issuer, signature, audience, time limits, and required token type?
- Does it confuse an ID token with an API access token?
- Which claims does it trust for business authorization? Does it treat role or group claims as permanently correct?
- Can an untrusted tenant, IdP, or claim value select a privileged context? When the application trusts several IdPs, can one IdP assert an identity that belongs to another?
- Can a callback, redirect, or account-linking flow attach the wrong external identity to a local account?
- Can account recovery replace an authenticator with weaker proof than enrollment required?
- Does sign-out end the relevant local and federated sessions?
- Is the authorization for a sensitive action bound to that action and the current context?

Report authentication and session observations separately from authorization observations. Use the observation classes that `../SKILL.md` defines. A weak sign-in flow and a missing object check need different corrections and tests.

## Public sources

For links to NIST SP 800-63-4, Request for Comments (RFC) 9700 on OAuth 2.0 security, OpenID Connect Core 1.0, SAML 2.0, and the SCIM standards (RFC 7644 and RFC 7643), see `public-sources.md`. Verify the current version before you map an observation to a requirement.
