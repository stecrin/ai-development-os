# AI Development OS — Prompt Builder

## Purpose

This file helps you create controlled Claude Code prompts without relying on memory.
Copy the template that matches your situation, fill in the blanks, and paste it into Claude Code.

---

## The Universal Prompt Structure

Every strong AI Development OS prompt follows this structure:

```
AI Development OS is active.

ROLE:
[Pick one: Analyst / Documentation Developer / Developer / QA Reviewer /
 Security Reviewer / DevOps Reviewer / Architect / Product Manager]

Goal:
[One sentence — what you want to happen.]

Task type:
[One of: investigation / documentation / feature / fix / review / deployment]

Context:
[What is already known, what has already been done, or what the problem is.]

Scope:
[What files or areas Claude is allowed to look at.]

Permissions:
You may create or edit only:
- [list files]

Do not edit:
- [list files]
Do not commit automatically.

Rules:
[Any specific rules for this task.]

Safety gates:
[List what requires human confirmation before proceeding.]

After the work:
Run: git diff -- [filename]
Run: git status --short
Do not commit automatically.

Output only:
- files inspected
- files created or changed
- sections created
- concerns found
- git status summary
- next recommended step
```

---

## Role Selection Guide

| Role | Use when |
|------|----------|
| **Analyst** | You don't know what's wrong yet. Something feels off. You want Claude to inspect and report — not fix. |
| **Documentation Developer** | You want a README, guide, reference file, or documentation created or updated. |
| **Developer** | You know what to build and want Claude to implement it on a feature branch. |
| **QA Reviewer** | Code is written. You want Claude to check it before committing. |
| **Security Reviewer** | You want Claude to check for secrets, auth issues, API exposure, privacy risks, or data leaks. |
| **DevOps Reviewer** | You are preparing to commit, open a PR, deploy, or change infrastructure. |
| **Architect** | The change spans multiple files, touches a data model, or needs a design decision before coding starts. |
| **Product Manager** | You have a vague idea and need it turned into a concrete, testable spec before any work begins. |

**Typical flow for small tasks:** `Developer → QA Reviewer`
**Typical flow for new features:** `Analyst → Architect → Developer → QA Reviewer → Security Reviewer → DevOps Reviewer`
**When unsure:** always start with Analyst.

---

## Default Analyst Prompt

Use this when you don't know what's wrong yet or when entering a new area.

```
AI Development OS is active.

ROLE:
Analyst

Goal:
Inspect [file / area / feature] and report what you find. Do not make any changes.

Task type:
Investigation

Context:
[Describe what you observed or what seems wrong. It's okay to say "I don't know the cause yet."]

Scope:
Inspect only:
- [list files or folders to inspect]

Do not read unrelated files.
Do not use 1M context.

Permissions:
Read only. Do not create or edit any files.
Do not commit automatically.

Output only:
- files inspected
- findings (what you observed)
- open questions
- risks found
- next recommended step
```

---

## Create Something Prompt

Use this when you want Claude to create a new file, feature, README, script, or page.

```
AI Development OS is active.

ROLE:
[Documentation Developer / Developer / Architect — pick the one that fits]

Goal:
Create [file name or feature description].

Task type:
[documentation / feature / script]

Context:
[What this file or feature is for. What problem it solves. Any existing files it should stay consistent with.]

Inspection scope:
Before writing, inspect only:
- [list files Claude should read for context]

Do not scan unrelated projects.
Do not use 1M context.

Permissions:
You may create or edit only:
- [list files]

Do not edit:
- [list files — especially existing framework files]
Do not commit automatically.

Rules:
- Stay consistent with the existing style and structure.
- Do not add features beyond what is requested.
- Do not create planning or analysis documents.
- Write only what was asked for.

After the work:
Run: git diff -- [filename]
Run: git status --short
Do not commit automatically.

Output only:
- files inspected
- file created
- sections created
- concerns found
- git status summary
- next recommended step
```

---

## Fix Code Prompt

Use this when something is broken and needs a controlled fix.

