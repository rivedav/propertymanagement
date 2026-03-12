# Assistant Bootstrap Prompts

## Claude Code Prompt (Quick Start - Recommended)
Paste this at the start of a new Claude Code session:

```text
Read CLAUDE.md, project-docs/README.md, and project-docs/00-source-of-truth-map.md first.
Then summarize the current project state and the next 5 implementation steps.
Do not change code until I approve.
```

## Claude Code Prompt (Strict Full Context)
Use this when the task is high risk (auth, RLS, permissions, migrations):

```text
Read and follow these files before making changes:
- CLAUDE.md
- project-docs/README.md
- project-docs/00-source-of-truth-map.md
- project-docs/00-product-vision.md
- project-docs/01-mvp-scope.md
- project-docs/02-roles-and-access.md
- project-docs/03-multitenancy-rules.md
- project-docs/04-architecture.md
- project-docs/05-data-model.md
- project-docs/12-global-rules.md
- project-docs/13-project-rules.md
- project-docs/15-database-setup-tracker.md
- project-docs/playbooks/01-error-fixing-root-cause.md
- project-docs/playbooks/02-debugging-and-validation-standard.md
- project-docs/playbooks/03-command-checklists.md

Rules:
- Follow the rules and playbooks exactly as written.
- Keep scope to current MVP only.
- Do not change code until approved for this session.

First task:
Summarize current state from project-docs and propose the next 5 implementation steps.
```

## ChatGPT Prompt
Paste this at the start of a new ChatGPT session:

```text
Use this repository documentation as source of truth:
project-docs/README.md
project-docs/00-source-of-truth-map.md
project-docs/00-product-vision.md
project-docs/01-mvp-scope.md
project-docs/02-roles-and-access.md
project-docs/03-multitenancy-rules.md
project-docs/04-architecture.md
project-docs/05-data-model.md
project-docs/12-global-rules.md
project-docs/13-project-rules.md
project-docs/15-database-setup-tracker.md
project-docs/playbooks/01-error-fixing-root-cause.md
project-docs/playbooks/02-debugging-and-validation-standard.md
project-docs/playbooks/03-command-checklists.md

I need practical, non-marketing answers focused on a secure multi-tenant MVP.
When proposing or fixing changes, include risks, assumptions, and root-cause evidence.
Avoid suggesting Stripe/subdomains/workers unless I request them.
Follow the AI operating rules and do not invent business rules beyond project-docs.
```

## Strict Context Addendum
For high-risk tasks, include this file in the strict read set:
- project-docs/19-doc-governance-and-boundaries.md



## AI Governance Addendum
For architecture, security, or pre-schema tasks, include these files in the read set:
- project-docs/19-doc-governance-and-boundaries.md
- project-docs/20-ai-skill-matrix.md
- project-docs/21-ai-operating-rules.md
- project-docs/23-security-baseline.md
- project-docs/24-saas-stack-guidance.md




