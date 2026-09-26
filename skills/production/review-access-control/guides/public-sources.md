# Public source guide

Use primary sources for definitions, current protocol behavior, and standards mappings. Verify the current version before you make a production, compliance, or standards claim. Link to the source. Paraphrase it. Do not reproduce substantial sections.

Most sources below come from the Open Worldwide Application Security Project (OWASP), the National Institute of Standards and Technology (NIST), the Internet Engineering Task Force (IETF), the Organization for the Advancement of Structured Information Standards (OASIS), and the OpenID Foundation.

**Link check:** Every linked address in this guide returned the Hypertext Transfer Protocol (HTTP) status 200 on 2026-09-26. Where an address redirected, this guide lists the final address. Check a link again before you cite it in a deliverable. When a link fails, find the current official address, check it, and update this date.

## Application authorization

- **OWASP Application Security Verification Standard (ASVS) 5.0.0:** Use chapter V8 (Authorization) for versioned application-verification requirements. Use the project page to find the current release. Use the GitHub tag and the V8 chapter file to cite the exact 5.0.0 text.
  - https://owasp.org/projects/asvs
  - https://github.com/OWASP/ASVS/tree/v5.0.0
  - https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x17-V8-Authorization.md
- **OWASP Web Security Testing Guide (WSTG):** Use the authorization testing chapter for test ideas and test boundaries: authorization bypass, privilege escalation, and insecure direct object references. The chapter link pins version 4.2. Check the project page for a later stable release.
  - https://owasp.org/projects/web-security-testing-guide
  - https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/05-Authorization_Testing/
- **OWASP Application Programming Interface (API) Security Top 10, 2023:** Use it for API review context. The relevant risks are API1 (Broken Object Level Authorization, BOLA), API3 (Broken Object Property Level Authorization), and API5 (Broken Function Level Authorization).
  - https://api-security.owasp.org/editions/2023/en/0x11-t10/
  - https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/
- **OWASP Authorization Cheat Sheet:** Use it for implementation guidance: least privilege, deny by default, a permission check on every request, checks in a trusted location, safe failure, logging, and authorization tests.
  - https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html
- **OWASP Insecure Direct Object Reference (IDOR) Prevention Cheat Sheet:** Use it for object-reference threats and object-level authorization checks. It explains that complex identifiers do not replace an access check.
  - https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html
- **OWASP Authorization Testing Automation Cheat Sheet:** Use it to express a permission map as data (the cheat sheet calls it an authorization matrix) and run it as automated integration tests. Use it to design regression tests after a correction.
  - https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Testing_Automation_Cheat_Sheet.html
- **OWASP Multi-Tenant Security Cheat Sheet:** Use it for tenant isolation: tenant context, database isolation, cross-tenant object access, cache and session isolation, asynchronous work, file storage, tenant onboarding and offboarding, and audit.
  - https://cheatsheetseries.owasp.org/cheatsheets/Multi_Tenant_Security_Cheat_Sheet.html

The older ASVS project address (`www-project-application-security-verification-standard`) now ends in HTTP 404. Do not use it.

**License:** OWASP project content is generally available under the Creative Commons Attribution-ShareAlike 4.0 International license (CC BY-SA 4.0), unless a project states another license. The ASVS applies CC BY-SA 4.0 to all of its content. Link, attribute, and paraphrase. Check the license of each project before you copy text. Copied text needs attribution. Adapted text also carries the ShareAlike condition.

- https://creativecommons.org/licenses/by-sa/4.0/

## Digital identity and federation

- **NIST Special Publication (SP) 800-63-4, Digital Identity Guidelines:** Use the final base volume and its companion volumes for identity proofing and enrollment (SP 800-63A-4), authentication and authenticator management (SP 800-63B-4), and federation and assertions (SP 800-63C-4). The suite defines the Identity Assurance Level (IAL), the Authentication Assurance Level (AAL), and the Federation Assurance Level (FAL). NIST published the final version in July 2025. It supersedes SP 800-63-3. The first link is the formal publication record. The second link opens the web version of all four volumes.
  - https://csrc.nist.gov/pubs/sp/800/63/4/final
  - https://pages.nist.gov/800-63-4/
- **Best Current Practice for OAuth 2.0 Security, Request for Comments (RFC) 9700:** Use it for current OAuth 2.0 security guidance and the patterns it advises against.
  - https://www.rfc-editor.org/info/rfc9700/
- **OpenID Connect (OIDC) Core 1.0 incorporating errata set 2:** Use it for the identity layer on top of OAuth 2.0, the ID token, and standard identity claims. The first link pins errata set 2. The second link always shows the latest errata set. Check it for a later set before you cite a section.
  - https://openid.net/specs/openid-connect-core-1_0-errata2.html
  - https://openid.net/specs/openid-connect-core-1_0.html
