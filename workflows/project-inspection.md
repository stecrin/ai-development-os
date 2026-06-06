# Workflow: Project Inspection

## Purpose

Recover an accurate picture of a project's current state using a minimal, targeted read of its most important files — without scanning the whole repository, loading unnecessary context, or making any changes.

This workflow exists because returning to a project after a break often means an uncertain mental model: you may not remember which branch is active, what task was last in progress, what risks were identified, or what the agreed next step was. This workflow resolves that uncertainty safely.

---

## When to use this workflow

Use this workflow when any of the following are true:

- You are starting a new session on a project you have not touched recently.
- You cannot remember what the current branch, last task, or next step was.
- You want to confirm the project state before instructing Claude to do anything.
- You are entering a project for the first time and have no `PROJECT_STATUS.md` yet.
- You want to understand the scope of a task before approving any changes.

Do not use this workflow if you already have a clear, current picture of the project state. In that case, use the appropriate role directly.

---

## Recommended role

**Analyst**

The Analyst inspects and reports. It does not plan, design, write code, or make changes. Keep this role active for the entire duration of this workflow.

---

## Allowed actions

- Read files explicitly listed in the prompt scope.
- Read files that are directly referenced by those files (e.g. a `CLAUDE.md` that points to a `PROJECT_STATUS.md`).
- Report findings, facts, and open questions.
- Flag risks and assumptions.
- Recommend the first safe next action.

---

## Forbidden actions

- Do not read files that were not listed or directly referenced.
- Do not scan the entire repository or list all files by default.
- Do not use 1M context or load large amounts of unrelated content.
- Do not edit any file.
- Do not create any file (except as the explicit output of a later approved task).
- Do not create a branch.
- Do not commit, push, or open a pull request.
- Do not run scripts, migrations, tests, or build commands.
- Do not assume facts that are not confirmed in the files you read. State assumptions separately and clearly.
- Do not continue past a safety gate without explicit human permission.

---

## Inspection steps

Follow these steps in order. Stop at each gate before continuing.

### Step 1 — Read the project entry point

Read the project's `CLAUDE.md` if it exists. This file holds:
- the active AI authority level
- the project stack and key commands
- the do-not-touch list
- the pointer back to the global framework

If `CLAUDE.md` does not exist, note that and continue to Step 2.

**Gate:** Report what you found in `CLAUDE.md` (or that it is missing) before continuing.

---

### Step 2 — Read project status

Read `PROJECT_STATUS.md` if it exists. This file holds:
- current development phase
- active branch
- last completed task
- next task
- known risks
- test and deployment status
- AI authority level (may override `CLAUDE.md`)

If `PROJECT_STATUS.md` does not exist, note that. It means the project has not been connected to the framework yet, or the session memory was not saved at the end of the last session.

**Gate:** Report what you found in `PROJECT_STATUS.md` (or that it is missing) before continuing.

---

### Step 3 — Read backlog if planning work (optional)

Read `PROJECT_BACKLOG.md` only if you need to understand what tasks are queued or prioritised. Do not read it by default.

**Gate:** Only read this file if the human has indicated they want to plan the next task, not just understand current state.

---

### Step 4 — Read specific files if provided

If the human has listed specific files to inspect (e.g. a source file, a config, a migration), read only those files. Do not read files beyond the provided list.

Do not open large files to skim them. If a file is unusually large, report its size and ask whether to continue before reading.

**Gate:** Confirm the file list with the human before reading if it was not explicitly provided in the prompt.

---

### Step 5 — Check git state (if permitted)

If the human asks for it, or if `PROJECT_STATUS.md` references a branch that may have changed, run:

```
git status --short
git branch --show-current
```

Do not run any other git commands unless explicitly asked.

**Gate:** Do not run `git log`, `git diff`, or any destructive git command without explicit permission.

---

### Step 6 — Report

Produce the inspection report (see Output Format below).

Do not continue to any other task after the report. Wait for the human to decide the next step.

---

## Safety gates

Stop immediately and ask for human permission before:

- Reading any file not listed in the prompt or directly referenced by a listed file.
- Running any command beyond `git status --short` and `git branch --show-current`.
- Touching authentication, authorization, payments, database migrations, environment variables, secrets, production data, deployments, external APIs, or infrastructure.
- Making any edit to any file.
- Creating any branch or commit.
- Continuing if any finding indicates a high-risk situation (e.g. uncommitted secrets, broken migrations, production pointing at wrong branch).

---

## Output format

Always produce the report in this format. Do not omit sections.

```
Files inspected:
- [list each file read, one per line]

Verified facts:
- [facts confirmed directly from file contents — cite the source file for each]

Assumptions:
- [anything inferred but not confirmed in a file — label each one ASSUMPTION]

Open questions:
- [anything that could not be determined from the files inspected]

Risks found:
- [any risks identified — include severity: low / medium / high]

Recommended first safe next action:
- [one specific, concrete action — e.g. "Run project-inspection step 5 to check git branch" or "Activate Developer role and apply the approved fix to src/auth/login.js"]

Human approval required before:
- [list what the next step requires human approval for]
```

---

## Example prompt

Copy this prompt, fill in the bracketed sections, and paste it into Claude Code.

```
AI Development OS is active.

ROLE:
Analyst

Goal:
Inspect the current state of this project and tell me where things stand.
Do not make any changes.

Task type:
Investigation

Context:
I have not worked on this project for [X days / weeks].
I do not remember [what branch I was on / what task was last / what the next step is].

Scope:
Follow the project-inspection workflow.
Inspect only:
- CLAUDE.md
- PROJECT_STATUS.md

If either file is missing, note it and stop at that gate.

Do not read any other files unless they are directly referenced by these files.
Do not scan the whole repository.
Do not use 1M context.

Rules:
Do not edit any file.
Do not create any branch.
Do not commit.
Separate verified facts from assumptions clearly.

Output only:
- files inspected
- verified facts
- assumptions (clearly labelled)
- open questions
- risks found
- recommended first safe next action
- what requires human approval before continuing
```
