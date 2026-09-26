# Non-human identities and agents

Use this guide to review, design, or explain access for software that acts without a person at the keyboard. `identity-context.md` gives the short overview. `review-playbook.md` holds the inspection checks.

## Scope and terms

- **Non-human identity (NHI):** An identity for a service, workload, application, device, integration, application programming interface (API) client, scheduled job, software robot, or artificial intelligence (AI) agent.
- **Service account:** An account that software or automation uses instead of interactive human work.
- **Accountable owner:** The person or team that accepts responsibility for a non-human identity and its delegated activity. Some governance tools call this person or team the sponsor.
- **Delegator:** The user, team, or business process that assigned the current task to an agent. When a user starts the task, that user is the initiating user.

The identity is the governed record. A credential lets software authenticate as that identity. Credentials can be keys, certificates, tokens, secrets, workload attestations, or other platform-issued credentials. Govern both. A new credential does not fix an identity with too much access. A deleted identity record does not revoke a credential that another system still accepts.

## Governance record

Record these ten fields for each non-human identity:

1. identity type and unique identifier;
2. business and technical purpose;
3. accountable owner (person or team);
4. application, environment, and expected calling environment;
5. authentication method, credential type, and credential storage location;
6. tenants, tools, APIs, data, and actions in scope;
7. creation source and approval;
8. creation date, expiry date, and review interval;
9. rotation, revocation, and incident process;
10. downstream identities and delegated access, including the delegator when the identity acts for a person.

When the platform supports them, prefer narrow, short-lived, platform-issued credentials over long-lived secrets that nobody manages.

## Lifecycle

Create the identity for an approved purpose, with an accountable owner and an expiry date.

Review the identity at its review interval. Also review it when its ownership, code, deployment, data scope, or business need changes.

When the accountable owner leaves or changes responsibility, transfer ownership or disable the identity. Do not let a human leaver event skip the identities that person owned.

Disable and remove the identity when the service or the delegation ends.

Offboarding needs more than the deletion of one account. Check and remove:

- keys, certificates, and secrets;
- active tokens and refresh capability;
- scheduled jobs and queued work;
- webhooks;
- cloud roles;
- downstream accounts and grants;
- cached permissions;
- ownership records.

After the documented propagation delay, test that each old credential fails.

## Agent delegation

An agent often acts for a user, a team, or a business process. An ownership record makes the agent accountable. It does not give the agent access. Do not grant access only because an accountable owner exists.

Before the agent performs an action, evaluate:

1. Is the agent active, and is it attributable to an accountable owner?
2. Does the agent have an approved purpose and a time limit?
3. Who delegated the current task, and who else can instruct the agent?
4. Can the delegator perform the target action?
5. Does policy allow the agent to use the requested tool?
6. Is the target resource within the delegated scope?
7. Do current policy, tenant, risk, and resource-state checks allow the action?
8. Did someone revoke or replace the delegation, or did it expire?

Allow the action only when every answer supports it. Evaluate the checks at the time of the action, not only when the delegation starts.

The agent must not exceed the access of its delegator. An instruction source other than the delegator must not expand the delegated scope.

Tool permission and resource permission are two separate decisions. Permission to call a search tool does not grant permission to read every dataset that the tool can reach.

For each agent action, record the agent, delegator, tool, resource, action, policy result, and outcome.

## Relationship model

Relationship-Based Access Control (ReBAC) can represent delegation as governed data. Read each line as subject, relation, object:

- `agent:vendor-review - sponsored-by - user:marc`
- `agent:vendor-review - acts-for - team:procurement`
- `agent:vendor-review - can-use - tool:supplier-search`
- `team:procurement - can-read - dataset:approved-vendors`

The `sponsored-by` relation records the accountable owner. It is not part of an access path.

Validate the complete path before you allow the action:

```text
Allow agent:vendor-review to read dataset:approved-vendors
through tool:supplier-search when:
the agent is active and its accountable owner is active,
agent:vendor-review acts-for team:procurement under an unexpired delegation,
team:procurement can-read dataset:approved-vendors,
agent:vendor-review can-use tool:supplier-search,
and current tenant, risk, and resource-state checks allow the read.
```

Treat changes to these relationships as sensitive authorization operations. `permission-modeling.md` explains how to protect them.

## Review risks

Look for these ten risks:

1. No active accountable owner exists.
2. A long-lived credential survives the service that created it.
3. The workload can request broader credentials than it needs.
4. The agent inherits all access of a user when the task needs only a subset.
5. The system treats tool permission and resource permission as the same decision.
6. The delegation has no purpose, expiry, or revocation path.
7. A background job uses a global administrator identity.
8. Different tenants share a cache, queue, storage prefix, or service credential without enforceable isolation.
9. Audit records show the service but not the delegator or the delegation.
10. A human leaver event does not transfer or disable the identities that person owned.

A risk in this list is a pattern to test. It is not a result. Classify each observation with the labels in `../SKILL.md`. Report a Verified weakness only when the evidence shows a reachable access failure. Use `finding-severity.md` to assign severity. Do not assign severity from the name of the risk.

A job or agent can use a platform-wide credential for a task that needs one tenant or one action. Report this excess privilege as a Probable weakness in the non-human access path. Report a Verified weakness when the evidence confirms the credential scope and a reachable path to another tenant or action. Do not reduce it to a Design concern only because the current code passes the correct tenant. With that credential, a defect, an injected instruction, or untrusted input can reach every tenant.

Report each missing governance element as an observation or a Policy question: accountable owner, delegator or initiating user, credential lifetime and storage, tenant and task scope, and audit attribution.

## Recommend a correction

When a background job or agent acts with a global administrator identity and needs one action on one resource type, recommend:

- a dedicated identity that holds only the required actions and resource types;
- an accountable owner recorded for the identity, with a review interval and an expiry date;
- binding each run to its delegator, tenant, approved purpose, and time window;
- short-lived, platform-issued credentials;
- tenant scope that the credential or the data layer enforces, such as a tenant-bound token or a row-level policy, not only a query parameter or a filter on returned results;
- a check of the delegator's access and the target tenant at the time of the action;
- an explicit human approval step before a destructive or high-impact tool call, such as disabling a user or exporting tenant data;
- audit events that name the identity, the delegator, the tool, the tenant, and the resource;
- a denied-path test that runs the job or agent with its real credential against the service or data layer in a test environment. The test requests another tenant's resource or an action outside its scope, and expects refusal and no returned data. A test that mocks the data service proves only the application filter.

## Public reference

Use the Open Worldwide Application Security Project (OWASP) Non-Human Identities Top 10 as one current risk reference. The 2025 list includes risks such as improper offboarding, secret leakage, overprivileged identities, long-lived secrets, and the reuse of one identity across several applications or environments.

- https://owasp.org/projects/non-human-identities-top-10

Treat AI-agent access control as an evolving area. Check current primary guidance before you map a result to it, and record the version and the date you checked. `public-sources.md` lists the other sources.
