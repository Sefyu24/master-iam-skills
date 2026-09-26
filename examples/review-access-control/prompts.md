# Example prompts for review-access-control

Each prompt names the skill with `$review-access-control`. Codex uses the `$` prefix to call a skill by name. Other agents that support Agent Skills can also load the skill when a request matches its description.

Use these prompts only on code and systems that you are authorized to review.

## Review without changing code

```text
Use $review-access-control to review this repository. Focus on object-level authorization, tenant isolation, role administration, and stale access. Do not modify files. Report evidence with file and line references.
```

Result: a review report based on [`templates/review-report.md`](../../skills/production/review-access-control/templates/review-report.md). The skill does not change files.

## Review and fix

```text
Use $review-access-control to inspect this API for broken access control. Fix each Verified weakness with High or Critical severity, and add a denied-path test for each fix. Keep existing behavior outside the affected permissions.
```

Result: small code corrections, denied-path tests, and a short report of the changes.

## Design

```text
Use $review-access-control to design permissions for a multi-tenant project-management application. Users can belong to several tenants. Projects can contain private documents. Project owners can invite external collaborators. Produce a permission map and a verification plan.
```

Result: a permission map based on [`templates/permission-map.md`](../../skills/production/review-access-control/templates/permission-map.md) and a verification plan based on [`templates/verification-plan.md`](../../skills/production/review-access-control/templates/verification-plan.md).

## Learn

```text
Use $review-access-control to teach me why authentication does not prove authorization. Use one API example, one business example, and three short questions.
```

Result: a short explanation that defines both terms, shows one decision, and ends with three questions.

## Lifecycle

```text
Use $review-access-control to map what must happen when an employee leaves the organization. Include sessions, tokens, shared resources, jobs, service identities, and ownership transfer.
```

Result: the lifecycle and change-propagation rows of a permission map, with the required access change, the owner, and a test for each item.
