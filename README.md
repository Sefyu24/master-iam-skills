# MASTER IAM Skills

Free, vendor-neutral Agent Skills for reviewing access control, designing permissions, and learning Identity and Access Management (IAM) through practical work.

## Review Access Control

`review-access-control` is an IAM access-control reviewer and learning companion for web applications and APIs.

Use it to:

- inspect an authorized codebase for broken access control;
- find cross-user, cross-role, and cross-organization access risks;
- review ownership, sharing, delegation, roles, and permissions;
- design Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), or Relationship-Based Access Control (ReBAC);
- create permission maps, review reports, and authorization test plans;
- connect IAM concepts to application behavior, labs, and certification topics.

## Install

Install the skill with the Agent Skills CLI:

```bash
npx skills@latest add Sefyu24/master-iam-skills --skill=review-access-control
```

The repository is private while the first release is under review. The command will work after the owner approves a public release.

## Try it

### Review a repository

```text
Use $review-access-control to review this application. Treat organization membership as a security boundary. Check whether a member of one organization can read or change another organization's workspaces, billing settings, exports, or membership. Do not modify code. Show evidence for each conclusion.
```

### Design permissions

```text
Use $review-access-control to design permissions for a collaborative media library. A person can join several studios. Each studio owns collections and media assets. Studio owners can invite guests to one collection for a limited time. Produce a permission map and an authorization test plan.
```

### Learn through a lab

```text
Use $review-access-control as my IAM learning companion for this lab. Explain the transferable concept behind each important setting. Point out assumptions, show one small application example, and ask three short questions to check my understanding.
```

## How it communicates

The skill uses ASD-STE100-inspired clear technical English. It favors active voice, short sentences, one main idea per sentence, consistent terminology, and concrete examples.

This is a writing approach. It is not a claim of formal ASD-STE100 compliance.

## Knowledge sources

The skill is written from public, authoritative sources and practical application-security methods. The source guide links to the relevant standards and projects.

## Safety

Use the skill only on systems and artifacts that you are authorized to inspect.

The skill supports focused IAM and access-control work. It does not replace a complete penetration test, compliance assessment, or legal review.

## License

The original content in this repository is available under the MIT License. External sources remain subject to their own licenses and notices.
