# Workflow: Pre-Commit Review

## Purpose

Run a structured, read-only safety check on staged and unstaged changes before the human commits and pushes them.

This workflow exists because a commit is a point of no easy return — once pushed, secrets, personal data, or unintended changes are visible in the remote history. This review catches problems before they leave the local machine.

Claude does not commit during this workflow. The human reviews the report and decides whether to proceed, fix, or abort.

---

## When to use this workflow

Use this workflow when any of the following are true:

- You are about to run `git commit` and want a final safety check.
- The changes include files you are not fully certain about.
- The repository is public or may become public, and you want to confirm nothing private is staged.
- The changes touch configuration, environment setup, or tooling files.
- You have been working on several things at once and want to confirm only the right files are staged.
- You want a second check on a change that touches auth, secrets, APIs, or personal data.

Do not use this workflow after a commit has already been made. In that case, use the Security Reviewer role or the DevOps Reviewer role to assess the situation.

---

## Recommended role

**QA Reviewer**

This maps to the QA/Test Architect role in [AI_AGENT_RULES.md](../AI_AGENT_RULES.md). In this workflow the role is focused entirely on review — not on writing tests. Keep this role active for the entire duration of the workflow.

---

## Allowed actions

- Run `git status --short`.
- Run `git branch --show-current`.
- Run `git diff`.
- Run `git diff --cached`.
- Read files that appear in the diff if a closer inspection is needed to assess risk.
- Report findings, flag risks, and produce the commit readiness verdict.
- Recommend the commit message format if the changes warrant one.

---

## Forbidden actions

- Do not stage any file.
- Do not run `git add` or `git add -A` or `git add .`.
- Do not commit.
- Do not push.
- Do not run `git reset`, `git restore`, `git checkout`, or `git clean`.
- Do not delete any file.
- Do not edit any file.
- Do not run tests, lint, build, or any script unless the human explicitly approves.
- Do not read files outside the diff or those directly needed to assess a risk in the diff.
- Do not scan the whole repository.
- Do not use 1M context.
- Do not continue past a safety gate without explicit human permission.

---

## Review steps

Follow these steps in order. Stop at each gate before continuing.

### Step 1 — Check working tree status

Run:

```
git status --short
```

Note:
- Which files are staged (marked `A` or `M` in the index column).
- Which files are modified but not staged.
- Which files are untracked.

**Gate:** Report the status output and the count of staged, unstaged, and untracked files before continuing.

---

### Step 2 — Check the active branch

Run:

```
git branch --show-current
```

Check whether the branch is `main` or `master`.

If the project's `CLAUDE.md`, `PROJECT_STATUS.md`, or an explicit human instruction in this session requires feature branches, treat a `main`/`master` branch as a hard blocker and stop.

If no such requirement is in effect (e.g. a personal repository that commits directly to `main`), report the branch as a **caution** and continue. State clearly in the report that the commit will land on `main`/`master` and ask the human to confirm that is intended.

**Gate:** If feature branches are required by project rules or human instruction, stop and report a hard blocker. Otherwise, flag the branch as a caution, confirm with the human, and continue.

---

### Step 3 — Review the staged diff

Run:

```
git diff --cached
```

This shows exactly what would be included in the next commit.

If no files are staged, report that and ask the human whether to continue reviewing the full working tree diff instead.

Read the diff carefully. Look for:
- Files that do not belong in this commit (see Step 5).
- Secrets, tokens, and sensitive data (see Step 6).
- Personal data (see Step 6).
- Private or local-only files (see Step 7).

**Gate:** Report a summary of the staged diff — files changed, lines added/removed, any initial concerns — before continuing.

---

### Step 4 — Review the working tree diff

Run:

```
git diff
```

This shows changes that are modified but not yet staged.

Note any significant unstaged changes the human may have intended to include in this commit but forgot to stage. Report them clearly — do not stage them automatically.

**Gate:** If there are unstaged changes that appear related to the staged work, flag them and ask whether they should be included before committing.

---

### Step 5 — Check for unrelated files

Review the staged file list from Step 3.

For each staged file, ask: does this file clearly belong to the stated purpose of this commit?

Flag any file that:
- Has a different scope than the other staged files (e.g. a config file mixed with a documentation commit).
- Appears to have been staged by accident (e.g. an IDE file, a lock file, a test artifact).
- Is in a directory not related to the change being committed.

