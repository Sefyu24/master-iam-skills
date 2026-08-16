# Public source guide

Use primary sources for definitions, current protocol behavior, and standards mappings. Link to the source. Paraphrase it. Do not reproduce substantial sections.

## Application authorization

- **OWASP Application Security Verification Standard 5.0.0:** Use the authorization chapter for versioned application-verification requirements.
  - https://owasp.org/www-project-application-security-verification-standard/
- **OWASP Web Security Testing Guide:** Use its authorization testing material for test ideas and test boundaries.
  - https://owasp.org/www-project-web-security-testing-guide/
- **OWASP API Security Top 10, 2023:** Use the object-level, property-level, and function-level authorization risks for API review context.
  - https://owasp.org/API-Security/
- **OWASP Authorization Cheat Sheet:** Use it for implementation-oriented authorization guidance.
  - https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html

Check the license shown by each OWASP project before reusing text. Prefer links and original explanations.

## Digital identity and federation

- **NIST SP 800-63-4:** Use the final Digital Identity Guidelines and their companion volumes for current identity proofing, authenticator, authentication, federation, and assurance guidance.
  - https://csrc.nist.gov/pubs/sp/800/63/4/final
- **OAuth 2.0 Security Best Current Practice, RFC 9700:** Use it for current OAuth 2.0 security guidance.
  - https://www.rfc-editor.org/info/rfc9700
- **OpenID Connect Core 1.0:** Use it for the identity layer and ID Token model built on OAuth 2.0.
  - https://openid.net/specs/openid-connect-core-1_0.html
- **SCIM protocol and core schema, RFC 7644 and RFC 7643:** Use them for cross-domain identity provisioning concepts.
  - https://www.rfc-editor.org/info/rfc7644
  - https://www.rfc-editor.org/info/rfc7643

Use the license and legal notices published with each standard. Do not reproduce a complete standard.

## Permission models

- **NIST Role-Based Access Control project:** Use it for the stable RBAC model and separation-of-duty foundations. The project is archived, so do not present it as current implementation guidance.
  - https://csrc.nist.gov/Projects/role-based-access-control
- **NIST SP 800-162:** Use it for Attribute-Based Access Control concepts.
  - https://csrc.nist.gov/pubs/sp/800/162/upd2/final
- **Zanzibar:** Use the Google research paper for a large-scale relationship-based authorization design.
  - https://research.google/pubs/zanzibar-googles-consistent-global-authorization-system/
- **OpenFGA documentation:** Use it for public, implementation-oriented relationship modeling examples. Treat it as product documentation, not a universal standard.
  - https://openfga.dev/docs

## Source rules

1. Verify current versions before giving a standards mapping.
2. Distinguish a binding requirement from advice or design judgment.
3. Use the minimum external material needed to support the answer.
4. Write original examples for the user's application or lab.
5. Do not imply endorsement by a standards body or vendor.
