# AI Development OS — Quick Start

Use this file when I forget how to start Claude Code properly.

## 1. Default starter prompt

AI Development OS is active.

ROLE:
Analyst

Task:
Help me understand what needs to be done next.

Scope:
Do not scan the whole repository.
Do not use 1M context.
Do not edit files yet.
Inspect only the files needed for the task.

Rules:
Do not touch secrets, .env files, credentials, production data, authentication, payments, deployment, or destructive actions.
Stop and ask before making changes.
Do not commit unless I explicitly approve.

Output only:

* files inspected
* what you found
* risks
* recommended next step
* first safe action

---

## 2. When I want Claude to inspect something

AI Development OS is active.

ROLE:
Analyst

Task:
Inspect the following issue and identify the root cause:

[describe the issue]

Scope:
Read only:

* [file 1]
* [file 2]
* [file 3]

Rules:
Do not edit files.
Do not scan the whole repository.
Do not use 1M context.
Do not commit.

Output only:

* issue found
* affected files
* recommended fix
* risk level
* first safe action

---

## 3. When I want Claude to edit code

AI Development OS is active.

ROLE:
Developer

Task:
Apply the approved fix:

[describe the fix]

Scope:
Read/edit only:

* [file 1]
* [file 2]

Rules:
Do not redesign.
Do not change unrelated files.
Do not scan the whole repository.
Do not use 1M context.
Do not commit.

Output only:

* files modified
* what changed
* how to test
* next step

---

## 4. When I want Claude to check the work

AI Development OS is active.

ROLE:
QA Reviewer

Task:
Review the last changes and check for regressions.

Scope:
Inspect only the changed files.

Rules:
Do not edit files.
Do not commit.
Check whether the change matches the approved task.

Output only:

* pass/fail
* issues found
* risk level
* test steps
* recommendation

---

## 5. When I want a security check

AI Development OS is active.

ROLE:
Security Reviewer

Task:
Review the current changes for security risks.

Scope:
Inspect only the changed files.

Rules:
Do not edit files.
Do not commit.
Check for secrets, .env exposure, unsafe auth changes, data leakage, dangerous file access, and deployment risk.

Output only:

* security pass/fail
* risks found
* severity
* required fix
* recommendation

---

## 6. Simple role guide

Use Analyst when:
I do not know what is wrong yet.

Use Developer when:
The fix is already approved and I want changes made.

Use QA Reviewer when:
I want to check that the fix did not break anything.

Use Security Reviewer when:
The task touches secrets, auth, user data, APIs, deployment, or infrastructure.

Use DevOps Reviewer when:
The task touches server, deployment, Docker, DNS, Apache, Caddy, GitHub Actions, or production.

Use Product Manager when:
I need to decide what to build next.

Use Architect when:
I need to design structure before coding.
