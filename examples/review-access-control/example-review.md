# Example access-control review

This example shows a finished report from the `review-access-control` skill. It reviews Case 2 and Case 3 in the [evaluation cases](../../evaluations/review-access-control/cases.md) and uses the structure of the [review report template](../../skills/production/review-access-control/templates/review-report.md).

The snippets do not include database policies, middleware, or business-policy documents. Each observation is therefore a Probable weakness, not a Verified weakness. The reviewer checked the public guidance links on 2026-09-26.

---

# Access-control review

## Outcome

- **Overall result:** Two Probable weaknesses. No Verified weakness, because the reviewer could not inspect runtime behavior or data-layer controls.
- **Highest severity:** High
- **Main affected boundary:** Object-level access to invoices and tenant isolation for projects
- **Immediate action:** Scope both lookups to the trusted tenant, check the requested action on the loaded record, and add cross-user and cross-tenant denied-path tests.

## Scope

- **Reviewed artifacts:** Case 2 and Case 3 in `evaluations/review-access-control/cases.md`
- **Review method:** Source review only. The reviewer ran no tests.
- **Authentication and session model:** `requireSession` returns an authenticated session. Case 3 receives a `Session` object that carries roles. The cases do not show the session fields.
- **Tenant model and the application's term for a tenant:** Assumed multi-tenant, because Case 3 names a missing tenant scope. The code uses no other term for a tenant. The cases do not show the schema.
- **Business-policy sources:** None supplied. The reviewer inferred the expected rules from function names and recorded them as assumptions.
- **Runtime facts available:** None. No database policy, middleware, or repository wrapper was supplied.
- **Exclusions:** The other cases, the database schema, deployment configuration, and the identity provider
- **Assumptions:** Each invoice and each project belongs to exactly one tenant. A user may read an invoice only inside the user's own tenant.
- **Unanswered policy questions:** Five

## Access map

| Rule ID | Actor | Action | Resource | Required rule | Enforcement point |
|---|---|---|---|---|---|
| AUTH-001 | Signed-in user | Read | Invoice selected by `id` | The invoice belongs to the actor's tenant, and the actor may read it. | Route handler and invoice query (Case 2) |
| AUTH-002 | Project administrator | Change | Project selected by `projectId` | The project belongs to the actor's active tenant, and the actor may change it. | `requireRole` and project update query (Case 3) |

## Observations

### Invoice lookup has no object-level check

- **Classification:** Probable weakness
- **Severity:** High
- **Confidence:** High
- **Actor:** Any signed-in user who can call the invoice endpoint
- **Action and resource:** Read an invoice that the caller selects by identifier
- **Affected rule:** AUTH-001
- **Evidence:** `evaluations/review-access-control/cases.md`, lines 33 to 36. The handler requires a session. It then loads the invoice by `id` alone and returns every field. The query does not use the session.
- **Failure path:** User A sends the identifier of an invoice that belongs to User B in another tenant. If no database policy applies, the endpoint returns User B's invoice.
- **Unverified facts:** One data-layer control, such as a database policy or a data-access wrapper, could scope the query. That control would withdraw this observation.
- **Business consequence:** If no database policy scopes the query, users can read invoices that belong to other customers. The impact depends on the invoice content and on the number of tenants exposed.
- **Durable correction:** Load the invoice with a query that is scoped to the tenant from the trusted session. Then evaluate the `read` action on the loaded invoice before the handler returns any field.
- **Regression test:** TEST-001. Create an invoice for User A in Tenant A and an invoice for User B in Tenant B. Sign in as User A. Request User B's invoice by identifier. Verify that the application denies the request and that the response contains no invoice fields.
- **Public guidance:** The Open Worldwide Application Security Project (OWASP) publishes the Application Security Verification Standard (ASVS). [ASVS v5.0.0-8.2.2 and v5.0.0-8.3.1](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x17-V8-Authorization.md) require data-specific access control and enforcement at a trusted service layer. For application programming interfaces (APIs), [OWASP API Security Top 10 2023, API1:2023 Broken Object Level Authorization](https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/) and the [OWASP Insecure Direct Object Reference (IDOR) Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html) describe this pattern. The link between these sources and this handler is the reviewer's inference.