**Gate:** Report each unrelated or questionable file and ask whether it should remain staged.

---

### Step 6 — Secret and personal-data check

This is the most critical step. Work through the staged diff from Step 3 and check for each of the following.

**Secrets and credentials:**
- `.env` files or any file with a name matching `.env*`, `*.env`, `*.local`.
- API keys, tokens, bearer tokens, JWT secrets, OAuth secrets.
- Private keys, certificates, PEM files, `.pem`, `.key`, `.p12`, `.pfx`.
- Database connection strings containing usernames, passwords, or hostnames.
- Hardcoded credentials in source code or configuration files.
- SSH keys or known_hosts entries.
- Any string that looks like a secret: long random alphanumeric strings, `sk_live_`, `pk_live_`, `ghp_`, `xoxb-`, `AKIA`, `-----BEGIN`.

**Personal data:**
- Phone numbers (any format: international, local, formatted, raw digits).
- Email addresses that belong to real people (not example.com or placeholder addresses).
- Physical addresses, postal codes, or GPS coordinates linked to a person.
- Full names combined with contact details or identification numbers.
- CV or résumé content, employment history, salary information, compensation notes.
- Job application content, cover letters, interview notes.
- Private financial data: bank account numbers, credit card numbers, tax IDs, salary figures.
- Medical or health information.
- Any data that identifies a real person and was not intended for public release.

**Backup and local-only files:**
- Backup files: `*.bak`, `*.backup`, `*.orig`, `*.old`, `*.tmp`, `~filename`.
- JSON exports, CSV exports, or SQL dumps from local development or staging databases.
- Screenshots or image files that contain personal data, private application states, or internal tooling.
- Files prefixed with `local_`, `dev_`, `tmp_`, `test_`, or `scratch_` that are clearly not intended for the repository.
- IDE configuration files that carry local paths, usernames, or machine-specific settings: `.idea/`, `.vscode/settings.json` (if it contains personal paths), `.DS_Store`.

**Gate:** If any secret, personal data, or private file is found in the staged diff, stop immediately. Report what was found, its location (file name and approximate line), and its severity. Do not continue. Wait for the human to resolve it before the review can proceed.

If nothing is found, state explicitly: "No secrets, personal data, or private files detected in the staged diff."

---

### Step 7 — Public repository check

If the repository is public, or if there is any indication it may be made public (e.g. GitHub remote URL, no `private: true` in `package.json`, no explicit indication in `CLAUDE.md`), apply the following additional checks to the staged diff:

- Internal company names, project codenames, or internal product names not intended for public disclosure.
- Internal URLs, IP addresses, or hostnames (e.g. intranet addresses, staging domain names, internal API endpoints).
- Internal tooling references or infrastructure details.
- Any configuration values specific to a private environment.

If the repository's public/private status is unclear, flag it and ask before continuing.

**Gate:** Report whether the repository appears to be public, private, or unknown. If public or unknown, report any findings from the additional checks above.

---

### Step 8 — README and documentation consistency

If any documentation file was changed in the staged diff (e.g. `README.md`, any `.md` file, any `docs/` file), check whether:

- The change is internally consistent (e.g. a section heading referenced elsewhere still exists).
- The change matches what the rest of the documentation says (e.g. a command listed in the README still matches what the code does).
- A related documentation file should also have been updated but was not staged.

This step produces a recommendation, not a blocker, unless documentation is the explicit purpose of the commit.

**Gate:** If a documentation inconsistency is found, report it and suggest the fix. Do not make the fix automatically. Wait for the human to decide.

---

### Step 9 — Test and lint check

Do not run tests or lint automatically.

Check whether the project defines safe test and lint commands in any of the following:
- `CLAUDE.md` (look for a "Commands" or "Testing" section).
- `package.json` scripts section.
- `Makefile`.
- `pyproject.toml` or `setup.cfg`.
- Any CI configuration file (`.github/workflows/`, `.circleci/`, `Jenkinsfile`).

Report what commands exist and whether they are likely safe to run (i.e. they do not deploy, migrate, or modify production state).

**Gate:** Do not run any test or lint command without explicit human approval. Present the available commands and ask whether to run them.

---

