# Access review playbook

Use this guide to inspect a web application, application programming interface (API), worker, or policy design. It holds the full set of review checks.

## 1. Set the objective

Determine whether each protected action is allowed only for the intended actor, resource, tenant, and context.

Connect three layers:

1. **Business rule:** What should the actor be able to do?
2. **Policy decision:** Which data and rules produce allow or deny?
3. **Enforcement:** Which trusted component prevents the operation when the decision is deny?

Do not stop after you find an authentication check. Do not stop after you find one authorization helper. Trace the requested operation and the data it affects.

## 2. Establish scope

Record:

- the authorized repository, service, or test environment;
- review-only or fix-authorized mode;
- the relevant applications and data stores;
- the known tenant boundaries;
- the excluded systems;
- the available policy documents and tests;
- the limits on runtime testing.

## 3. Orient in the repository

Identify:

- the application framework and its version;
- the server and client boundaries;
- the routing and API conventions;
- the authentication and session libraries;
- the authorization helpers, middleware, guards, decorators, policies, and policy engines;
- the database access layer and the database policies;
- caches, object storage, queues, background workers, and webhooks;
- the tenant, team, and ownership models;
- the role, permission, group, sharing, invitation, and delegation data;
- tests, architecture records, and security requirements.

Search for both positive and negative authorization language. Useful terms include `authorize`, `permission`, `role`, `admin`, `owner`, `member`, `tenant`, `organization`, `workspace`, `account`, `policy`, `guard`, `can`, `deny`, `forbidden`, `session`, `claims`, `scope`, `impersonate`, `invite`, and `share`. Search for each common name of the tenant boundary.

A function called `requireAdmin` proves nothing until you read what it checks.

## 4. Record the intended policy

Do not treat existing code as the intended policy. Build a preliminary table of actors, actions, and resources. Use `../templates/permission-map.md` when the review needs the full map.

For each important action, record:

- the intended actors;
- the denied actors;
- the ownership or tenant rule;
- the relevant resource state;
- the required role, permission, attribute, or relationship;
- the source of the rule;
- the unanswered questions.

If documentation and implementation conflict, report the conflict as a Policy question and name both sources. Do not silently choose one as correct.

## 5. Inventory the access surface

List every path that can cause a protected effect. A protected effect is any operation that reads data, changes state, discloses files, changes permissions, creates exports, invokes tools, or performs administration.

Include:

- pages and views that load protected data;
- HTTP (Hypertext Transfer Protocol) routes and API handlers, including mobile and public APIs;
- GraphQL, RPC (remote procedure call), and WebSocket operations;
- server actions and form handlers;
- file download, upload, and preview;
- signed uniform resource locator (URL) creation and other object-storage access;
- bulk actions, exports, imports, and reports;
- invitation, sharing, role assignment, and ownership transfer;
- support, impersonation, debug, internal, and administrative tools;
- webhooks and integrations;
- scheduled jobs, queues, and event consumers;
- service-to-service calls and agent tool calls;
- database procedures and database policies when relevant.

A signed URL is a time-limited link that grants access to one stored object. Check who can create it, for which object, and for how long.

Include read operations. Data exposure is an authorization failure even when no state changes.

## 6. Trace the complete operation

For each representative operation:

1. Identify the caller-controlled input.
2. Identify the authenticated identity and the session or token claims that the code trusts.
3. Identify how the code selects the tenant context. Confirm that the tenant comes from trusted context, not from caller input.
4. Locate the function-level policy decision and the component that enforces it.
5. Follow the resource lookup.
6. Confirm object-level authorization for the requested record.
7. Confirm field-level restrictions for reads and writes, including response filtering.
8. Follow the service, storage, and database calls to the final effect. Include side effects, events, stored files, and queued work.
9. Check alternate routes, aliases, batch operations, and internal calls that reach the same action.
10. Check the failure behavior, the audit evidence, and the denied-path tests.

Record the file and line for each important step. A missing step becomes a weakness only after you confirm that no other trusted layer supplies it.

A protected interface does not prove that its API is protected. A route-level role check does not prove that the data query enforces ownership or tenant scope.

