# Access review playbook

Use this guide to inspect a web application, API, worker, or policy design.

## 1. Establish scope

Record:

- authorized repository, service, or test environment;
- review-only or fix-authorized mode;
- relevant applications and data stores;
- known organization boundaries;
- excluded systems;
- available policy documents and tests;
- limits on runtime testing.

## 2. Locate the access surface

Inventory operations that read data, change state, disclose files, change permissions, create exports, invoke tools, or perform administration.

Include:

- HTTP routes and API handlers;
- GraphQL or RPC operations;
- server actions;
- object-storage access;
- webhooks and integrations;
- scheduled and queued work;
- support and impersonation tools;
- invitation and sharing flows;
- bulk actions and exports.

Search for policy language such as `authorize`, `permission`, `role`, `owner`, `member`, `organization`, `workspace`, `guard`, `deny`, `session`, `scope`, `invite`, `share`, and `impersonate`.

## 3. Trace the complete operation

For each representative path:

1. Identify caller-controlled input.
2. Identify the trusted identity and organization context.
3. Locate the policy decision.
4. Locate enforcement.
5. Follow the resource lookup and data mutation.
6. Check response filtering and sensitive fields.
7. Check side effects, events, storage, and queued work.
8. Locate audit evidence and denied-path tests.

A protected interface does not prove that its API is protected. A route-level role check does not prove that the data query enforces ownership or organization scope.

## 4. Check common boundaries

### Function

Verify that each sensitive operation checks the actor's permission. Include administrative and support operations.

### Object

Verify authorization for the selected resource after the system resolves its real owner and organization. Do not rely on identifier secrecy.

### Collection

Verify that list, search, count, export, and aggregate queries return only permitted records.

### Field

Review sensitive fields separately for reads and writes. Prevent caller-controlled changes to owner, organization, role, approval state, visibility, or security status unless policy permits them.

### Organization

Derive the active organization from trusted context. Bind both the actor and resource to the same approved boundary unless an explicit cross-organization rule applies.

### Relationship and delegation

Review who can create, change, and remove ownership, membership, sharing, and delegation. Check relationship expiry and cascading access.

### Administration

Treat role assignment, permission changes, invitation policy, impersonation, and organization transfer as protected actions.

### Freshness

Check whether access survives a role removal, organization removal, ownership transfer, disabled identity, expired delegation, or terminated relationship.

### Non-human path

Check service identities, API clients, workers, webhooks, and agents. Verify purpose, owner, credential scope, and resource boundary.

## 5. Avoid false positives

- Trace shared middleware and data policies before declaring a missing check.
- Confirm whether a role is global or organization-scoped.
- Check whether data-access helpers apply hidden filters.
- Distinguish a public resource from an undocumented resource.
- Treat missing business intent as a policy question.
- State when runtime configuration or database policy was unavailable.

## 6. Set severity and confidence

Choose severity from realistic impact and reachability. Do not use the name of a weakness as the severity.

- **Critical:** A reachable path can broadly compromise protected data or administrative control with limited prerequisites.
- **High:** A realistic actor can reach sensitive resources outside the intended scope or gain materially stronger privileges.
- **Medium:** The weakness has meaningful impact but needs narrower conditions, affects less sensitive data, or leaves a limited stale-access window.
- **Low:** The impact is narrow or primarily reduces assurance, maintainability, or auditability.

Set confidence separately:

- **Confirmed:** Evidence demonstrates the complete failure path.
- **High:** Evidence strongly supports the conclusion, but one relevant runtime fact is unavailable.
- **Moderate:** Several facts support the concern, but an alternative control can change the result.
- **Low:** The observation is useful for investigation but not ready to report as a weakness.

## 7. Recommend durable corrections

Prefer a correction that expresses the real policy in one trusted, testable place.

A useful recommendation states:

- where the decision belongs;
- which trusted facts it needs;
- how the resource is scoped;
- how denial works;
- how access changes propagate;
- which regression test proves the boundary.

Avoid recommendations that only hide controls, make identifiers harder to guess, or add another interface check without protecting the underlying operation.
