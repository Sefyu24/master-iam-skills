# Evaluation cases for review-access-control

Use these cases to check that the skill finds real access-control failures and does not report false positives.

## How to run a case

1. Install the skill.
2. Start a new agent session for each case.
3. Send this prompt with the case code: `Use $review-access-control to review this code. Do not modify files. Only the code below is available.`
4. Compare the answer with the expected signal and with the passing behavior at the end of this file.

Judge the signal, not the exact wording. An answer passes when it reaches the expected classification for the right reason and with the right evidence.

## Case 1: Administrator check only in the interface

```tsx
export function AdminButton({ user }: { user: User }) {
  return user.role === "admin" ? <button>Delete tenant</button> : null;
}

export async function POST(request: Request) {
  const { tenantId } = await request.json();
  await db.tenant.delete({ where: { id: tenantId } });
  return Response.json({ ok: true });
}
```

Expected signal: A Verified weakness at the function level and the object level. The handler deletes any tenant that the caller names, and it checks no identity or permission first. Hiding the button is not server enforcement. If the reviewer cannot rule out shared middleware, a Probable weakness with High confidence also passes, but the answer must say which fact is missing.

## Case 2: Signed-in user with any object identifier

```ts
export async function GET(request: Request, context: { params: { id: string } }) {
  const session = await requireSession(request);
  const invoice = await db.invoice.findUnique({ where: { id: context.params.id } });
  return Response.json(invoice);
}
```

Expected signal: A Probable or Verified weakness at the object level. The classification depends on the database policy, which is not available. Authentication alone does not prove that the caller may read this invoice.

## Case 3: Missing tenant scope

```ts
export async function updateProject(session: Session, projectId: string, input: Input) {
  requireRole(session, "project-admin");
  return db.project.update({ where: { id: projectId }, data: input });
}
```

Expected signal: A Probable weakness across the tenant boundary when the role is tenant-scoped but the project lookup is global. The reviewer must confirm the role scope and the database policy before reporting a Verified weakness. If the role scope is unknown, the reviewer records a Policy question.

## Case 4: Update of restricted fields

```ts
export async function updateMember(memberId: string, body: Record<string, unknown>) {
  return db.member.update({ where: { id: memberId }, data: body });
}
```

Expected signal: A Probable weakness at the field level, with mass-assignment risk for fields such as role, tenant, owner, or status. Mass assignment means that the application writes every field the caller sends. Severity depends on the accepted schema and on the authorization of the caller. The correction must not pass an unfiltered request body into a model update.

## Case 5: Scoped lookup with a policy check

```ts
export async function readInvoice(session: Session, invoiceId: string) {
  const invoice = await db.invoice.findFirst({
    where: { id: invoiceId, tenantId: session.tenantId },
  });

  if (!invoice || !can(session.subject, "read", invoice)) {
    throw new ForbiddenError();
  }

  return invoice;
}
```

Expected signal: No object-level or cross-tenant weakness from this snippet. The reviewer reports the tenant-scoped lookup and the explicit policy check as Effective controls. The reviewer can ask whether `session.tenantId` comes from trusted server-side context, but must not invent a weakness.

## Case 6: Role claim that can become stale

```ts
const token = sign({ userId, roles }, { expiresIn: "24h" });

export function authorize(token: Token, requiredRole: string) {
  return token.roles.includes(requiredRole);
}
```

Expected signal: A Design concern. It becomes a weakness only when access removal must take effect faster than the token lifetime and no compensating control exists. The skill must ask about token refresh, revocation checks such as a deny list, fresh checks for sensitive actions, and the accepted propagation delay.

## Case 7: Artificial intelligence (AI) agent with a global administrator identity

```ts
const platformAdmin = createAdminClient(process.env.PLATFORM_ADMIN_TOKEN);

const tools = {
  exportTickets: ({ tenantId }: { tenantId: string }) =>
    platformAdmin.tickets.export({ tenantId }),
  disableUser: ({ userId }: { userId: string }) =>
    platformAdmin.users.disable(userId),
};

export async function handleSupportChat(session: Session, message: string) {
  return agent.run({ message, tools });
}
```

Expected signal: A Probable or Verified weakness in a non-human access path. The classification depends on who can reach `handleSupportChat`. The agent calls its tools with a platform-wide administrator credential, not with the delegated rights of the signed-in user. The language model chooses `tenantId` and `userId`, so neither the session tenant nor the user's permissions limit a tool call. The reviewer must recommend delegation and scope limits:

- the agent acts for the signed-in user, inside the session tenant;
- each tool gets the smallest scope that its task needs;
- the server authorizes each tool call for both the delegator (the signed-in user) and the agent;
- destructive tools, such as `disableUser`, need an explicit approval step;
- audit records show the human actor, the agent, the tool, and the resource.

The reviewer must also ask who can instruct the agent, where the system stores the credential, and who owns it. The same signal applies to a scheduled job that uses a global administrator identity to process the data of one tenant.

## Passing behavior

The skill must:

- separate Verified weaknesses from missing context, and record missing context as a Probable weakness or a Policy question;
- name the actor, the action, and the resource for each weakness;
- propose a correction in a trusted server-side layer or data layer;
- propose at least one denied-path regression test that checks the data effect, not only the status code;
- use the skill's observation labels: Verified weakness, Probable weakness, Design concern, Policy question, and Effective control;
- report Case 5 as an Effective control, and ask about `session.tenantId` without inventing a weakness;
- recommend delegation and scope limits for the non-human identity in Case 7;
- set severity from impact and reachability, never from a pattern name alone.