## 7. Check each boundary

### Authentication is not authorization

Some business rules also require a check of role, permission, relationship, tenant, ownership, or resource state. When such a rule applies, flag a protected operation that checks only whether a user is signed in.

For authentication, session, or federation detail, read `authentication-and-federation.md`.

### Interface controls

Hidden buttons and protected client routes improve the user experience. They do not protect a server operation. Confirm enforcement in a trusted server or data layer.

When a server handler performs a protected action and checks no identity or permission, report a Verified weakness. If only upstream middleware or gateway coverage is unseen, report a Probable weakness with High confidence, name that one fact, and put the server-side check first in the recommended order.

### Function

Verify that each sensitive operation checks the actor's permission before it runs. Give special attention to:

- administrator and support functions;
- role and permission management;
- bulk operations and exports;
- impersonation;
- debug and internal endpoints;
- operations that more than one route can reach.

### Object

Verify authorization for the selected resource after the system resolves its real owner and tenant. This check finds insecure direct object reference (IDOR), which API guidance also calls broken object level authorization (BOLA).

Unsafe pattern:

```text
load the object by caller-supplied id from the global collection
perform the action
```

Safer pattern:

```text
load the object from a collection already scoped to the actor or tenant
evaluate the requested action and the resource state
perform the action
```

Random identifiers make guessing harder. They do not replace authorization. Do not rely on identifier secrecy.

When a lookup filters by a tenant from server-side session context and a policy check on the loaded object follows, record both as Effective controls. Report a weakness only when evidence shows that the caller controls the tenant value or that it can be missing. Otherwise, ask where the session tenant comes from as a Policy question.

Test list, read, create, change, remove, share, export, and administrative actions separately. A check on one action does not prove that the other actions are protected.

### Collection

Verify that list, search, count, export, and aggregate queries return only permitted records. A count or a search result can disclose data that the actor cannot open.

### Field

Review sensitive fields separately for reads and writes. Sensitive fields include:

- role, permission, group, or scope;
- owner, tenant, or parent resource;
- account status, approval state, and visibility;
- price, credit, limit, or workflow state;
- security flags and identity links;
- internal notes and personal data.

Reject or ignore each field that the caller is not permitted to control. Do not pass an unfiltered request body into a model update. Security guidance calls this failure mass assignment. Recommend an allow list of writable fields for each actor and action. Filter each response so that the caller receives only the fields it is permitted to read.

When the schema or the validation layer is not visible, state severity conditionally (see `finding-severity.md`) and label the business consequence as inference.

### Tenant isolation

Trace the tenant context from authentication to the final data, cache, storage, queue, and background operation. Derive the active tenant from trusted context. Bind the actor and the resource to the same tenant unless an explicit cross-tenant rule applies.

Check:

- whether the caller can select an arbitrary tenant identifier;
- whether the resource belongs to the active tenant;
- whether queries include an enforceable tenant boundary;
- whether global identifiers bypass tenant scoping;
- whether cache keys and storage keys include trusted tenant context;
- whether jobs and messages carry validated tenant context;
- whether support and platform administrators follow explicit, audited cross-tenant rules;
- whether onboarding and offboarding isolate or remove tenant resources.

### Relationship and delegation

Review who can create, change, and remove ownership, membership, sharing, and delegation. Check relationship expiry and cascading access. Cascading access is access that one relationship passes through another, such as a folder share that reaches every document in the folder.

### Access administration

Access administration is itself a protected capability. Treat role assignment, permission changes, invitation policy, impersonation, and tenant ownership transfer as protected actions.

Check who can:

- create or change role definitions;
- assign roles or permissions;
- invite a user and choose the initial role;
- transfer resource or tenant ownership;
- create groups or nested membership;
- create public links or cross-tenant sharing.

Check whether an actor can:

- approve their own request;
- grant a permission equal to or stronger than their own;
- bypass policy through a direct grant.

### Freshness

Check what happens after:

- a role or permission is removed;
- a user leaves a tenant;
- an account is suspended or an identity is disabled;
- a resource changes owner or classification;
- a delegation expires;
- a relationship ends;
- risk requires reauthentication.