- **Security Assertion Markup Language (SAML) 2.0, OASIS Standard:** Use the OASIS document set for assertions, protocols, bindings, profiles, and metadata. The same directory holds the approved errata.
  - https://docs.oasis-open.org/security/saml/v2.0/
- **System for Cross-domain Identity Management (SCIM), RFC 7644 and RFC 7643:** Use RFC 7644 for the provisioning protocol and RFC 7643 for the core schema. SCIM moves identity data between domains. It does not decide who should have access.
  - https://www.rfc-editor.org/info/rfc7644/
  - https://www.rfc-editor.org/info/rfc7643/

ASVS 5.0.0 also has identity chapters: V6 (Authentication), V7 (Session Management), V9 (Self-contained Tokens), and V10 (OAuth and OIDC). Use them when you map an authentication or session observation.

Use the license and legal notices published with each standard. Do not reproduce a complete standard.

## Permission models

- **NIST Role-Based Access Control (RBAC) project:** Use it for the stable RBAC model: role assignment, permissions, role hierarchy, and separation of duties (SoD), which NIST calls separation of duty. NIST has archived the project, so do not present it as current implementation guidance.
  - https://csrc.nist.gov/Projects/role-based-access-control
- **NIST SP 800-162, Attribute Based Access Control (ABAC) Definition and Considerations:** Use it for ABAC concepts. It describes subject, object, operation, and environment attributes. This skill calls them actor, resource, action, and environment attributes.
  - https://csrc.nist.gov/pubs/sp/800/162/upd2/final
- **Zanzibar:** Use the Google research paper as a primary technical reference for Relationship-Based Access Control (ReBAC) at large scale.
  - https://research.google/pubs/zanzibar-googles-consistent-global-authorization-system/
- **OpenFGA documentation:** Use the authorization concepts page for public explanations of fine-grained and relationship-based authorization. Use the documentation root for modeling examples. Treat it as product documentation, not a universal standard.
  - https://openfga.dev/docs/authorization-concepts
  - https://openfga.dev/docs/

## Non-human identities

- **OWASP Non-Human Identities (NHI) Top 10, 2025:** Use it as one current risk reference for service accounts, API keys, workloads, and other non-human identities. The 2025 list includes improper offboarding (NHI1:2025), secret leakage (NHI2:2025), overprivileged NHI (NHI5:2025), and long-lived secrets (NHI7:2025).
  - https://owasp.org/projects/non-human-identities-top-10

Treat access control for artificial intelligence (AI) agents as an evolving area. Check the current edition of each source before you map a result to it, and record the date you checked.

## Cite a versioned requirement

Requirement numbers can change between releases. When you map an observation to a numbered requirement:

1. Record the exact version with the identifier. For ASVS, use the format `v<version>-<chapter>.<section>.<requirement>`. Example: `ASVS v5.0.0-8.2.2`, the requirement that limits data-specific access to actors with explicit permission for those data items.
2. Do not cite an ASVS identifier without a version. The ASVS README says that readers should take such an identifier to mean the latest release.
3. Link the pinned source, such as the GitHub tag, next to the identifier.
4. Check that the requirement in that version matches the observation.
5. Present the mapping as supporting guidance, not as a compliance result.

In ASVS 5.0.0, V8 has four sections:

- **V8.1:** authorization documentation;
- **V8.2:** general authorization design, including function-level, data-specific, and field-level access;
- **V8.3:** operation-level authorization, including enforcement in a trusted service layer, prompt application of access changes, and access based on the originating actor's permissions, not those of an intermediary service;
- **V8.4:** other considerations, including cross-tenant controls and administrative interfaces.

Match the requirement to the boundary. Cite the requirement, not only the section. In ASVS 5.0.0:

- function-level access maps to 8.2.1;
- object-level (data-specific) access maps to 8.2.2;
- field-level read or write access, including mass assignment, maps to 8.2.3;
- access changes that must take effect promptly map to 8.3.2;
- access through an intermediary service maps to 8.3.3;
- cross-tenant controls map to 8.4.1.

For stale claims in a self-contained token, also check chapter V9. For field-level API findings, OWASP API3:2023 (Broken Object Property Level Authorization) also applies. Confirm each number against the v5.0.0 source before you cite it.

## Source rules

1. Prefer a current official standard or project page.
2. Distinguish a binding requirement from advice or design judgment.
3. Separate a source statement from your own inference. Label the inference as yours.
4. Use the minimum external material needed to support the answer.
5. Paraphrase. Quote only a short passage when the exact wording matters, such as a normative "MUST" in an RFC. Mark the quotation and link the source.
6. Write original examples for the user's application or lab.
7. Do not claim that a design or application complies with a standard unless the required verification is complete.
8. Do not imply endorsement by a standards body, author, or vendor, such as NIST, OWASP, the IETF, OASIS, the OpenID Foundation, Google, or OpenFGA.