### Step 10 — Generate the verdict and report

Produce the full report (see Output Format below) and end with the commit readiness verdict.

Do not continue to any other action after the report. Wait for the human to decide.

---

## Safety gates

Stop immediately and ask for human permission before:

- Staging, unstaging, or modifying any file.
- Running any command other than `git status --short`, `git branch --show-current`, `git diff`, and `git diff --cached`.
- Running any test, lint, build, or deployment command.
- Touching any area covered by the safety gate list: authentication, authorization, payments, database migrations, production deployment, user data, production data, file uploads, external APIs, webhooks, security policies, permissions, infrastructure, environment variables, secrets.
- Continuing if any finding from Step 6 or Step 7 indicates a hard blocker.

---

## Output format

Always produce the report in this format. Do not omit sections. Do not merge sections.

```
Branch:
- [current branch name]
- [flag if main/master]

Staged files:
- [list each staged file]

Unstaged changes:
- [list files with unstaged changes, or "none"]
- [flag any that appear related to the staged work]

Unrelated files in staging:
- [list any files that appear unrelated to this commit, or "none found"]

Secrets and credentials:
- [findings, with file name and approximate location, or "none detected"]

Personal data:
- [findings, with file name and approximate location, or "none detected"]

Private or local-only files:
- [findings, or "none detected"]

Public repository check:
- Repository status: [public / private / unknown]
- [findings, or "no additional concerns"]

Documentation consistency:
- [findings, or "not applicable" if no documentation was changed]

Test and lint commands available:
- [list commands found, or "none identified"]
- [recommendation on whether to run them]

Commit readiness verdict:
- [SAFE TO COMMIT / SAFE AFTER MINOR FIX / NOT SAFE TO COMMIT]
- Reason: [explain why — one sentence per finding that influenced the verdict]

Suggested commit message:
- [type: short description] — e.g. docs: add pre-commit-review workflow

Human approval required before:
- [list what the next step requires — e.g. "running tests", "resolving staged file X", "committing"]
```

---

## Commit readiness verdicts

### SAFE TO COMMIT

No blockers found. No secrets, no personal data, no private files, no unrelated files. The diff matches the stated purpose of the commit. If the branch is `main`/`master`, the human has confirmed this is intentional.

The human may proceed to commit. Claude does not commit on their behalf.

### SAFE AFTER MINOR FIX

One or more non-critical issues were found that should be resolved before committing, but none are hard blockers. Examples: an unrelated file is staged, a documentation inconsistency was found, an unstaged change appears to belong with this commit.

The human should address the listed issues and then re-run this workflow or commit with awareness of the remaining items.

### NOT SAFE TO COMMIT

A hard blocker was found. Examples: a secret or API key is in the staged diff, personal data is present, a `.env` file is staged, a backup file with sensitive content is staged, or the branch is `main`/`master` and the project rules require a feature branch.

The human must resolve the blocker before any commit is made. Claude will not continue until instructed.

---

## Example prompt

Copy this prompt, fill in the bracketed sections, and paste it into Claude Code.

```
AI Development OS is active.

ROLE:
QA Reviewer

Goal:
Run a pre-commit review on the current staged changes and tell me whether it is safe to commit.
Do not commit, push, stage, or edit any file.

Task type:
Review

Context:
I am about to commit changes to [describe what you changed — e.g. "the documentation workflow files"].
The repository is [public / private / I'm not sure].

Scope:
Follow the pre-commit-review workflow.

Run only:
- git status --short
- git branch --show-current
- git diff --cached
- git diff

Do not run any other commands.
Do not read files outside the diff unless needed to assess a risk.
Do not scan the whole repository.
Do not use 1M context.

Rules:
Do not stage, commit, push, or delete anything.
Do not run tests or lint unless I explicitly approve.
Check for secrets, personal data, private files, unrelated staged files, and branch safety.
Separate each check into its own section.

Output only:
- branch name
- staged files
- unstaged changes
- unrelated files in staging
- secrets and credentials check
- personal data check
- private or local-only files check
- public repository check
- documentation consistency check
- test and lint commands available
- commit readiness verdict (SAFE TO COMMIT / SAFE AFTER MINOR FIX / NOT SAFE TO COMMIT) with reason
- suggested commit message
- what requires human approval before continuing
```