```
AI Development OS is active.

ROLE:
Developer

Goal:
Fix [describe the specific bug or broken behavior].

Task type:
Fix

Context:
[Describe what is broken. Include error messages, file names, line numbers if known.
Describe what the correct behavior should be.]

Scope:
Inspect only the files needed to understand and fix this issue:
- [list files]

Permissions:
You may edit only:
- [list files]

Do not edit:
- [list files]
Do not commit automatically.

Rules:
- Make the smallest effective change.
- Do not refactor surrounding code.
- Do not add features.
- Do not change unrelated files.
- If the fix requires a larger change, stop and explain before proceeding.

Safety gates:
Stop and ask before changing anything related to:
- authentication or authorization
- database schema or migrations
- environment variables or secrets
- payment logic
- production deployment

After the work:
Run: git diff -- [filename]
Run: git status --short
Do not commit automatically.

Output only:
- files inspected
- files changed
- what was fixed and why
- concerns found
- git status summary
- next recommended step
```

---

## Documentation Prompt

Use this for README files, project docs, portfolio text, CV project entries, or public-facing explanations.

```
AI Development OS is active.

ROLE:
Documentation Developer

Goal:
Create [or update] [file name] — [one-line description of what it should contain].

Task type:
Documentation

Context:
[Describe the audience, the purpose of the document, and any existing docs it should stay consistent with.]

Inspection scope:
Before writing, inspect only:
- [list files needed for context]

Do not scan unrelated projects.
Do not use 1M context.
Do not make changes to existing files unless explicitly listed below.

Permissions:
You may create or edit only:
- [list files]

Do not edit:
- [list files — especially existing docs, CLAUDE.md, README.md, framework files]
Do not commit automatically.

Style:
- Clear, practical, and copyable.
- Beginner-friendly where needed.
- No long theory. Focus on real use.
- No emojis unless requested.

After the work:
Run: git diff -- [filename]
Run: git status --short
Do not commit automatically.

Output only:
- files inspected
- file created or updated
- sections written
- concerns found
- git status summary
- next recommended step
```

---

## QA Review Prompt

Use this after code is written, before committing.

```
AI Development OS is active.

ROLE:
QA Reviewer

Goal:
Review the changes in [file or branch] and confirm they are correct and safe to commit.

Task type:
Review

Context:
[Describe what was changed and why. What the expected behavior is.]

Scope:
Review only:
- [list files changed]

Permissions:
Read only. Do not edit files unless a clear bug is found.
If a bug is found, describe it and ask for permission before fixing.
Do not commit automatically.

Checks to run:
- git diff -- [filename]
- [list any test commands if known, e.g. npm test, pytest, etc.]

Output only:
- files reviewed
- checks run
- findings (pass / needs changes / needs human review)
- specific issues found
- next recommended step
```

---

## Security Review Prompt

Use this before committing anything that touches secrets, auth, APIs, privacy, or data exposure.

```
AI Development OS is active.

ROLE:
Security Reviewer

Goal:
Review [file or change] for security risks before committing.

Task type:
Review

Context:
[Describe what changed. Mention any sensitive areas: auth, API keys, user data, file uploads, external APIs, environment variables, database access.]

Scope:
Review only:
- [list files]

Permissions:
Read only. Do not edit files.
Do not commit automatically.

Check for:
- Secrets, tokens, or API keys hardcoded or logged
- Auth or permission logic that could be bypassed
- User input that is not validated or sanitized
- Data that could be leaked to logs, responses, or error messages
- External API calls that expose sensitive data
- Environment variables referenced correctly (not hardcoded)
- Any new dependencies that are unverified

Output only:
- files reviewed
- security verdict: pass / needs-changes / needs-human-review
- specific findings with file and line reference
- next recommended step
```

---

## DevOps Prompt

Use this for Docker, server config, DNS, Apache, Caddy, GitHub Actions, deployment, or infrastructure work.

