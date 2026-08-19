# MASTER IAM Skills

## Skill status

### Production

Production skills are ready for normal use.

#### IAM Career Interviewer

`plan-iam-career` asks a short series of questions and creates a focused IAM career plan.

It uses:

- your location and work preferences;
- your target roles and companies;
- your education, experience, and certifications;
- your IAM and coding interests;
- current employer and job-market evidence;
- your available study time and career deadline.

The result includes a recommended role, three main gaps, one certification, one portfolio project, and goals for 30, 60, and 90 days. When file tools are available, the skill also creates a downloadable Markdown career plan.

Install it with the Agent Skills CLI:

```bash
npx skills@latest add Sefyu24/master-iam-skills --skill=plan-iam-career
```

Try it:

```text
Use $plan-iam-career to interview me and create my IAM career plan. Ask one question at a time and keep the final plan short.
```

#### Review Access Control

`review-access-control` is an IAM access-control reviewer and learning companion for web applications and APIs.

Use it to:

- inspect an authorized codebase for broken access control;
- find cross-user, cross-role, and cross-organization access risks;
- review ownership, sharing, delegation, roles, and permissions;
- design Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), or Relationship-Based Access Control (ReBAC);
- create permission maps, review reports, and authorization test plans;
- connect IAM concepts to application behavior, labs, and certification topics.

Install it with the Agent Skills CLI:

```bash
npx skills@latest add Sefyu24/master-iam-skills --skill=review-access-control
```

Try it:

```text
Use $review-access-control to review this application. Treat organization membership as a security boundary. Do not modify code. Show evidence for each conclusion.
```

## Repository structure

```text
skills/
└── production/
    ├── plan-iam-career/
    └── review-access-control/
```

## How the skills communicate

The skills use ASD-STE100-inspired clear technical English. They favor active voice, short sentences, one main idea per sentence, consistent terminology, and concrete examples.

This is a writing approach. It is not a claim of formal ASD-STE100 compliance.

## Knowledge sources

The skills use public, authoritative sources and practical IAM and application-security methods. They verify current vendor and certification details before making recommendations.

## Safety

Use the skills only on systems and artifacts that you are authorized to inspect.

The access-control skill supports focused review work. It does not replace a complete penetration test, compliance assessment, or legal review.

## License

The original content in this repository is available under the MIT License. External sources remain subject to their own licenses and notices.