Review each mechanism that can keep old access active: session duration, token refresh, self-contained token claims, caches, policy-engine data, and active jobs. Record the accepted delay for each mechanism. Do not assume that revocation is immediate when the architecture cannot provide it.

When a self-contained token carries roles or permissions, ask these questions. Record each unanswered one as a Policy question.

1. Does refresh re-read current roles, and how long does refresh stay usable?
2. Does the server check a revocation list, deny list, or token version on each request?
3. Do sensitive actions get a fresh authorization check on the server?
4. What maximum propagation delay does the policy accept?

Revocation checks and fresh checks are compensating controls. Classify the stale claim as a Design concern. It becomes a weakness only when policy requires removal faster than the token lifetime and no compensating control exists.

When the review must design how changes propagate, read `permission-modeling.md`.

### Non-human and delegated access

Identify the effective identity that each job, webhook, service, API client, integration, and agent uses.

Check:

- the purpose and the accountable owner;
- the credential scope, lifetime, and storage location;
- the tenant and resource scope;
- whether the system preserves the initiating user when policy requires it;
- the tool permission compared with the target-resource permission;
- delegation expiry and revocation;
- downstream credentials and grants;
- audit attribution.

For a governance record or an agent delegation review, read `non-human-identities-and-agents.md`.

### Failure and logging

Verify that a denial:

- prevents the protected effect;
- does not return sensitive details;
- does not leave a partial update;
- does not fail open (allow the action) after an exception or a policy-service timeout;
- creates proportionate security evidence for sensitive operations.

Do not log raw tokens, credentials, or unnecessary sensitive data.

## 8. Avoid false positives

Before you report a weakness:

- Trace shared middleware, service layers, and database layers for enforcement that the route delegates.
- Check row-level security and storage policies.
- Check whether data-access helpers apply hidden filters.
- Confirm that the route is reachable in the deployed application.
- Confirm that an upstream gateway or verified middleware actually covers the route.
- Confirm whether a role is global or tenant-scoped. Distinguish platform administrators from tenant administrators. If the available code does not show the role scope, record it as a Policy question and list it as unverified.
- Confirm whether a batch operation re-checks each object in a trusted helper.
- Distinguish an intentionally public resource from an undocumented or accidentally unprotected resource.
- Record missing business intent as a Policy question. Record missing documentation as a Design concern when it makes correct enforcement hard to establish. Neither one proves unauthorized access.
- State when runtime configuration or database policy was unavailable. Classify the observation as a Probable weakness, not a Verified weakness, until someone confirms the missing fact.

## 9. Plan the tests

Use `../templates/verification-plan.md`. Build test cases across these actors and states:

- an unauthenticated actor;
- the correct user and resource;
- a different user with the same role;
- a lower-privileged role;
- a different tenant;
- an expired or removed membership;
- a suspended identity;
- a service or agent identity;
- each security-relevant resource state.

For each action, include:

- one expected allow;
- the expected denials;
- cross-user and cross-tenant attempts;
- direct invocation without the interface;
- object-reference substitution;
- restricted-field input;
- stale-session or stale-token behavior when relevant.

Verify the protected effect and the data state, not only the HTTP status code. After a fix, add a denied-path regression test.

## 10. Set severity and confidence

Record the evidence and the confidence before you choose severity. Read `finding-severity.md` for the levels, conditional severity, and the severity questions.

## 11. Report the observations

Use `../templates/review-report.md`.

- Lead with the Verified weaknesses of the highest severity.
- Keep Policy questions separate from weaknesses.
- Record Effective controls that materially reduce risk.
- Do not state that the application is secure because the review found no weakness.

## 12. Recommend durable corrections

Prefer a correction that expresses the real policy in one trusted, testable place.

A useful recommendation states:

- where the decision belongs;
- which trusted facts it needs;
- how the query scopes the resource;
- how denial works;
- how access changes propagate;
- which regression test proves the boundary.

Avoid recommendations that only hide controls, make identifiers harder to guess, or add another interface check without protecting the underlying operation.