```
AI Development OS is active.

ROLE:
DevOps Reviewer

Goal:
[Describe the infrastructure task — e.g. review the GitHub Actions workflow, check the Docker config, verify the deployment is safe.]

Task type:
[deployment / infrastructure / CI/CD / configuration]

Context:
[Describe the current setup and what you are trying to change or verify. Include environment: staging vs production.]

Scope:
Inspect only:
- [list files: e.g. docker-compose.yml, .github/workflows/deploy.yml, Caddyfile]

Permissions:
Read only unless explicitly stated.
Do not apply changes to production automatically.
Do not commit automatically.

Safety gates:
Stop and ask before:
- Any change that affects production
- Any change that modifies environment variables or secrets
- Any change that could affect uptime or the existing deployment structure

Output only:
- files inspected
- findings
- deployment verdict: safe / unsafe / needs human review
- rollback plan if applicable
- next recommended step
```

---

## Commit Readiness Prompt

Use this when you want to ask Claude if the current changes are safe to commit.

```
AI Development OS is active.

ROLE:
DevOps Reviewer

Goal:
Check whether the current changes are ready to commit safely.

Task type:
Review

Context:
[Describe what was done. What files were changed. What branch you are on.]

Run these checks:
- git status --short
- git diff -- [filename]

Confirm:
- [ ] Changes are on a feature branch, not main or master
- [ ] No secrets or API keys are exposed
- [ ] No unrelated files are changed
- [ ] The commit message follows the convention: fix: / feat: / docs: / refactor: / test: / chore:
- [ ] No failing tests introduced

Output only:
- git status result
- files changed
- commit readiness verdict: ready / not ready / needs human review
- any blockers found
- suggested commit message
- next recommended step
```

---

## Bad Prompt vs Good Prompt Examples

### Example 1 — Vague vs Controlled

**Bad:**
```
Fix the login bug.
```

**Good:**
```
AI Development OS is active.

ROLE:
Developer

Goal:
Fix the login redirect bug where users are sent to /dashboard instead of /home after login.

Task type:
Fix

Context:
The login route is in src/auth/login.js. The redirect on line 42 goes to /dashboard but it should go to /home based on the current spec.

Permissions:
You may edit only:
- src/auth/login.js

Do not edit any other files.
Do not commit automatically.

Output only:
- file changed
- what was fixed
- git diff result
- next recommended step
```

---

### Example 2 — Too open vs Scoped

**Bad:**
```
Review the project for security issues.
```

**Good:**
```
AI Development OS is active.

ROLE:
Security Reviewer

Goal:
Review the authentication middleware for security issues before the next commit.

Task type:
Review

Context:
The middleware was updated yesterday to add JWT validation. The file is src/middleware/auth.js.

Scope:
Review only:
- src/middleware/auth.js

Permissions:
Read only. Do not edit files.
Do not commit automatically.

Output only:
- findings
- security verdict: pass / needs-changes / needs-human-review
- next recommended step
```

---

### Example 3 — No role vs Clear role

**Bad:**
```
Write a README for my project.
```

**Good:**
```
AI Development OS is active.

ROLE:
Documentation Developer

Goal:
Create README.md for the project — a clear, practical overview for a developer audience.

Task type:
Documentation

Context:
The project is a Node.js REST API for managing user subscriptions. It uses Express, PostgreSQL, and JWT auth.

Inspection scope:
Before writing, inspect only:
- package.json
- src/index.js

Permissions:
You may create only:
- README.md

Do not edit any other files.
Do not commit automatically.

Output only:
- files inspected
- file created
- sections written
- next recommended step
```

---

## Memory Rule

When you don't know where to start, use this decision tree:

```
Do I know what the problem is?
├── No  → Use: Analyst
└── Yes → Do I know what to build or create?
          ├── No  → Use: Analyst or Product Manager
          └── Yes → Is it documentation?
                    ├── Yes → Use: Documentation Developer
                    └── No  → Is it a fix?
                              ├── Yes → Use: Developer (Fix Code Prompt)
                              └── No  → Is it a new feature or file?
                                        └── Yes → Use: Developer or Architect (Create Something Prompt)

After Claude finishes any task:
→ Use QA Reviewer before committing.
→ Use Security Reviewer before committing if auth, secrets, or APIs were touched.
→ Use Commit Readiness Prompt to confirm it's safe to commit.
```

**Shortcut:**
- Don't know what's wrong → **Analyst**
- Know what to create → **Create Something**
- Know what to fix → **Fix Code**
- Claude just finished → **QA Reviewer** before committing
