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

`review-access-control` is an identity and access management (IAM) access-control reviewer and learning companion for web applications and application programming interfaces (APIs).

Use it to:

- inspect an authorized codebase for broken access control;
- find cross-user, cross-role, and cross-tenant access weaknesses;
- review ownership, sharing, delegation, roles, and permissions;
- review access by service accounts, background jobs, and artificial intelligence (AI) agents;
- design Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), or Relationship-Based Access Control (ReBAC);
- analyze existing access for role design and role mining;
- create permission maps, authorization threat models, verification plans, and review reports;
- connect IAM concepts to application behavior, labs, and certification topics.

The skill classifies each observation as a Verified weakness, Probable weakness, Design concern, Policy question, or Effective control. It states the evidence and the confidence for each one.

Install it with the Agent Skills CLI:

```bash
npx skills@latest add Sefyu24/master-iam-skills --skill=review-access-control
```

Try it:

```text
Use $review-access-control to review this application. Treat tenant membership as a security boundary. Do not modify code. Show evidence for each conclusion.
```

See it in use:

- [Example review](examples/review-access-control/example-review.md): a finished report with severity, confidence, evidence, corrections, and regression tests.
- [Example prompts](examples/review-access-control/prompts.md): prompts for review, fix, design, learning, and lifecycle work.
- [Evaluation cases](evaluations/review-access-control/cases.md): seven code cases with the expected result for each one.
- [Public source guide](skills/production/review-access-control/guides/public-sources.md): the standards and public guidance that the skill cites.

The skill uses only public standards and public guidance. It contains no private course material, certification exam content, or customer data.

##### Manual install

If you cannot use the Agent Skills CLI, clone this repository and copy the skill folder:

```bash
git clone https://github.com/Sefyu24/master-iam-skills.git
cd master-iam-skills
```

| Agent | Command |
|---|---|
| Claude Code, all projects | `cp -R skills/production/review-access-control ~/.claude/skills/` |
| Claude Code, one project | `cp -R skills/production/review-access-control <project>/.claude/skills/` |
| Codex | `cp -R skills/production/review-access-control ~/.codex/skills/` |

Create the target `skills` folder first if it does not exist.

##### Claude.ai

Claude.ai needs a ZIP file in which the skill folder is the root. Create it from the `production` folder:

```bash
cd skills/production
zip -r review-access-control.zip review-access-control -x "*.DS_Store"
```

The archive must contain `review-access-control/SKILL.md`, not `SKILL.md` at the top level. Upload the ZIP file as a custom skill in the Claude.ai settings.

##### What it does not promise

- It is not a penetration test.
- It cannot prove that an application is secure. A review that finds no weakness is not proof of security.
- It does not replace a qualified security review of a high-risk production system.
- It does not confirm compliance with a standard.
- It does not cover a complete certification syllabus or guarantee an exam result.
- Use it only on systems and code that you are authorized to review.

## Repository structure

```text
skills/
└── production/
    ├── plan-iam-career/
    └── review-access-control/
        ├── SKILL.md
        ├── agents/openai.yaml
        ├── guides/
        └── templates/
examples/
└── review-access-control/
    ├── example-review.md
    └── prompts.md
evaluations/
└── review-access-control/
    └── cases.md
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