### Project update does not check the project's tenant

- **Classification:** Probable weakness
- **Severity:** High if `project-admin` is a tenant-scoped role. None if the role is global by design.
- **Confidence:** Moderate
- **Actor:** A caller with the `project-admin` role
- **Action and resource:** Change a project that the caller selects by identifier
- **Affected rule:** AUTH-002
- **Evidence:** `evaluations/review-access-control/cases.md`, lines 45 to 47. `requireRole` checks only the role name. The update selects the project by `id` alone and does not compare the project's tenant with the active tenant of the session.
- **Failure path:** If `project-admin` applies to one tenant only, a Tenant A administrator can send the identifier of a Tenant B project. Without another data-layer control, the update changes the Tenant B project.
- **Unverified facts:** The scope of the `project-admin` role and any database policy. Both are Policy questions below.
- **Business consequence:** If the role is tenant-scoped and no data-layer control applies, one customer can change another customer's project. That result would break the tenant boundary that customers rely on.
- **Durable correction:** Take the active tenant from the trusted session. Scope the update to both `projectId` and `tenantId`. Evaluate the actor's `update` permission on the loaded project. If the role is global by design, record that rule and test it.
- **Regression test:** TEST-002. Create one project in Tenant A and one in Tenant B. Sign in as a Tenant A project administrator. Try to update the Tenant B project. Verify that the application denies the request and that the Tenant B record is unchanged.
- **Public guidance:** [OWASP ASVS v5.0.0-8.4.1](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x17-V8-Authorization.md) requires cross-tenant controls in multi-tenant applications. The [OWASP Multi-Tenant Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multi_Tenant_Security_Cheat_Sheet.html) explains how to keep tenant context trusted. The [OWASP Authorization Testing Automation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Testing_Automation_Cheat_Sheet.html) supports the regression test approach.

## Policy questions

| Question | Why it changes the result | Decision owner |
|---|---|---|
| Is `project-admin` global or tenant-scoped? | A global role makes the Case 3 update intended. A tenant-scoped role makes it a cross-tenant weakness. | Product or application owner |
| Does the database enforce tenant isolation for each row? | A row-level policy can block both paths. The reviewer would then record it as an Effective control and withdraw both observations. | Application and database owner |
| May a user read other users' invoices in the same tenant? | The answer sets the cross-user test inside one tenant. | Billing data owner |
| Which invoice fields may each actor read? | Access to an object does not grant access to every field. | Billing data owner |
| Which project fields may a project administrator change? | Case 3 writes `input` without a visible field filter. | Product owner |

## Effective controls

| Control | Boundary it protects | Evidence |
|---|---|---|
| Authenticated session before any data read | Blocks anonymous callers from the invoice endpoint | `evaluations/review-access-control/cases.md`, line 34 |
| Named role check before the update | Blocks callers without the `project-admin` role | `evaluations/review-access-control/cases.md`, line 46 |

These controls are useful. The shown code does not connect them to the selected invoice, the selected project, or the active tenant.

## Recommended order

1. Scope the invoice query to the trusted tenant, and add the object-level `read` check.
2. Confirm the scope of the `project-admin` role, then scope the project update to the active tenant.
3. Add TEST-001 and TEST-002 to the release gate.
4. Answer the policy questions, and update this report.

## Limits

- This was a focused identity and access management (IAM) and access-control review.
- This review does not claim complete application security or standards compliance.
- The absence of an observation does not prove that an area is secure.
- Checks not performed: runtime tests, database policies, middleware, and the field-level risk of the `input` write in Case 3 (the `Input` type is not shown). Any of these can change a classification.
