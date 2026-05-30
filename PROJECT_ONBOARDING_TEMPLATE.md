# PROJECT ONBOARDING TEMPLATE

Run this ONCE when entering a new or unknown project under `~/Projects`.
Role: **Analyst**. Output: a filled profile saved into the project (in `CLAUDE.md` or `docs/`),
plus a fresh `PROJECT_STATUS.md` from [PROJECT_STATUS_TEMPLATE.md](./PROJECT_STATUS_TEMPLATE.md).

Do NOT edit code during onboarding. Inspect only. Never open `.env`/secret files.

**Scan exclusions** — skip these folders unless specifically needed:
`node_modules`, `.venv`, `venv`, `__pycache__`, `dist`, `build`, `.git`, `vendor`, `target`, `coverage`, `_backups`, and any other generated or dependency folders.

## 1. Identify the project
- Name / purpose (1 line):
- Repo root path:
- Is it a git repo? Default branch:
- **Stage:** `<discovery / prototype / active development / production / archived>`
- **Priority:** `<high / medium / low>` — Reason: `<why>`
- **AI authority level:** `<read-only / propose-only / may-edit / may-commit / may-deploy-with-approval>`

## 2. Detect the stack (inspect, don't assume)
- Languages & versions:
- Framework(s):
- Package manager / lockfile:
- Key entry points (server, CLI, main app):

## 3. Detect commands (find them; don't guess)
Look in `package.json` scripts, `Makefile`, `pyproject.toml`, CI config, README.
- Install:
- Run / dev:
- Build:
- Test (unit/integration/e2e):
- Lint / format / type-check:
- Security audit (if any):

## 4. Detect conventions
- Branching strategy:
- Commit style:
- Test layout & naming:
- Code style / formatter config:

## 5. Deployment & infra (read-only)
- How is it deployed (CI/CD, scripts, platform)?
- Environments (dev/staging/prod):
- Where config/secrets are referenced (NAMES only):

## 6. Risk scan
- Safety-gate areas present? (auth / payments / migrations / user data / infra):
- Known fragile or untested areas:
- Anything that looks dangerous to change:
- **Production risk level:** `<low / medium / high>` — Why: `<reason>`

## 7. Memory files check
Check whether each file exists in the project root:
- [ ] `CLAUDE.md`
- [ ] `PROJECT_STATUS.md`
- [ ] `PROJECT_SUMMARY.md`
- [ ] `PROJECT_BACKLOG.md`
- [ ] `DECISION_LOG.md`
- [ ] `RETROSPECTIVE.md`

## 8. Lightweight Agile context
(Fill if a backlog or epic structure exists, otherwise skip.)
- **Current epic:** `<large outcome being worked toward>`
- **Current story:** `<the specific task in progress>`
- **Acceptance criteria:** `<how we know the story is done>`
- **Backlog present?** `<yes / no>`

## 9. Outputs
- [ ] Profile saved into the project (stack + commands + conventions)
- [ ] `PROJECT_STATUS.md` created from the status template
- [ ] `CLAUDE.md` present and pointing to the global framework
      (use [PROJECT_CLAUDE_TEMPLATE.md](./PROJECT_CLAUDE_TEMPLATE.md))
- [ ] Memory files check completed (section 7)
- [ ] Open questions for the human listed
- **Recommended next task:**
  - **Action:** `<the single next concrete action>`
  - **Why it matters:** `<impact or unblocking reason>`
  - **Done when:** `<the specific condition that marks it complete>`
